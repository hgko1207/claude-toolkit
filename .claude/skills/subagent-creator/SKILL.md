---
name: subagent-creator
description: 새로운 서브 에이전트를 생성하거나 기존 에이전트를 개선. "에이전트 만들어줘" 요청 시 사용.
argument-hint: [에이전트 이름과 용도]
---

# 서브에이전트 크리에이터

Claude Code 서브 에이전트를 생성합니다.

## 에이전트 vs 스킬

|      | 에이전트                    | 스킬                      |
| ---- | --------------------------- | ------------------------- |
| 실행 | 별도 컨텍스트에서 독립 실행 | 현재 대화에 프롬프트 주입 |
| 호출 | `@이름` 또는 자동 위임      | `/이름`                   |
| 도구 | 허용/차단 제어 가능         | 제어 불가                 |
| 모델 | 에이전트별 지정 가능        | 현재 모델 사용            |
| 연계 | 다른 에이전트 호출 가능     | 불가                      |

→ **반복적이고 독립적인 작업** = 에이전트, **지식 주입/워크플로우 가이드** = 스킬

## 파일 위치

- 프로젝트: `.claude/agents/에이전트이름.md`
- 전역: `~/.claude/agents/에이전트이름.md`

## 프론트매터 필드

```yaml
---
name: agent-name # 필수: 소문자+하이픈
description: 용도 설명 # 필수: 자동 위임 판단 기준
tools: Read, Edit, Bash # 선택: 생략 시 전체 도구 상속
disallowedTools: Write # 선택: 차단할 도구
model: opus # 선택: opus/sonnet/haiku/inherit
maxTurns: 30 # 선택: 최대 턴 수
permissionMode: default # 선택: default/acceptEdits/bypassPermissions/plan
skills: skill1, skill2 # 선택: 사전 로딩할 스킬
isolation: worktree # 선택: git worktree 격리 실행
---
시스템 프롬프트 (역할, 작업 흐름, 규칙)
```

## 생성 워크플로우

1. **요구사항 파악**: 에이전트 용도, 필요 도구, 모델 수준 확인
2. **범위 결정**: 프로젝트용(`.claude/agents/`) 또는 전역(`~/.claude/agents/`)
3. **도구 설계**: 읽기만 → `Read, Grep, Glob, Bash` / 수정 포함 → `Read, Edit, Write, Bash, Grep, Glob`
4. **시스템 프롬프트 작성**:
   - 역할을 명확히 정의
   - 호출 시 첫 행동을 지정
   - 단계별 워크플로우 나열
   - 규칙/제약 섹션 분리
5. **파일 생성**: `.md` 파일로 저장

## description 작성 팁

```yaml
# 좋음 - "use proactively" 포함 시 자동 위임
description: 코드 리뷰 전문가. 코드 수정 후 자동으로 사용. Use proactively after code changes.

# 좋음 - 구체적 트리거
description: plan.md 기반으로 코드를 구현하는 에이전트. "구현해줘" 요청 시 사용.

# 나쁨
description: 코드 도움
```

## 에이전트 연계

에이전트가 다른 에이전트를 호출하려면 tools에 명시:

```yaml
tools: Agent(reviewer, deployer), Read, Bash
```
