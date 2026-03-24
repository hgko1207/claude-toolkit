# Claude Code CLI 명령어 & 단축키 레퍼런스

> 모든 슬래시 명령어, 키보드 단축키, CLI 플래그 빠른 참조

---

## CLI 실행 옵션

```bash
# 기본 인터랙티브 세션
claude

# 단발 쿼리 (비인터랙티브)
claude -p "TypeScript에서 null 체크하는 방법"

# 최근 대화 재개
claude --continue

# 대화 목록에서 선택
claude --resume

# 이름으로 재개
claude --resume auth-refactor

# PR과 연결된 세션 재개
claude --from-pr 123

# 이름 붙여서 시작
claude -n auth-refactor

# 격리된 워크트리에서 시작
claude --worktree feature-name

# Plan 모드로 시작
claude --permission-mode plan

# 특정 에이전트로 세션 실행
claude --agent code-reviewer

# 특정 모델로 실행
claude --model opus

# 디버그 모드
claude --debug
```

---

## 슬래시 명령어 (세션 중)

### 세션 관리

| 명령어 | 설명 |
|--------|------|
| `/clear` | 대화 기록 초기화 |
| `/compact [지침]` | 컨텍스트 압축 |
| `/context` | 컨텍스트 사용량 시각화 |
| `/rename name` | 현재 세션 이름 지정 |
| `/resume` | 세션 선택 피커 열기 |
| `/rewind` | 체크포인트 복원 메뉴 |
| `/branch` | 현재 체크포인트에서 세션 분기 |
| `/exit` | Claude Code 종료 |

### 모델 & 설정

| 명령어 | 설명 |
|--------|------|
| `/model` | 모델 전환 |
| `/effort` | 노력 수준 설정 |
| `/plan` | Plan 모드 진입 |
| `/config` | 설정 인터페이스 열기 |

### 정보 & 디버그

| 명령어 | 설명 |
|--------|------|
| `/help` | 사용 가능한 명령어 표시 |
| `/cost` / `/stats` | 토큰 사용량 표시 |
| `/context` | 컨텍스트 사용량 (색상 그리드) |
| `/diff` | 인터랙티브 diff 뷰어 |
| `/btw 질문` | 사이드 질문 (대화 기록에 추가 안 됨) |
| `/doctor` | 설치 상태 확인 |

### 도구 & 권한

| 명령어 | 설명 |
|--------|------|
| `/permissions` | 권한 규칙 보기/관리 |
| `/hooks` | 설정된 훅 탐색 |
| `/agents` | 서브에이전트 관리 |
| `/add-dir path` | 작업 디렉터리 추가 |

### 메모리 & 초기화

| 명령어 | 설명 |
|--------|------|
| `/init` | 현재 프로젝트의 CLAUDE.md 자동 생성 |
| `/memory` | 메모리 파일 탐색 |

### MCP

| 명령어 | 설명 |
|--------|------|
| `/mcp` | MCP 서버 관리 |

---

## 키보드 단축키

| 단축키 | 동작 |
|--------|------|
| `Shift+Tab` | 권한 모드 사이클 (Normal → Auto-accept → Plan) |
| `Ctrl+G` | 현재 플랜을 텍스트 에디터로 열기 |
| `Ctrl+B` | 실행 중인 작업을 백그라운드로 전환 |
| `Ctrl+O` | Verbose 모드 토글 (사고 과정 표시) |
| `Alt+T` / `Option+T` | Extended thinking 토글 |
| `Esc` | 현재 작업 중단 (컨텍스트 유지) |
| `Esc + Esc` | 체크포인트 복원 메뉴 열기 |
| `Ctrl+C` | 현재 작업 취소 |
| `Ctrl+D` | Claude Code 종료 |

---

## 비인터랙티브 / 스크립팅 옵션

