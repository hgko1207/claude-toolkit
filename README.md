# Claude Toolkit

Claude Code를 위한 범용 스킬 + 에이전트 템플릿 모음.

어떤 프로젝트에서든 동일한 에이전트 워크플로우로 개발할 수 있습니다.

## 설치

```bash
# 1. 클론
git clone https://github.com/hgko1207/claude-toolkit.git

# 2. 전역 설치 (~/.claude/에 복사)
cp -r .claude/skills/* ~/.claude/skills/
cp -r .claude/agents/* ~/.claude/agents/
```

설치 후 어떤 프로젝트에서든 바로 사용 가능합니다.

---

## 구성 요약

### 간편 버전 (Simple)

SKILL.md 하나로 빠르게 사용:

| 스킬 | 호출 | 용도 |
|---|---|---|
| `/skill-creator` | 새 스킬 생성 | SKILL.md 하나 |
| `/subagent-creator` | 새 서브 에이전트 생성 | SKILL.md 하나 |
| `/project-init` | 프로젝트 Claude 환경 세팅 | SKILL.md 하나 |
| `/crystalize` | 프롬프트 토큰 압축 | SKILL.md 하나 |

### Pro 버전

references/, assets/ 포함한 체계적 구성:

| 스킬 | 호출 | 포함 내용 |
|---|---|---|
| `/skill-creator-pro` | 고급 스킬 생성 | 작성 가이드 + 예시 5종 + 스킬 템플릿 |
| `/subagent-creator-pro` | 고급 에이전트 생성 | 도구 목록 + 예시 6종 + 에이전트 템플릿 |
| `/project-init-pro` | 체계적 프로젝트 초기화 | CLAUDE.md 템플릿 + plan.md 템플릿 |

### 범용 에이전트

| 에이전트 | 호출 | 용도 | 모델 |
|---|---|---|---|
| **@planner** | `@planner 기능 추가해줘` | plan.md에 플랜 작성 | Opus |
| **@implementer** | `@implementer 구현해줘` | plan.md 기반 코드 구현 | Opus |
| **@reviewer** | `@reviewer 리뷰해줘` | 코드 리뷰 (수정 불가) | Opus |
| **@deployer** | `@deployer 배포해줘` | 빌드 → 커밋 → 배포 | Opus |

---

## 워크플로우

### 기본 개발 사이클

```
@planner 다크모드 추가해줘     → plan.md에 플랜 작성
@implementer 구현해줘          → plan.md 기반 코드 구현 + 타입체크
@reviewer 리뷰해줘             → 변경사항 검토, 문제점 보고
@deployer 배포해줘             → 빌드 → 커밋 → push 배포
```

### 새 프로젝트 시작

```
/project-init-pro web
  → CLAUDE.md, plan.md, 에이전트, 스킬 일괄 생성
  → 바로 @implementer로 구현 시작 가능
```

### 스킬/에이전트 확장

```
/skill-creator-pro API 연동 스킬    → references/ 포함한 체계적 스킬 생성
/subagent-creator-pro 테스터        → 도구/모델 최적화된 에이전트 생성
```

### 프롬프트 최적화

```
/crystalize 긴 프롬프트...          → 토큰 50%+ 압축
```

---

## gstack — 가상 엔지니어링 팀

Garry Tan (Y Combinator CEO)의 워크플로우. Claude Code를 28개 슬래시 명령어로 구성된 팀으로 만든다.

| 역할 | 명령어 |
|------|--------|
| 기획/검증 | `/office-hours`, `/autoplan` |
| 코드 리뷰 | `/review`, `/codex` |
| QA | `/qa`, `/qa-only`, `/benchmark` |
| 배포 | `/ship`, `/land-and-deploy`, `/canary` |
| 보안 | `/cso`, `/careful`, `/guard` |
| 디자인 | `/design-consultation`, `/design-review` |

→ [gstack/](gstack/) 폴더에서 설치 가이드 + 전체 명령어 상세 가이드 확인

---

## 꿀팁 모음

Claude Code를 더 잘 쓰기 위한 실전 팁 — 입문부터 고급까지 카테고리별 심층 가이드.

### 입문 & 실전

