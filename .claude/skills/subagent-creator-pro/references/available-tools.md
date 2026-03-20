# 사용 가능한 도구 목록

## 코어 도구

| 도구 | 설명 |
|------|------|
| `Read` | 파일 내용 읽기 (이미지, PDF 포함) |
| `Write` | 파일 생성 또는 덮어쓰기 |
| `Edit` | 기존 파일의 특정 부분 수정 |
| `Glob` | 패턴으로 파일 찾기 (`**/*.tsx`) |
| `Grep` | 정규식으로 파일 내용 검색 |
| `Bash` | 셸 명령어 실행 |
| `Agent` | 서브 에이전트 호출. `Agent(이름1, 이름2)` 형태로 허용 에이전트 지정 |

## 상호작용 도구

| 도구 | 설명 |
|------|------|
| `AskUser` | 사용자에게 질문하여 정보 확인 |
| `TodoWrite` | 작업 목록 관리 (진행 상태 추적) |

## 웹 도구

| 도구 | 설명 |
|------|------|
| `WebFetch` | URL에서 콘텐츠 가져와서 분석 |
| `WebSearch` | 웹 검색 |

## IDE 도구 (VS Code 연동 시)

| 도구 | 설명 |
|------|------|
| `mcp__ide__getDiagnostics` | VS Code의 타입/린트 에러 가져오기 |
| `mcp__ide__executeCode` | Jupyter 커널에서 코드 실행 |

## MCP 도구

외부 MCP 서버 연동 시 `mcp__<서버>__<도구>` 패턴으로 사용.

## 추천 도구 조합

### 읽기 전용 분석 (리뷰, 감사)
```yaml
tools: Read, Grep, Glob, Bash
```

### 코드 수정 (구현, 리팩토링)
```yaml
tools: Read, Write, Edit, Grep, Glob, Bash
```

### 최소 접근 (보안 감사)
```yaml
tools: Read, Grep, Glob
```

### 전체 접근
```yaml
# tools 필드를 생략하면 모든 도구 상속
```

### 다른 에이전트 연계
```yaml
tools: Agent(implementer, reviewer), Read, Bash
```