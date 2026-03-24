# Claude Code 필수 설정 가이드

> CLAUDE.md, settings.json, 권한 설정 — 모든 프로젝트에서 첫 번째로 해야 할 것들

---

## 1. CLAUDE.md — 세션의 두뇌

Claude Code가 시작될 때 **가장 먼저 읽는 파일**. 이 파일이 Claude의 "작업 지시서"다.

### 파일 위치와 우선순위

```
~/.claude/CLAUDE.md              # 전역 (모든 프로젝트에 적용)
./CLAUDE.md                      # 프로젝트 루트 (팀 공유, git 커밋)
./subdir/CLAUDE.md               # 서브디렉터리 (해당 디렉터리 작업 시 로드)
```

우선순위: 서브디렉터리 > 프로젝트 > 전역

### 무엇을 넣어야 하는가

**넣어야 할 것:**

- Claude가 추측할 수 없는 빌드/실행 명령어
- 프로젝트 고유 코드 스타일 규칙
- 테스트 실행 방법
- 브랜치 네이밍, PR 컨벤션
- 아키텍처 결정 사항
- 개발 환경 특이사항 (필수 환경변수 등)
- Claude가 반복하는 실수 패턴 (발견하면 즉시 추가)

**넣지 말아야 할 것:**

- 코드를 읽으면 알 수 있는 것들
- 표준 언어 컨벤션 (Claude가 이미 알고 있음)
- 자세한 API 문서 (링크로 대체)
- 자주 바뀌는 정보
- 긴 튜토리얼이나 설명
- "깨끗한 코드를 작성해라" 같은 자명한 내용

### 예시 CLAUDE.md

```markdown
# 프로젝트 스택

- Node.js + TypeScript strict mode
- Bun 패키지 매니저 (npm 사용 금지)
- PostgreSQL + Prisma ORM

# 빌드 & 테스트

- 타입 체크: bun run typecheck
- 테스트 실행: bun test --testFile (전체 스위트 실행 금지)
- 빌드: bun run build

# 코딩 컨벤션

- ES modules (import/export), CommonJS 금지
- 가능하면 destructure import 사용
- 컴포넌트: PascalCase, 유틸리티: camelCase

# Git

- 브랜치: feature/_, bugfix/_, chore/\*
- main에 직접 push 금지, 항상 PR 생성
- 커밋 메시지: [타입] 한 줄 요약

# 주의사항

- .env 파일 절대 수정 금지
- migration 파일 직접 편집 금지 (prisma migrate 사용)
```

### 다른 파일 참조 (Import)

```markdown
# 프로젝트 개요

@README.md 참고

# 사용 가능한 npm 명령어

@package.json 참고

# Git 워크플로우

@docs/git-guide.md 참고
```

`@` 문법으로 다른 파일을 참조하면 해당 파일 내용을 컨텍스트에 포함시킨다.

### 유지 관리 원칙

- **200줄 이하 유지**: 200줄 이하 → 92% 이상 규칙 적용. 400줄 초과 → 71%로 급감
- **자주 정리**: 코드처럼 관리. 문제 발생하면 즉시 수정
- **팀 기여**: git에 커밋해서 팀 전체가 함께 관리
- **강조 표시**: 중요한 규칙은 `IMPORTANT:` 또는 `YOU MUST:` 사용
- **자동 생성**: `/init` 명령으로 현재 프로젝트에 맞는 초안 자동 생성

---

## 2. settings.json — 세밀한 동작 제어

### 파일 위치와 우선순위 (높음 → 낮음)

| 파일                          | 범위          | 설명                   |
| ----------------------------- | ------------- | ---------------------- |
| `managed-settings.json`       | 조직 전체     | IT 배포, 덮어쓰기 불가 |
| CLI 인수                      | 현재 세션     | 임시                   |
| `.claude/settings.local.json` | 개인 프로젝트 | gitignore됨            |
| `.claude/settings.json`       | 팀 프로젝트   | git 커밋               |
| `~/.claude/settings.json`     | 개인 전역     | 모든 프로젝트 기본값   |

### 핵심 설정 예시

```json
{
  "model": "claude-sonnet-4-6",
  "permissions": {
    "allow": ["Bash(npm run lint)", "Bash(npm run test *)", "Bash(git commit *)", "Bash(git push origin *)"],
    "deny": ["Bash(curl *)", "Bash(rm -rf *)", "Read(./.env)", "Read(./.env.*)", "Read(./secrets/**)"],
    "ask": ["Bash(rm *)"],
    "defaultMode": "default",
    "additionalDirectories": ["../shared-lib"]
  },
  "env": {
    "BASH_DEFAULT_TIMEOUT_MS": "30000"
  },
  "cleanupPeriodDays": 30,
  "includeCoAuthoredBy": true
}
```

