# Skills & Sub-agents 심층 가이드

> 재사용 가능한 워크플로우(Skills)와 전문 AI 팀원(Sub-agents)을 만드는 법입니다

---

## Part 1: Skills — 재사용 가능한 워크플로우

### 스킬이란

스킬은 `/deploy`, `/fix-issue`, `/review` 같은 **슬래시 명령어**로 호출할 수 있는 재사용 가능한 워크플로우입니다.
SKILL.md 파일 하나로 만들 수 있으며, 지원 파일(템플릿, 예시, 스크립트)을 함께 포함할 수 있습니다.

### 파일 구조

```
.claude/skills/
  my-skill/
    SKILL.md          # 필수 — 메인 지침
    template.md       # 선택 — 출력 템플릿
    examples/
      sample.md       # 선택 — 예시 출력
    scripts/
      validate.sh     # 선택 — 실행 스크립트
```

### SKILL.md 프론트매터

```yaml
---
name: fix-issue                    # /fix-issue 슬래시 명령어 생성
description: GitHub 이슈 번호로 버그 수정   # Claude가 자동 호출 판단에 사용
argument-hint: "[issue-number]"    # 자동완성 힌트
disable-model-invocation: true     # true: 사용자만 호출 가능 (Claude 자동 호출 불가)
user-invocable: false              # false: / 메뉴에 숨김 (Claude 전용 백그라운드 스킬)
allowed-tools: Read, Grep, Bash    # 이 스킬 실행 시 자동 허용 도구
model: sonnet                      # 이 스킬 실행 시 사용할 모델
context: fork                      # 격리된 서브에이전트 컨텍스트에서 실행
agent: Explore                     # context: fork 시 사용할 에이전트 타입
---
```

### 호출 제어 매트릭스

| 설정 | 사용자 호출 | Claude 자동 호출 | 컨텍스트 로드 |
|------|------------|-----------------|--------------|
| 기본값 | O | O | description이 컨텍스트에 포함됩니다 |
| `disable-model-invocation: true` | O | X | description 컨텍스트 미포함 |
| `user-invocable: false` | X | O | description 컨텍스트 포함 |

### 인수 치환 변수

| 변수 | 설명 |
|------|------|
| `$ARGUMENTS` | 모든 인수를 하나의 문자열로 전달합니다 |
| `$ARGUMENTS[0]`, `$0` | 첫 번째 인수 |
| `$ARGUMENTS[1]`, `$1` | 두 번째 인수 |
| `${CLAUDE_SESSION_ID}` | 현재 세션 ID |
| `${CLAUDE_SKILL_DIR}` | SKILL.md가 있는 디렉터리 경로 |

### `!` 동적 컨텍스트 주입

```yaml
---
name: pr-summary
description: PR 변경사항 요약
---
## PR 컨텍스트

- PR 변경 내용: !`gh pr diff`
- PR 댓글: !`gh pr view --comments`
- 변경된 파일: !`gh pr diff --name-only`

이 PR을 명확하게 요약해줘.
```

`!`` 문법으로 실행한 명령어 결과가 Claude가 보기 전에 삽입됩니다.

---

### 실전 스킬 예시

#### GitHub 이슈 자동 수정

```yaml
---
name: fix-issue
description: GitHub 이슈 번호로 버그 분석 및 수정
disable-model-invocation: true
allowed-tools: Bash, Read, Edit, Grep, Glob
---
GitHub 이슈 $ARGUMENTS를 분석하고 수정해:

1. `gh issue view $ARGUMENTS`로 이슈 내용 확인
2. 관련 코드 파일 검색
3. 필요한 변경사항 구현
4. 수정 검증을 위한 테스트 작성 및 실행
5. 린트, 타입 체크 통과 확인
6. 이슈를 참조하는 커밋 메시지 작성
7. PR 생성 (`gh pr create --title "fix: #$ARGUMENTS ..."`)
```

#### 배포 스킬

```yaml
---
name: deploy
description: 프로덕션 배포 실행
disable-model-invocation: true
allowed-tools: Bash
---
$ARGUMENTS 환경에 배포:
1. 테스트 스위트 실행: npm test
2. 빌드: npm run build
3. 배포: ./scripts/deploy.sh $ARGUMENTS
4. 헬스 체크 확인: ./scripts/health-check.sh $ARGUMENTS
```

#### 코드 리뷰 요약 (Claude 자동 호출)

```yaml
---
name: review-summary
description: 코드 변경 후 자동으로 리뷰 요점 정리
user-invocable: false
allowed-tools: Read, Grep, Glob
---
최근 변경된 파일들의 리뷰 요약:

변경 내용: !`git diff HEAD~1 --stat`

