# MCP (Model Context Protocol) 서버 설정 가이드

> Claude Code에 외부 도구, 데이터베이스, API 접근 권한을 부여하는 방법입니다

---

## MCP란

MCP 서버는 Claude Code가 **외부 시스템과 연결**할 수 있게 해주는 플러그인입니다.
GitHub, Slack, 데이터베이스, 브라우저 자동화 등을 Claude가 직접 사용할 수 있게 됩니다.

### 토큰 소비 주의사항

- MCP 서버가 많으면 도구 목록이 시스템 프롬프트에 포함되어 토큰을 소비합니다
- Tool Search 기능(Sonnet 4+, Opus 4+에서 자동 활성화) 덕분에 서버 수에 관계없이 ~8,700 토큰으로 유지됩니다
- Tool Search 없이 50개 이상 도구 → 50K~100K 토큰 소비됩니다
- **결론**: 최신 모델에서는 MCP를 많이 설치해도 괜찮습니다. 단, 동시에 켜두는 것은 5~6개로 제한하세요 (오래된 모델 사용 시)

---

## MCP 서버 추가

```bash
# 사용자 범위로 추가 (모든 프로젝트에서 사용 가능)
claude mcp add github --scope user

# 프로젝트 범위로 추가 (.mcp.json에 저장, git 커밋)
claude mcp add postgres --scope project

# 설치된 서버 목록 확인
claude mcp list

# 서버 제거
claude mcp remove github
```

---

## .mcp.json (프로젝트 공유 설정)

```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "playwright": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"]
    },
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "${DATABASE_URL}"]
    }
  }
}
```

---

## 추천 MCP 서버 목록

### 개발 필수

| 서버 | 패키지 | 용도 |
|------|--------|------|
| GitHub | `@modelcontextprotocol/server-github` | 이슈, PR, 코드 검색, 코멘트 |
| Filesystem | `@modelcontextprotocol/server-filesystem` | 구조화된 파일 접근 |
| Git | `@modelcontextprotocol/server-git` | 고급 git 작업 |

### 테스트 & QA

| 서버 | 패키지 | 용도 |
|------|--------|------|
| Playwright | `@playwright/mcp` | 브라우저 자동화, E2E 테스트 |
| Puppeteer | `@modelcontextprotocol/server-puppeteer` | 웹 스크래핑, UI 테스트 |

### 데이터베이스

| 서버 | 패키지 | 용도 |
|------|--------|------|
| PostgreSQL | `@modelcontextprotocol/server-postgres` | DB 쿼리, 스키마 확인 |
| SQLite | `@modelcontextprotocol/server-sqlite` | 로컬 SQLite DB |
| MySQL | `@benborla29/mcp-server-mysql` | MySQL 쿼리 |

### 협업 도구

| 서버 | 패키지 | 용도 |
|------|--------|------|
| Slack | `@modelcontextprotocol/server-slack` | 채널 메시지, DM |
| Notion | `@notionhq/notion-mcp-server` | 문서, 데이터베이스 |
| Linear | `@linear/mcp-server` | 이슈 트래킹 |
| Jira | `@modelcontextprotocol/server-jira` | 이슈/스프린트 관리 |

### 기타

| 서버 | 패키지 | 용도 |
|------|--------|------|
| Figma | `figma-mcp` | 디자인 시스템 통합 |
| AWS | `@aws-labs/mcp` | AWS 서비스 접근 |
| Docker | `docker-mcp` | 컨테이너 관리 |

---

## 전체 설정 예시 (웹 개발팀)

```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "playwright": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"]
    },
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "slack": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-slack"],
      "env": {
        "SLACK_BOT_TOKEN": "${SLACK_BOT_TOKEN}"
      }
    }
  }
}
```

---

## 서브에이전트에 MCP 서버 할당

특정 에이전트에만 MCP 서버를 적용할 수 있습니다:

```markdown
---
name: ui-tester
description: UI 컴포넌트 테스트. 브라우저 자동화가 필요한 테스트 요청 시 사용.
tools: Bash, Read, Edit
mcpServers:
  - playwright:
      type: stdio
      command: npx
      args: ["-y", "@playwright/mcp@latest"]
---

당신은 UI 테스터입니다. Playwright를 사용해 E2E 테스트를 작성하고 실행하세요.
```

---

## MCP 서버 권한 제어

```json
{
  "permissions": {
    "allow": [
      "mcp__github__create_issue",
      "mcp__github__create_pull_request"
    ],
    "deny": [
      "mcp__github__delete_*",
      "mcp__slack__*"
    ],
    "ask": [
      "mcp__github__merge_pull_request"
    ]
  }
}
```

Matcher 패턴: `mcp__<서버이름>__<도구이름>`

---

## 엔터프라이즈 환경 (관리형 설정)

팀 전체에 특정 MCP 서버만 허용합니다:

```json
{
  "allowedMcpServers": [
    { "serverName": "github" },
    { "serverName": "postgres" }
  ],
  "allowManagedMcpServersOnly": true
}
```

---

## 프로젝트 MCP 서버 자동 활성화

```json
{
  "enableAllProjectMcpServers": true
}
```

이 설정을 `~/.claude/settings.json`에 추가하면 `.mcp.json`의 모든 서버가 자동으로 활성화됩니다.

---

## 실전 활용 패턴

### GitHub Issue → 자동 수정 → PR

```text
GitHub 이슈 #456을 읽고, 관련 코드를 수정하고, 자동으로 PR 생성해줘.
```
(GitHub MCP 필요)

### 테스트 실패 시 Slack 알림

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "RESULT=$(jq -r '.tool_response.output'); if echo \"$RESULT\" | grep -q 'FAIL'; then echo '{\"channel\": \"#dev-alerts\", \"text\": \"테스트 실패!\"}' | npx slack-mcp send; fi"
          }
        ]
      }
    ]
  }
}
```

### DB 스키마 자동 문서화

```text
현재 데이터베이스 스키마를 읽어서 docs/schema.md에 마크다운 형식으로 문서화해줘.
```
(PostgreSQL MCP 필요)

### Playwright로 UI 변경 검증

```text
새로 추가한 로그인 폼을 Playwright로 E2E 테스트해줘:
1. 이메일/패스워드 입력
2. 로그인 버튼 클릭
3. 대시보드 리디렉션 확인
```
(Playwright MCP 필요)

---

## 주의사항

1. **환경 변수로 시크릿 관리**: API 키를 .mcp.json에 직접 쓰지 마세요
2. **필요한 것만 켜두기**: 사용하지 않는 서버는 비활성화하세요 (토큰 절약)
3. **.mcp.json 보안 검토**: git에 커밋하기 전 민감 정보를 확인하세요
4. **권한 최소화**: 읽기만 필요하면 쓰기 권한을 차단하세요