### 권한 모드

| 모드                | 설명                             | 사용 시점        |
| ------------------- | -------------------------------- | ---------------- |
| `default`           | 읽기 허용, 쓰기/실행은 확인      | 일반 작업        |
| `plan`              | 읽기 전용, 파일 수정/실행 불가   | 분석만 필요할 때 |
| `acceptEdits`       | 파일 편집 자동 승인, bash는 확인 | 코딩 작업        |
| `bypassPermissions` | 모든 권한 확인 생략              | 샌드박스 환경만  |

**Plan 모드를 기본값으로 설정 (먼저 계획, 후에 실행):**

```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

### 권한 규칙 평가 순서

**deny → ask → allow** 순으로 평가 — 첫 번째 매칭이 적용됨

### 권한 규칙 문법

```json
"Bash(npm run lint)"      // 정확히 일치
"Bash(npm run test *)"    // 와일드카드
"Read(~/.zshrc)"          // 특정 파일 허용
"Read(./.env*)"           // 패턴 매칭
"Write(production.*)"     // 쓰기 제어
"Agent(Explore)"          // 특정 서브에이전트 허용/차단
```

---

## 3. 필수 보안 설정

### .env와 시크릿 파일 보호 (모든 프로젝트 필수)

```json
{
  "permissions": {
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Read(./.env.local)",
      "Read(./.env.production)",
      "Read(./secrets/**)",
      "Read(./**/credentials*)",
      "Write(./.env*)"
    ]
  }
}
```

### 프로덕션 환경 보호

```json
{
  "permissions": {
    "deny": ["Bash(kubectl delete *)", "Bash(terraform destroy *)", "Bash(DROP TABLE *)"],
    "ask": ["Bash(kubectl *)", "Bash(terraform *)"]
  }
}
```

### bypassPermissions 모드 비활성화 (팀 환경)

```json
{
  "permissions": {
    "disableBypassPermissionsMode": true
  }
}
```

---

## 4. 추가 디렉터리 설정

Claude가 기본적으로 접근하는 범위를 확장:

```json
{
  "permissions": {
    "additionalDirectories": ["../shared-lib", "../common-types", "~/dotfiles"]
  }
}
```

---

## 5. 팀 설정 분리 전략

### 팀 공유 설정 (.claude/settings.json → git 커밋)

```json
{
  "permissions": {
    "allow": ["Bash(npm run lint)", "Bash(npm run test *)", "Bash(npm run build)"],
    "deny": ["Read(./.env*)"]
  }
}
```

### 개인 설정 (.claude/settings.local.json → gitignore)

```json
{
  "model": "claude-opus-4-6",
  "permissions": {
    "allow": ["Bash(git push *)"]
  }
}
```

### .gitignore에 추가

```
.claude/settings.local.json
.claude/worktrees/
.claude/agent-memory-local/
```

---

## 6. 첫 프로젝트 체크리스트

```
□ /init 실행 → CLAUDE.md 자동 생성 후 편집
□ .claude/settings.json 생성 — 팀 공유 권한 설정
□ .env* 파일 deny 목록에 추가
□ git에 CLAUDE.md, settings.json 커밋
□ .gitignore에 settings.local.json 추가
□ ~/.claude/CLAUDE.md에 개인 전역 설정 추가 (편집기, 커밋 스타일 등)
```

---

## 참고 파일 위치 전체 목록

```
~/.claude/settings.json         # 개인 전역 설정
~/.claude/CLAUDE.md             # 개인 전역 지침
~/.claude/agents/               # 개인 전역 서브에이전트
~/.claude/skills/               # 개인 전역 스킬

.claude/settings.json           # 프로젝트 공유 설정 (git 커밋)
.claude/settings.local.json     # 개인 로컬 설정 (gitignore)
.claude/CLAUDE.md               # 프로젝트 지침 (git 커밋)
CLAUDE.md                       # 프로젝트 루트에도 가능
.claude/agents/                 # 프로젝트 서브에이전트
.claude/skills/                 # 프로젝트 스킬
.claude/hooks/                  # Hook 스크립트
.mcp.json                       # 프로젝트 MCP 설정
```
