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

## 구성

### 메타 스킬 (스킬/에이전트를 만드는 스킬)

| 스킬 | 호출 | 용도 |
|---|---|---|
| **skill-creator** | `/skill-creator` | 새 스킬 생성 |
| **subagent-creator** | `/subagent-creator` | 새 서브 에이전트 생성 |
| **project-init** | `/project-init` | 새 프로젝트에 Claude 환경 일괄 세팅 |
| **crystalize** | `/crystalize` | 프롬프트 토큰 압축 |

### 범용 에이전트

| 에이전트 | 호출 | 용도 | 모델 |
|---|---|---|---|
| **@planner** | `@planner 기능 추가해줘` | plan.md에 플랜 작성 | Opus |
| **@implementer** | `@implementer 구현해줘` | plan.md 기반 코드 구현 | Opus |
| **@reviewer** | `@reviewer 리뷰해줘` | 코드 리뷰 (수정 불가) | Opus |
| **@deployer** | `@deployer 배포해줘` | 빌드 → 커밋 → 배포 | Opus |

### 워크플로우 예시

```
사용자: @planner 다크모드 추가해줘
  → plan.md에 플랜 작성

사용자: @implementer 구현해줘
  → plan.md 기반으로 코드 구현 + 타입체크

사용자: @reviewer 리뷰해줘
  → 변경사항 검토, 문제점 보고

사용자: @deployer 배포해줘
  → 빌드 → 커밋 → push → Vercel 자동 배포
```

### 새 프로젝트 시작

```
사용자: /project-init web
  → CLAUDE.md, plan.md, 에이전트, 스킬 일괄 생성
  → 바로 @implementer로 구현 시작 가능
```

## 파일 구조

```
claude-toolkit/
├── .claude/
│   ├── skills/
│   │   ├── skill-creator/SKILL.md      # 스킬을 만드는 스킬
│   │   ├── subagent-creator/SKILL.md   # 에이전트를 만드는 스킬
│   │   ├── project-init/SKILL.md       # 프로젝트 초기화
│   │   └── crystalize/SKILL.md         # 프롬프트 압축
│   └── agents/
│       ├── planner.md                  # 플랜 작성
│       ├── implementer.md              # 코드 구현
│       ├── reviewer.md                 # 코드 리뷰
│       └── deployer.md                 # 빌드/배포
└── README.md
```

## 커스터마이징

### 프로젝트별 에이전트 추가

프로젝트의 `.claude/agents/`에 프로젝트 전용 에이전트를 추가하면 전역 에이전트보다 우선 적용됩니다.

### 에이전트 연계

```yaml
# coordinator.md
tools: Agent(implementer, reviewer, deployer), Read
```

에이전트가 다른 에이전트를 호출하여 파이프라인 구성 가능.

## 참고

- [Claude Code 공식 문서](https://docs.anthropic.com/en/docs/claude-code)
- 영감: [monet-registry](https://github.com/monet-design/monet-registry), [cc-system](https://github.com/greatSumini/cc-system)