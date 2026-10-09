# Claude Toolkit

Claude Code용 스킬·에이전트 템플릿과 사용 가이드 모음입니다. 어떤 프로젝트에서든 같은 흐름(계획 → 구현 → 리뷰 → 배포)으로 개발할 수 있게 해 줍니다.

## 설치

```bash
# 1. 클론
git clone https://github.com/hgko1207/claude-toolkit.git

# 2. 전역 설치 (~/.claude/에 복사)
cp -r .claude/skills/* ~/.claude/skills/
cp -r .claude/agents/* ~/.claude/agents/
```

설치 후 어떤 프로젝트에서든 바로 사용 가능합니다.

## 들어 있는 것

**스킬** — `-pro`는 작성 가이드·예시·템플릿(references/, assets/)이 함께 들어 있는 버전입니다.

| 스킬 | 하는 일 |
|---|---|
| `/skill-creator` · `/skill-creator-pro` | 새 스킬 만들기 (pro: 작성 가이드 + 예시 5종 + 템플릿) |
| `/subagent-creator` · `/subagent-creator-pro` | 새 서브 에이전트 만들기 (pro: 도구 목록 + 예시 6종 + 템플릿) |
| `/project-init` · `/project-init-pro` | 프로젝트에 CLAUDE.md·plan.md·에이전트·스킬 세팅 |
| `/crystalize` | 긴 프롬프트를 토큰 적게 압축 |
| `/write-note` | 유튜브·블로그 소스를 마크다운 노트로 정리 |

**에이전트**

| 에이전트 | 하는 일 | 모델 |
|---|---|---|
| `@planner` | plan.md에 계획 작성 | Opus |
| `@implementer` | plan.md대로 구현 + 타입체크 | Opus |
| `@reviewer` | 변경사항 리뷰 (수정은 안 함) | Opus |
| `@deployer` | 빌드 → 커밋 → 배포 | Opus |

## 쓰는 흐름

```
@planner 다크모드 추가해줘     → plan.md에 플랜 작성
@implementer 구현해줘          → plan.md 기반 코드 구현 + 타입체크
@reviewer 리뷰해줘             → 변경사항 검토, 문제점 보고
@deployer 배포해줘             → 빌드 → 커밋 → push 배포
```

새 프로젝트는 `/project-init-pro web`으로 CLAUDE.md·plan.md·에이전트를 한 번에 만든 뒤 바로 `@implementer`로 시작하면 됩니다. 프로젝트 전용 에이전트·에이전트 연계는 [docs/customizing.md](docs/customizing.md)를 보세요.

## 가이드

| 폴더 | 내용 |
|---|---|
| [tips/](tips/) | Claude Code 설정·Hooks·스킬·MCP·워크플로·비용 최적화·CLI 레퍼런스, CLAUDE.md 템플릿 ([목차](tips/README.md)) |
| [gstack/](gstack/) | Garry Tan의 gstack(Claude Code를 기획·리뷰·QA·배포 팀처럼 쓰는 명령어 세트) 설치·사용 가이드 |
| [impeccable/](impeccable/) | AI가 만든 티가 나는 UI를 막는 디자인 스킬 Impeccable 설치·명령어 가이드 |

## 참고

### 공식 문서
- [Claude Code 공식 문서](https://code.claude.com/docs)

### 추천 레포 & 도구
- [ykdojo/claude-code-tips](https://github.com/ykdojo/claude-code-tips) — 45개 검증된 Claude Code 팁 모음
- [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) — 커뮤니티 큐레이션 리소스 목록
- [gstack](https://github.com/garrytan/gstack) — Garry Tan의 가상 엔지니어링 팀 스킬 28종
- [ccusage](https://github.com/ryoppippi/ccusage) — 세션별 토큰 사용량 분석 대시보드
- [claude-devtools](https://github.com/matt1398/claude-devtools) — 도구 호출 인스펙터 (DevTools 스타일)
- [ccswarm](https://github.com/nwiizo/ccswarm) — 병렬 Claude 세션 자동화

### 영감
- [monet-registry](https://github.com/monet-design/monet-registry), [cc-system](https://github.com/greatSumini/cc-system)

## 라이선스

MIT — [LICENSE](LICENSE)