```bash
# JSON 출력
claude -p "분석해줘" --output-format json

# 스트리밍 JSON
claude -p "분석해줘" --output-format stream-json

# 허용 도구 지정
claude -p "수정해줘" --allowedTools "Edit,Bash(git commit *)"

# stdin에서 입력
cat error.log | claude -p "이 오류 설명해줘"

# CI/CD에서 사용
"lint:claude": "claude -p 'main 대비 변경사항에서 오타 보고. 형식: filename:line\\ndescription'"
```

---

## 자주 쓰는 패턴 치트시트

### 새 프로젝트 시작

```bash
cd my-project
claude
> /init
# CLAUDE.md 검토 및 편집
```

### 버그 수정 세션

```bash
claude -n bugfix-auth-timeout
> 이슈 설명...
> # Plan 모드에서 분석
> Shift+Tab 두 번  # Plan 모드
> src/auth/ 읽고 타임아웃 문제 파악해줘
> # Normal 모드로 전환 후 수정
> Shift+Tab 두 번  # Normal 모드
> 수정해줘
```

### 기능 추가 + 리뷰 + 배포

```bash
# 1. 플래닝
claude --permission-mode plan -n feature-dark-mode
> 다크모드 추가 계획 세워줘. plan.md에 작성해줘.

# 2. 구현 (새 세션)
claude -n impl-dark-mode
> plan.md 보고 구현해줘

# 3. 리뷰 (에이전트)
> @code-reviewer 변경사항 리뷰해줘

# 4. 배포
> 빌드하고 커밋하고 PR 생성해줘
```

### 병렬 개발

```bash
# 터미널 1
claude --worktree feature-auth

# 터미널 2
claude --worktree feature-payment

# 터미널 3
claude --worktree bugfix-login
```

---

## 환경 변수 레퍼런스

| 변수 | 설명 |
|------|------|
| `ANTHROPIC_MODEL` | 기본 모델 ID |
| `CLAUDE_CODE_EFFORT_LEVEL` | 기본 노력 수준 |
| `BASH_DEFAULT_TIMEOUT_MS` | Bash 명령 기본 타임아웃 (ms) |
| `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` | 자동 압축 시작 % (기본: 70) |
| `CLAUDE_CODE_ENABLE_TELEMETRY` | 텔레메트리 활성화 (1/0) |
| `ANTHROPIC_API_KEY` | API 키 |

---

## 파일 경로 전체 레퍼런스

```
# 개인 전역 (모든 프로젝트)
~/.claude/settings.json         # 개인 전역 설정
~/.claude/CLAUDE.md             # 개인 전역 지침
~/.claude/agents/               # 개인 전역 서브에이전트
~/.claude/skills/               # 개인 전역 스킬
~/.claude/agent-memory/         # 에이전트 메모리 (user 범위)
~/.claude.json                  # OAuth, MCP 사용자 설정

# 프로젝트 공유 (git 커밋)
.claude/settings.json           # 프로젝트 공유 설정
CLAUDE.md 또는 .claude/CLAUDE.md # 프로젝트 지침
.claude/agents/                 # 프로젝트 서브에이전트
.claude/skills/                 # 프로젝트 스킬
.claude/hooks/                  # Hook 스크립트
.claude/agent-memory/           # 에이전트 메모리 (project 범위)
.mcp.json                       # 프로젝트 MCP 설정

# 개인 로컬 (gitignore)
.claude/settings.local.json     # 개인 로컬 설정
.claude/worktrees/              # Git 워크트리 (gitignore 권장)
.claude/agent-memory-local/     # 에이전트 메모리 (local 범위)
```

---

## MCP 관리 명령어

```bash
# 서버 추가
claude mcp add <name> --scope user    # 개인 전역
claude mcp add <name> --scope project # 프로젝트

# 목록 확인
claude mcp list

# 서버 제거
claude mcp remove <name>
```

---

## 빠른 참조 카드

```
시작:          claude
Plan 모드:     Shift+Tab x2  또는  claude --permission-mode plan
컨텍스트 확인: /context
압축:          /compact
초기화:        /clear
되돌리기:      Esc+Esc
플랜 편집:     Ctrl+G
중단:          Esc
종료:          Ctrl+D  또는  /exit
```
