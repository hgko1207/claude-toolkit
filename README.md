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

- [Claude Code 공식 문서](https://docs.anthropic.com/en/docs/claude-code)
- 영감: [monet-registry](https://github.com/monet-design/monet-registry), [cc-system](https://github.com/greatSumini/cc-system)