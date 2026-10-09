# 커스터마이징

## 프로젝트별 에이전트 추가

프로젝트의 `.claude/agents/`에 프로젝트 전용 에이전트를 추가하면 전역 에이전트보다 우선 적용됩니다.

## 에이전트 연계 (파이프라인)

```yaml
# coordinator.md
tools: Agent(implementer, reviewer, deployer), Read
```

에이전트가 다른 에이전트를 호출하여 구현→리뷰→배포 파이프라인 구성 가능.

## Pro 예시에 포함된 에이전트 종류

| 에이전트 | 용도 | 도구 |
|---|---|---|
| code-reviewer | 코드 리뷰 | Read, Grep, Glob, Bash |
| debugger | 버그 디버깅 | Read, Edit, Bash, Grep, Glob |
| test-runner | 테스트 실행/수정 | Bash, Read, Edit, Grep, Glob |
| doc-writer | 기술 문서 작성 | Read, Write, Edit, Glob, Grep |
| security-auditor | 보안 취약점 감사 | Read, Grep, Glob, Bash |
| coordinator | 에이전트 연계 파이프라인 | Agent(...), Read, Bash |

`/subagent-creator-pro`로 이 예시들을 참고하여 커스텀 에이전트를 바로 생성할 수 있습니다.