| 파일 | 내용 |
|---|---|
| [pro-tips.md](tips/pro-tips.md) | **고급 꿀팁 15선** — 습관/설정/도구/비용/보안 카테고리별, 초보자도 이해할 수 있게 정리 |
| [design-workflow.md](tips/design-workflow.md) | **AI Slop 극복** — 디자인 스킬 + Gemini 영상 + 템플릿으로 프로급 랜딩 페이지 만들기 |

### 기본

| 파일 | 내용 |
|---|---|
| [claude-code-basics.md](tips/claude-code-basics.md) | 입문~고수 꿀팁 10가지 요약 (CLAUDE.md, MCP, 컨텍스트, 서브 에이전트, Worktree, Hooks) |

### CLAUDE.md 템플릿 (바이브 코딩)

| 파일 | 내용 |
|---|---|
| [CLAUDE-web.md](tips/templates/CLAUDE-web.md) | **웹 앱 템플릿** — Next.js + TypeScript + Tailwind + Supabase 바이브 코딩 설정 |
| [CLAUDE-app.md](tips/templates/CLAUDE-app.md) | **모바일 앱 템플릿** — Expo + React Native + TypeScript 바이브 코딩 설정 |
| [CLAUDE-vibe.md](tips/templates/CLAUDE-vibe.md) | **공통 바이브 코딩 설정** — 스택 무관 공통 원칙 + Claude에게 원하는 것 |

### 심층 가이드 (카테고리별)

| 파일 | 내용 |
|---|---|
| [setup-guide.md](tips/setup-guide.md) | **필수 설정** — CLAUDE.md 작성법, settings.json 완전 가이드, 권한 설정, 보안 설정 |
| [hooks-guide.md](tips/hooks-guide.md) | **Hooks 완전 가이드** — 모든 훅 이벤트, 실전 레시피 10종 (자동 포맷, 알림, 파일 보호, 컨텍스트 재주입 등) |
| [skills-subagents-guide.md](tips/skills-subagents-guide.md) | **Skills & Sub-agents** — 스킬 작성법, 프론트매터 전체 옵션, 에이전트 설계 패턴, 멀티 에이전트 파이프라인 |
| [mcp-guide.md](tips/mcp-guide.md) | **MCP 서버 설정** — 추천 서버 목록, 설정 예시, 권한 제어, 에이전트별 MCP 할당 |
| [workflow-guide.md](tips/workflow-guide.md) | **개발 워크플로우** — Plan 모드 4단계, 컨텍스트 관리, Git Worktree 병렬 개발, 멀티 에이전트 패턴 |
| [optimization-guide.md](tips/optimization-guide.md) | **최적화** — 모델 선택 가이드, 비용 절감 전략, 프롬프트 엔지니어링, 흔한 실수 해결법 |
| [cli-reference.md](tips/cli-reference.md) | **CLI 레퍼런스** — 모든 슬래시 명령어, 키보드 단축키, CLI 플래그, 파일 경로 전체 목록 |

---

## 파일 구조

