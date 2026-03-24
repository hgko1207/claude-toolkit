# 개발 워크플로우 심층 가이드

> Plan 모드, 컨텍스트 관리, Git Worktree, 멀티 에이전트 파이프라인

---

## Part 1: Plan 모드 — 먼저 생각, 후에 실행

### 왜 Plan 모드인가

코딩부터 시작하면 잘못된 방향으로 달려가 토큰만 낭비한다.
Plan 모드는 **읽기 전용**으로 동작해서 파일 수정이나 명령 실행 없이 분석만 한다.

**효과**: 플랜 단계에서 실행 단계를 분리하면 토큰 40~60% 절약.

### Plan 모드 활성화 방법

| 방법 | 설명 |
|------|------|
| `Shift+Tab` 두 번 | Normal → Auto-accept → Plan 사이클 |
| `/plan` 명령 | 세션 중 전환 |
| `claude --permission-mode plan` | CLI 실행 시 적용 |
| settings.json 기본값 | `"defaultMode": "plan"` |

### 권장 4단계 워크플로우

#### 1단계: 탐색 (Plan 모드에서)

```text
src/auth 디렉터리를 읽고 세션 관리와 로그인 처리 방식을 파악해줘.
그리고 환경변수를 어떻게 관리하는지도 확인해줘.
```

#### 2단계: 계획 (Plan 모드에서)

```text
Google OAuth를 추가하고 싶어.
어떤 파일들을 수정해야 해? 세션 플로우는 어떻게 되?
plan.md에 작성해줘.
```

`Ctrl+G` → 플랜을 텍스트 에디터에서 직접 편집 가능

#### 3단계: 구현 (Normal 모드로 전환)

```text
플랜대로 OAuth 플로우를 구현해줘.
콜백 핸들러 테스트를 작성하고, 테스트 스위트를 실행해서 실패 시 수정해줘.
```

#### 4단계: 커밋

```text
명확한 메시지로 커밋하고 PR 생성해줘.
```

### Plan 모드를 건너뛰어도 되는 경우

- 범위가 명확하고 수정이 작을 때 (오타, 로그 한 줄, 변수명 변경)
- 변경 사항을 한 문장으로 설명할 수 있을 때

Plan 모드가 필요한 경우:
- 접근 방법이 불확실할 때
- 여러 파일을 동시에 수정할 때
- 낯선 코드를 수정할 때

### plan.md 패턴

```text
이 기능을 구현하기 위한 상세한 plan.md를 작성해줘.
태스크 분해, 의존성, 실행 순서를 포함해줘.
```

---

## Part 2: 컨텍스트 관리 — 우유처럼 신선하게

### 컨텍스트의 한계

Claude의 200K 토큰 컨텍스트 창에는 전체 대화가 들어간다.
대화가 길어질수록 앞 내용이 흐릿해지고 성능이 저하된다.
**컨텍스트 관리가 Claude Code 품질의 1번 요인이다.**

### 핵심 명령어

| 명령어 | 설명 |
|--------|------|
| `/clear` | 컨텍스트 완전 초기화 |
| `/compact [지침]` | 압축 (선택적 지침 포함) |
| `/context` | 현재 토큰 사용량 시각화 |
| `/btw 질문` | 사이드 질문 (대화 기록에 추가 안 됨) |
| `Esc+Esc` 또는 `/rewind` | 체크포인트 복원 메뉴 |

### 압축 전략

```bash
# 기본 압축
/compact

# API 변경사항에 집중해서 압축
/compact API 변경사항에 집중해줘

# 현재 스프린트 목표를 유지하면서 압축
/compact 현재 진행 중인 인증 리팩토링 맥락을 유지해줘
```

**압축 타이밍:**
- 큰 기능 하나를 완성했을 때
- 작업 방향이 바뀔 때
- `/context`로 봤을 때 70% 이상 찼을 때

### CLAUDE.md에 압축 지침 추가

```markdown
## 컨텍스트 압축 시 보존 항목
- 수정된 파일 전체 목록
- 사용한 테스트 명령어
- 현재 스프린트 목표
- 주요 아키텍처 결정 사항
```

### 세션 관리

```bash
# 가장 최근 대화 재개
claude --continue

# 대화 목록에서 선택
claude --resume

# 이름으로 재개
claude --resume auth-refactor

# PR과 연결된 세션 재개
claude --from-pr 123
```

세션 이름 지정:
```bash
claude -n auth-refactor    # CLI에서
/rename auth-refactor      # 세션 중
```

### 체크포인트 활용

- Claude가 모든 액션마다 자동으로 체크포인트 생성
- `Esc+Esc` 또는 `/rewind` → 복원 메뉴
- 대화만, 코드만, 또는 둘 다 복원 가능
- 세션 간에도 유지됨

### 환경 변수로 자동 압축 조절

```bash
# 50% 찼을 때 자동 압축 (기본값보다 더 일찍)
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=50
```

### 컨텍스트 최적화 기법

| 기법 | 토큰 절약 |
|------|----------|
| Plan 모드 (생각/실행 분리) | 40~60% |
| 서브에이전트로 탐색 작업 위임 | 메인 컨텍스트 보호 |
| 무관한 작업 간 `/clear` | 불필요한 노이즈 방지 |
| 집중된 세션 (30~45분) | 정밀도 유지 |
| 파일 읽기 범위 제한 | 불필요한 파일 읽기 방지 |
| 큰 파일 → 작은 집중 파일로 분리 | 파일당 비용 감소 |

---