위 변경사항에 대해:
- 잠재적 버그나 엣지 케이스
- 성능 고려사항
- 보안 이슈
를 간결하게 보고해줘.
```

#### PR 체크리스트

```yaml
---
name: pr-check
description: PR 생성 전 체크리스트 검증
disable-model-invocation: true
allowed-tools: Bash, Read
---
PR 생성 전 다음을 확인해:

현재 변경사항: !`git diff main --stat`

체크리스트:
- [ ] 테스트 통과: `npm test`
- [ ] 타입 체크: `npm run typecheck`
- [ ] 린트 통과: `npm run lint`
- [ ] CHANGELOG 업데이트
- [ ] 관련 이슈 링크

모두 통과하면 `gh pr create` 실행.
```

---

### 스킬 저장 위치

| 위치 | 범위 |
|------|------|
| `~/.claude/skills/<name>/SKILL.md` | 개인 전역 (모든 프로젝트) |
| `.claude/skills/<name>/SKILL.md` | 현재 프로젝트만 |

---

## Part 2: Sub-agents — 전문 AI 팀원

### 서브에이전트란

서브에이전트는 **독립된 컨텍스트 창**에서 실행되는 전문 AI 어시스턴트입니다.
각 에이전트는 고유한 시스템 프롬프트, 허용된 도구 목록, 독립적인 권한을 가집니다.

핵심 차이:
- **스킬**: 현재 대화의 컨텍스트 안에서 실행됩니다
- **서브에이전트**: 완전히 분리된 컨텍스트 창에서 실행됩니다 → 메인 대화 컨텍스트를 보호합니다

### 에이전트 파일 형식

```markdown
---
name: security-reviewer
description: 코드 보안 취약점 검토. 코드 변경 후 자동으로 사용.
tools: Read, Grep, Glob, Bash
disallowedTools: Write, Edit
model: opus
permissionMode: default
maxTurns: 50
memory: project
isolation: worktree
---

당신은 시니어 보안 엔지니어입니다. 다음을 검토하세요:
- SQL 인젝션, XSS, 커맨드 인젝션
- 인증/인가 취약점
- 코드에 포함된 시크릿/자격증명
- 안전하지 않은 데이터 처리

각 취약점에 파일명:라인 번호와 수정 제안을 포함해 보고하세요.
```

### 에이전트 프론트매터 전체 목록

| 필드 | 설명 |
|------|------|
| `name` | 고유 ID (소문자, 하이픈) |
| `description` | Claude가 언제 이 에이전트에 위임할지 판단 기준 |
| `tools` | 허용 도구 목록 (생략 시 전체 상속) |
| `disallowedTools` | 상속 목록에서 제거할 도구 |
| `model` | `sonnet`, `opus`, `haiku`, 또는 전체 모델 ID |
| `permissionMode` | `default`, `acceptEdits`, `plan`, `bypassPermissions` |
| `maxTurns` | 최대 에이전트 턴 수 |
| `skills` | 시작 시 미리 로드할 스킬 목록 |
| `mcpServers` | 이 에이전트 전용 MCP 서버 |
| `hooks` | 이 에이전트 실행 중에만 활성화되는 훅 |
| `memory` | `user`, `project`, `local` — 세션 간 기억 |
| `background` | `true`면 항상 백그라운드 작업으로 실행됩니다 |
| `isolation` | `worktree`면 격리된 git worktree에서 실행됩니다 |

### 에이전트 저장 위치 (우선순위 순)

| 위치 | 범위 | 우선순위 |
|------|------|---------|
| `--agents` CLI 플래그 | 현재 세션 | 1 (최고) |
| `.claude/agents/` | 현재 프로젝트 | 2 |
| `~/.claude/agents/` | 모든 프로젝트 | 3 |

---

### 실전 에이전트 예시

#### 코드 리뷰어 (수정 불가)

```markdown
---
name: code-reviewer
description: 코드 변경사항 품질 검토. 리뷰 요청 시 사용.
tools: Read, Grep, Glob, Bash
disallowedTools: Write, Edit
model: opus
---

당신은 시니어 개발자입니다. 다음을 중점적으로 검토하세요:

1. **타입 안전성**: TypeScript strict 위반, any 타입 오용
2. **코드 품질**: 중복, 불필요한 복잡도, 네이밍
3. **보안**: 입력 검증, 인젝션, 시크릿 노출
4. **성능**: N+1 쿼리, 불필요한 재렌더링, 메모리 누수
5. **테스트**: 커버리지 부족, 빠진 엣지 케이스

파일명:라인번호 형식으로 구체적인 위치와 수정 제안을 제공하세요.
수정은 하지 말고 보고만 하세요.
```

#### 디버거

```markdown
---
name: debugger
description: 버그 분석 및 수정. "버그 있어", "오류 나" 요청 시 사용.
tools: Read, Edit, Bash, Grep, Glob
model: opus
maxTurns: 30
---