```
claude-toolkit/
├── .claude/
│   ├── skills/
│   │   ├── skill-creator/              # 간편 스킬 생성
│   │   │   └── SKILL.md
│   │   ├── skill-creator-pro/          # 고급 스킬 생성
│   │   │   ├── SKILL.md
│   │   │   ├── references/
│   │   │   │   ├── writing-guide.md    # 스킬 작성 가이드
│   │   │   │   └── examples.md         # 스킬 예시 5종
│   │   │   └── assets/
│   │   │       └── skill-template/     # 스킬 폴더 템플릿
│   │   ├── subagent-creator/           # 간편 에이전트 생성
│   │   │   └── SKILL.md
│   │   ├── subagent-creator-pro/       # 고급 에이전트 생성
│   │   │   ├── SKILL.md
│   │   │   ├── references/
│   │   │   │   ├── available-tools.md  # 사용 가능 도구 목록
│   │   │   │   └── examples.md         # 에이전트 예시 6종
│   │   │   └── assets/
│   │   │       └── subagent-template.md
│   │   ├── project-init/               # 간편 프로젝트 초기화
│   │   │   └── SKILL.md
│   │   ├── project-init-pro/           # 체계적 프로젝트 초기화
│   │   │   ├── SKILL.md
│   │   │   └── assets/
│   │   │       ├── CLAUDE-template.md  # CLAUDE.md 템플릿
│   │   │       └── plan-template.md    # plan.md 템플릿
│   │   └── crystalize/                 # 프롬프트 압축
│   │       └── SKILL.md
│   └── agents/
│       ├── planner.md                  # 플랜 작성
│       ├── implementer.md              # 코드 구현
│       ├── reviewer.md                 # 코드 리뷰
│       └── deployer.md                 # 빌드/배포
├── tips/
│   ├── pro-tips.md                    # 고급 꿀팁 15선 (습관/설정/도구/비용/보안)
│   ├── design-workflow.md              # AI Slop 극복 디자인 워크플로우
│   ├── claude-code-basics.md           # 입문~고수 꿀팁 10가지 요약
│   ├── setup-guide.md                  # 필수 설정 (CLAUDE.md, settings.json, 권한)
│   ├── hooks-guide.md                  # Hooks 완전 가이드 + 레시피
│   ├── skills-subagents-guide.md       # Skills & Sub-agents 심층 가이드
│   ├── mcp-guide.md                    # MCP 서버 설정 가이드
│   ├── workflow-guide.md               # 개발 워크플로우 (Plan, Context, Worktree)
│   ├── optimization-guide.md           # 모델 선택 + 비용 최적화 + 프롬프트
│   ├── cli-reference.md               # CLI 명령어 + 단축키 레퍼런스
│   └── templates/
│       ├── CLAUDE-web.md              # 웹 앱 바이브 코딩 CLAUDE.md 템플릿
│       ├── CLAUDE-app.md              # 모바일 앱 바이브 코딩 CLAUDE.md 템플릿
│       └── CLAUDE-vibe.md             # 공통 바이브 코딩 CLAUDE.md 템플릿
├── gstack/
│   ├── README.md                      # 전체 명령어 개요
│   ├── install-guide.md               # 설치 가이드 (Windows 트러블슈팅 포함)
│   ├── planning-guide.md              # 기획/플래닝 명령어 가이드
│   ├── development-guide.md           # 개발/디버깅/코드리뷰 가이드
│   ├── design-guide.md                # 디자인 명령어 가이드
│   ├── qa-deploy-guide.md             # QA + 배포 워크플로우 가이드
│   └── workflows.md                   # 실전 워크플로우 조합 패턴
└── README.md
```

---

## 커스터마이징

### 프로젝트별 에이전트 추가

프로젝트의 `.claude/agents/`에 프로젝트 전용 에이전트를 추가하면 전역 에이전트보다 우선 적용됩니다.

### 에이전트 연계 (파이프라인)

```yaml
# coordinator.md
tools: Agent(implementer, reviewer, deployer), Read
```

에이전트가 다른 에이전트를 호출하여 구현→리뷰→배포 파이프라인 구성 가능.

### Pro 예시에 포함된 에이전트 종류

| 에이전트 | 용도 | 도구 |
|---|---|---|
| code-reviewer | 코드 리뷰 | Read, Grep, Glob, Bash |
| debugger | 버그 디버깅 | Read, Edit, Bash, Grep, Glob |
| test-runner | 테스트 실행/수정 | Bash, Read, Edit, Grep, Glob |
| doc-writer | 기술 문서 작성 | Read, Write, Edit, Glob, Grep |
| security-auditor | 보안 취약점 감사 | Read, Grep, Glob, Bash |
| coordinator | 에이전트 연계 파이프라인 | Agent(...), Read, Bash |

`/subagent-creator-pro`로 이 예시들을 참고하여 커스텀 에이전트를 바로 생성할 수 있습니다.

---

## 참고

### 공식 문서
- [Claude Code 공식 문서](https://docs.anthropic.com/en/docs/claude-code)

### 추천 레포 & 도구
- [ykdojo/claude-code-tips](https://github.com/ykdojo/claude-code-tips) — 45개 검증된 Claude Code 팁 모음
- [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) — 커뮤니티 큐레이션 리소스 목록
- [gstack](https://github.com/garrytan/gstack) — Garry Tan의 가상 엔지니어링 팀 스킬 28종
- [ccusage](https://github.com/ryoppippi/ccusage) — 세션별 토큰 사용량 분석 대시보드
- [claude-devtools](https://github.com/matt1398/claude-devtools) — 도구 호출 인스펙터 (DevTools 스타일)
- [ccswarm](https://github.com/nwiizo/ccswarm) — 병렬 Claude 세션 자동화

### 영감
- [monet-registry](https://github.com/monet-design/monet-registry), [cc-system](https://github.com/greatSumini/cc-system)