## Part 3: Git Worktree — 병렬 개발

### 워크트리란

Git worktree는 **같은 저장소에서 여러 작업 디렉터리를 동시에** 만드는 기능이다.
각 디렉터리는 독립적인 브랜치와 파일을 가진다.
Claude Code에서는 각 디렉터리에서 별도 Claude 세션 실행 → **병렬 기능 개발 가능**.

### Claude Code 네이티브 워크트리 지원

```bash
# 이름을 지정해서 워크트리 생성 후 Claude 시작
claude --worktree feature-auth

# 다른 병렬 세션
claude --worktree bugfix-123

# 이름 자동 생성
claude --worktree
```

워크트리는 `.claude/worktrees/<name>`에 생성됨.
`.gitignore`에 `.claude/worktrees/` 추가 필수.

### 수동 워크트리 관리

```bash
# 새 브랜치로 워크트리 생성
git worktree add ../project-feature-a -b feature-a

# 기존 브랜치로 워크트리 생성
git worktree add ../project-bugfix bugfix-123

# 해당 디렉터리에서 Claude 실행
cd ../project-feature-a && claude

# 워크트리 삭제
git worktree remove ../project-feature-a
```

### 워크트리 정리 동작

- **변경 없음**: 워크트리 + 브랜치 자동 삭제
- **변경/커밋 있음**: Claude가 유지 또는 삭제 여부 확인

### 서브에이전트 격리 워크트리

```yaml
---
name: feature-builder
isolation: worktree
---
```

`isolation: worktree`를 설정한 에이전트는 자동으로 격리된 워크트리에서 실행된다.

### /batch 스킬 — 자동화된 병렬 워크트리

```text
/batch src/의 모든 API 핸들러를 async/await로 마이그레이션해줘
```

자동으로:
1. 코드베이스 분석
2. 작업을 5~30개 독립 단위로 분해
3. 각 단위마다 별도 에이전트를 워크트리에서 실행
4. 각 에이전트가 구현 + 테스트 실행 + PR 생성

### 병렬 개발 패턴

```bash
# 터미널 1: 인증 기능
claude --worktree feature-auth

# 터미널 2: 버그 수정
claude --worktree bugfix-456

# 터미널 3: 리팩토링
claude --worktree refactor-api
```

각 Claude 인스턴스는 독립적인 브랜치와 파일 → 충돌 없음.

**주의사항:**
- 각 체크포인트에서 main을 merge해서 드리프트 방지
- 완료 후 워크트리 PR 생성 → merge → 정리

---

## Part 4: 멀티 에이전트 파이프라인

### 패턴 1: 순차 파이프라인

```text
다음 서브에이전트를 순서대로 실행해:
1. pm-spec: GitHub 이슈 #123 읽고 spec.md에 상세 스펙 작성
2. architect-review: 플랫폼 제약 조건으로 설계 검증, ADR 작성
3. implementer-tester: 코드와 테스트 구현, 문서 업데이트, PR 생성
```

### 패턴 2: 병렬 연구

```text
다음 모듈을 서브에이전트를 사용해 병렬로 분석해:
- src/auth/ 인증 시스템
- src/db/ 데이터베이스 레이어
- src/routes/ API 라우팅
분석 완료 후 통합 포인트 파악.
```

### 패턴 3: 작성자/검토자 패턴

```bash
# 세션 A: 구현
claude -n implement-rate-limiter
> API 엔드포인트에 rate limiter 구현해줘

# 세션 B: 검토
claude -n review-rate-limiter
> @src/middleware/rateLimiter.ts 엣지 케이스, 레이스 컨디션, 기존 미들웨어와 일관성 검토해줘

# 세션 A로 돌아와서
> [세션 B 출력 붙여넣기]
이 검토 의견을 반영해줘
```

### 패턴 4: Fan-out 마이그레이션

```bash
# 마이그레이션할 파일 목록 생성
claude -p "마이그레이션 필요한 Python 파일 목록 알려줘" > files.txt

# 병렬 처리
for file in $(cat files.txt); do
  claude -p "Python 2 → 3로 $file 마이그레이션. OK 또는 FAIL로 응답." \
    --allowedTools "Edit,Bash(git commit *)" &
done
wait
```

---

## 권장 워크플로우: 기능 추가 예시

```bash
# 1. Plan 모드로 시작
claude --permission-mode plan

# 2. 탐색
"현재 인증 시스템을 이해해야 해. src/auth/ 읽어봐줘."

# 3. 계획
"Google OAuth를 추가하고 싶어. plan.md에 상세 플랜 작성해줘."
# Ctrl+G로 플랜 검토 및 편집

# 4. Normal 모드로 전환
# Shift+Tab 두 번

# 5. 구현
"plan.md의 플랜대로 구현해줘. 각 단계에서 테스트 실행."

# 6. 코드 리뷰 (선택)
"@code-reviewer 변경사항 리뷰해줘"

# 7. 커밋 및 PR
"커밋 후 PR 생성해줘"
```

---

## 실전 세션 관리 팁

1. **세션을 브랜치처럼 이름 지정**: `"oauth-migration"`, `"memory-leak-debug"`
2. **30~45분 집중 세션**: 긴 방황 세션보다 짧고 집중된 세션이 효과적
3. **각 기능마다 새 브랜치**: 워크트리 또는 세션 격리
4. **검증 인프라에 투자**: 테스트, 린터, 타입체커 — 이게 ROI 배수를 결정
5. **문제 발생 시 즉시 CLAUDE.md 업데이트**: 같은 실수 반복 방지