당신은 시니어 디버거입니다.

1. 오류 메시지나 현상을 분석해 근본 원인 파악
2. 관련 코드 추적 (스택 트레이스, 로그 분석)
3. 최소한의 수정으로 버그 수정
4. 수정 후 테스트로 검증
5. 같은 버그가 다른 곳에 있는지 확인

절대 증상만 숨기는 workaround는 사용하지 마세요.
```

#### 문서 작성자

```markdown
---
name: doc-writer
description: 기술 문서 작성. "문서 작성해줘", "README 업데이트" 요청 시 사용.
tools: Read, Write, Edit, Glob, Grep
model: sonnet
---

당신은 기술 문서 작가입니다.

개발자를 위한 명확한 문서를 작성하세요:
- 코드 주석은 "무엇"이 아닌 "왜"를 설명
- JSDoc은 타입보다 의도와 사용 예시 중심
- README는 30초 안에 프로젝트를 파악할 수 있게
- API 문서는 각 엔드포인트에 예시 요청/응답 포함

기존 코드 스타일의 문서와 일관성을 유지하세요.
```

#### 테스트 러너

```markdown
---
name: test-runner
description: 테스트 실행 및 실패한 테스트 수정. "테스트 실행", "테스트 고쳐줘" 요청 시 사용.
tools: Bash, Read, Edit, Grep, Glob
model: sonnet
maxTurns: 20
---

당신은 QA 엔지니어입니다.

1. 테스트 스위트 실행
2. 실패 원인 분석 (구현 버그인지, 테스트 버그인지 구분)
3. 구현 버그: 구현 코드 수정
4. 테스트 버그: 테스트 로직 수정 (테스트를 회피하지 말 것)
5. 모든 테스트 통과 확인

테스트를 삭제하거나 skip하는 것은 금지입니다.
```

#### 보안 감사자 (읽기 전용)

```markdown
---
name: security-auditor
description: 보안 취약점 감사. "보안 검토", "취약점 확인" 요청 시 사용.
tools: Read, Grep, Glob, Bash
disallowedTools: Write, Edit
model: opus
---

당신은 보안 전문가입니다. OWASP Top 10 기준으로 검토하세요:

1. **A01 접근 제어 실패**: 인증 없는 기능 접근, IDOR
2. **A02 암호화 실패**: 평문 저장, 취약한 알고리즘
3. **A03 인젝션**: SQL, XSS, 커맨드 인젝션
4. **A07 인증 실패**: 약한 패스워드, 세션 관리 문제
5. **A09 보안 로깅 실패**: 민감 정보 로그 노출

각 취약점: [심각도] 파일명:라인 — 취약점 설명 — 수정 방법
수정은 하지 않고 보고만 합니다.
```

---

### 호출 방법

```text
# 자연어로
security-auditor 에이전트로 auth 모듈을 확인해줘

# @-멘션 (해당 작업에만 강제 위임)
@"security-auditor" auth 변경사항 확인해줘

# 세션 전체를 에이전트 페르소나로 실행
claude --agent code-reviewer
```

---

### 서브에이전트 vs. 메인 대화

**서브에이전트 사용 시:**
- 긴 탐색 작업 (결과가 메인 컨텍스트에 필요 없을 때)
- 특정 도구만 사용해야 할 때 (예: 리뷰어는 Edit 불가)
- 병렬 연구가 필요할 때
- 독립적으로 완결되는 작업

**메인 대화 사용 시:**
- 여러 단계가 컨텍스트를 공유할 때
- 짧은 수정 작업
- 반복적인 피드백이 필요할 때

---

### 멀티 에이전트 파이프라인

```text
# 순차 파이프라인 (PM → 설계 → 개발)
서브에이전트를 순서대로 실행해:
1. pm-spec: GitHub 이슈 #123 읽고 spec.md에 상세 스펙 작성
2. architect-review: 플랫폼 제약 조건으로 설계 검증, ADR 작성
3. implementer-tester: 코드와 테스트 구현, PR 생성

# 병렬 연구
다음 모듈을 병렬로 분석해줘:
- src/auth/ 인증 시스템
- src/db/ 데이터베이스 레이어
- src/routes/ API 라우팅
분석 후 통합 포인트 파악.
```

---

### 에이전트 메모리 (세션 간 기억)

```yaml
---
name: code-reviewer
memory: project   # user, project, local 중 선택
---
```

| 범위 | 저장 위치 |
|------|---------|
| `user` | `~/.claude/agent-memory/<name>/` |
| `project` | `.claude/agent-memory/<name>/` |
| `local` | `.claude/agent-memory-local/<name>/` |

`MEMORY.md` 파일의 처음 200줄이 에이전트 컨텍스트에 자동 주입됩니다.
