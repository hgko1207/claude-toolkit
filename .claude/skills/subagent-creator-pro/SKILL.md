---
name: subagent-creator-pro
description: 고급 서브 에이전트를 생성. references/에서 도구 목록과 예시를 참조하고 assets/의 템플릿을 사용. "에이전트 만들어줘", "프로 에이전트" 요청 시 사용.
argument-hint: [에이전트 이름과 용도]
---

# 서브에이전트 크리에이터 Pro

references/와 assets/를 활용하여 완성도 높은 서브 에이전트를 생성합니다.

## 생성 워크플로우

1. **요구사항 수집**: 에이전트 용도, 트리거 조건, 필요 도구를 사용자에게 확인
2. **범위 결정**: 프로젝트(`.claude/agents/`) 또는 전역(`~/.claude/agents/`)
3. **설정 구성**: name, description, tools, model 결정
   - 도구 선택은 [references/available-tools.md](references/available-tools.md) 참고
4. **시스템 프롬프트 작성**: 역할, 행동, 규칙, 출력 형식 정의
   - 작성 가이드는 아래 "효과적인 에이전트 작성법" 참고
   - 예시는 [references/examples.md](references/examples.md) 참고
5. **파일 생성**: [assets/subagent-template.md](assets/subagent-template.md)를 복사하여 작성

## 파일 형식

```markdown
---
name: 에이전트이름
description: 용도 설명 (자동 위임 시 "use proactively" 포함)
tools: Tool1, Tool2
model: opus
maxTurns: 30
permissionMode: default
skills: skill1, skill2
---

시스템 프롬프트
```

### 프론트매터 필드

| 필드 | 필수 | 설명 |
|------|------|------|
| `name` | O | 소문자+하이픈 |
| `description` | O | 자동 위임 판단 기준. 구체적으로 작성 |
| `tools` | X | 쉼표 구분. 생략 시 전체 상속 |
| `disallowedTools` | X | 차단할 도구 |
| `model` | X | `opus` / `sonnet` / `haiku` / `inherit` |
| `maxTurns` | X | 최대 턴 수 |
| `permissionMode` | X | `default` / `acceptEdits` / `bypassPermissions` / `plan` |
| `skills` | X | 사전 로딩할 스킬 |
| `isolation` | X | `worktree` — git worktree 격리 실행 |

## 효과적인 에이전트 작성법

### description이 가장 중요
Claude가 자동 위임할지 판단하는 유일한 기준:

```yaml
# 좋음 — 구체적 트리거 + proactively
description: 코드 리뷰 전문가. 코드 수정 후 자동으로 사용. Use proactively after code changes.

# 좋음 — 명확한 용도
description: plan.md 기반으로 코드를 구현. "구현해줘" 요청 시 사용.

# 나쁨 — 모호
description: 코드 도움
```

### 시스템 프롬프트 구조

1. **역할 정의**: "You are a [전문 역할]"
2. **호출 시 행동**: 첫 번째로 할 일을 명시
3. **책임 범위**: 무엇을 처리하는지
4. **규칙/제약**: 하지 말아야 할 것
5. **출력 형식**: 결과를 어떤 구조로 반환하는지

### 도구 선택 가이드

- **읽기 전용 분석**: `Read, Grep, Glob, Bash`
- **코드 수정 포함**: `Read, Write, Edit, Grep, Glob, Bash`
- **전체 접근**: `tools` 필드 생략
- **다른 에이전트 호출**: `Agent(에이전트1, 에이전트2)` 추가

상세 도구 목록: [references/available-tools.md](references/available-tools.md)

### 에이전트 연계

```yaml
tools: Agent(reviewer, deployer), Read, Bash
```

에이전트가 다른 에이전트를 호출하여 파이프라인 구성 가능.