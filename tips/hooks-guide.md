# Claude Code Hooks 완전 가이드

> 특정 시점에 자동으로 실행되는 커맨드. "Claude가 파일을 수정할 때마다 자동으로 포맷터를 실행해줘" 같은 것들.

---

## 훅이란

Claude Code의 **훅(Hook)**은 특정 이벤트가 발생했을 때 자동으로 실행되는 쉘 명령어다.
Claude한테 부탁하는 게 아니라, Claude가 특정 동작을 할 때 **자동으로 강제 실행**된다는 게 핵심.

예시:

- Claude가 파일을 편집할 때마다 → Prettier 자동 실행
- 대화가 끝날 때마다 → 알림 전송
- 특정 bash 명령 실행 전 → 차단 여부 검증

---

## 모든 훅 이벤트

| 이벤트               | 실행 시점                         |
| -------------------- | --------------------------------- |
| `SessionStart`       | 세션 시작 또는 재시작 시          |
| `UserPromptSubmit`   | 프롬프트 제출 시 (Claude 처리 전) |
| `PreToolUse`         | 도구 실행 직전 — **차단 가능**    |
| `PermissionRequest`  | 권한 다이얼로그 표시 시           |
| `PostToolUse`        | 도구 실행 성공 후                 |
| `PostToolUseFailure` | 도구 실행 실패 후                 |
| `Notification`       | Claude Code가 알림 전송 시        |
| `SubagentStart`      | 서브에이전트 생성 시              |
| `SubagentStop`       | 서브에이전트 종료 시              |
| `Stop`               | Claude 응답 완료 시               |
| `StopFailure`        | API 오류로 응답 중단 시           |
| `PreCompact`         | 컨텍스트 압축 직전                |
| `PostCompact`        | 컨텍스트 압축 완료 후             |
| `TaskCompleted`      | 할 일 완료 표시 시                |
| `SessionEnd`         | 세션 종료 시                      |

---

## 훅 타입

| 타입      | 설명                         |
| --------- | ---------------------------- |
| `command` | 쉘 명령어 실행 (가장 일반적) |
| `http`    | URL에 POST 요청 전송         |
| `prompt`  | Haiku로 단발성 평가          |
| `agent`   | 도구 접근 가능한 멀티턴 검증 |

---

## 훅 종료 코드

| 종료 코드 | 의미                                                       |
| --------- | ---------------------------------------------------------- |
| `0`       | 진행 허용                                                  |
| `2`       | **차단** (stderr에 이유 작성 → Claude에게 피드백으로 전달) |
| 기타      | 진행 허용, stderr 기록됨                                   |

---

## 훅 설정 위치

```json
// .claude/settings.json 또는 ~/.claude/settings.json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "..."
          }
        ]
      }
    ]
  }
}
```

---

## 실전 훅 레시피

### 파일 편집 후 자동 포맷 (Prettier)

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write 2>/dev/null || true"
          }
        ]
      }
    ]
  }
}
```

### 파일 편집 후 자동 포맷 (ESLint)

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx eslint --fix 2>/dev/null || true"
          }
        ]
      }
    ]
  }
}
```

### Claude가 입력 기다릴 때 macOS 알림

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude Code가 당신을 기다리고 있어요\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

### Claude가 입력 기다릴 때 Windows 알림

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -Command \"[Windows.UI.Notifications.ToastNotificationManager, Windows.UI.Notifications, ContentType = WindowsRuntime] > $null; $template = [Windows.UI.Notifications.ToastTemplateType]::ToastText01; $xml = [Windows.UI.Notifications.ToastNotificationManager]::GetTemplateContent($template); $xml.GetElementsByTagName('text')[0].InnerText = 'Claude Code needs attention'; [Windows.UI.Notifications.ToastNotificationManager]::CreateToastNotifier('Claude Code').Show([Windows.UI.Notifications.ToastNotification]::new($xml))\""
          }
        ]
      }
    ]
  }
}
```

### 보호된 파일 편집 차단

```bash
# .claude/hooks/protect-files.sh
#!/bin/bash
INPUT=$(cat)
FILE_PATH=$(echo "$INPUT" | jq -r '.tool_input.file_path // empty')

PROTECTED_PATTERNS=(".env" ".env.local" ".env.production" "package-lock.json" "yarn.lock")

for pattern in "${PROTECTED_PATTERNS[@]}"; do
  if [[ "$FILE_PATH" == *"$pattern"* ]]; then
    echo "차단됨: '$FILE_PATH'는 보호된 파일입니다 (패턴: '$pattern')" >&2
    exit 2
  fi
done

exit 0
```

settings.json에 등록:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/protect-files.sh"
          }
        ]
      }
    ]
  }
}
```

### 압축 후 중요 컨텍스트 재주입

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "compact",
        "hooks": [
          {
            "type": "command",
            "command": "echo '중요: npm 대신 Bun 사용. 커밋 전 bun test 실행. 현재 스프린트: 인증 리팩토링.'"
          }
        ]
      }
    ]
  }
}
```

### Plan 모드 후 자동 승인 (권한 다이얼로그 생략)

```json
{
  "hooks": {
    "PermissionRequest": [
      {
        "matcher": "ExitPlanMode",
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"hookSpecificOutput\": {\"hookEventName\": \"PermissionRequest\", \"decision\": {\"behavior\": \"allow\"}}}'"
          }
        ]
      }
    ]
  }
}
```

### Stop 훅: 모든 작업 완료 여부 검증

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "모든 작업이 완료됐는지 확인해. 미완료 항목이 있으면 {\"ok\": false, \"reason\": \"남은 작업 내용\"} 형식으로 응답해."
          }
        ]
      }
    ]
  }
}
```

### 설정 파일 변경 감사 로그

```json
{
  "hooks": {
    "ConfigChange": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "jq -c '{timestamp: now | todate, source: .source, file: .file_path}' >> ~/claude-config-audit.log"
          }
        ]
      }
    ]
  }
}
```

### TypeScript 파일 편집 후 타입 체크

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "FILE=$(jq -r '.tool_input.file_path'); if [[ \"$FILE\" == *.ts || \"$FILE\" == *.tsx ]]; then npx tsc --noEmit 2>&1 | tail -20; fi"
          }
        ]
      }
    ]
  }
}
```

### bash 명령어 실행 전 위험 명령 차단

```bash
# .claude/hooks/validate-bash.sh
#!/bin/bash
INPUT=$(cat)
COMMAND=$(echo "$INPUT" | jq -r '.tool_input.command // empty')

DANGEROUS_PATTERNS=(
  "rm -rf /"
  "git push --force"
  "git reset --hard"
  "DROP TABLE"
  "DELETE FROM.*WHERE.*1=1"
)

for pattern in "${DANGEROUS_PATTERNS[@]}"; do
  if echo "$COMMAND" | grep -qi "$pattern"; then
    echo "위험한 명령어 차단됨: $COMMAND" >&2
    exit 2
  fi
done

exit 0
```

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/validate-bash.sh"
          }
        ]
      }
    ]
  }
}
```

---

## Matcher 문법

| 패턴              | 매칭 대상                  |
| ----------------- | -------------------------- |
| `Edit\|Write`     | Edit 또는 Write 도구       |
| `Bash`            | Bash 도구 정확 매칭        |
| `mcp__github__.*` | 모든 GitHub MCP 도구       |
| `compact`         | SessionStart의 압축 재시작 |
| `startup`         | SessionStart의 처음 시작   |
| `resume`          | SessionStart의 재개        |
| `""` (빈 문자열)  | 모든 이벤트 매칭           |

---

## 훅 범위별 위치

| 위치                          | 범위             | git 커밋? |
| ----------------------------- | ---------------- | --------- |
| `~/.claude/settings.json`     | 내 모든 프로젝트 | 불필요    |
| `.claude/settings.json`       | 프로젝트 공유    | 커밋      |
| `.claude/settings.local.json` | 개인 로컬        | gitignore |

---

## 핵심 활용 패턴 3가지

### 1. 품질 자동화 (PostToolUse)

코드 작성 → 자동 포맷 → 자동 린트 → 자동 타입체크
개발자가 매번 직접 실행할 필요 없음.

### 2. 안전망 (PreToolUse)

위험한 명령/파일 편집 → 자동 차단 + 이유 전달
실수로 인한 피해를 방지.

### 3. 컨텍스트 재주입 (SessionStart)

압축/재시작 후 → 중요 정보 자동 재주입
Claude가 "까먹는" 것 방지.

---

## 고급 레시피 (커뮤니티 발견)

### 테스트 통과 전 세션 종료 불가 (Stop Hook)

Claude가 "완료했습니다" 하고 끝내려는데 테스트가 실패? 이 Hook을 걸면 테스트 통과 전까지 끝낼 수 없다.

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "npm test 2>&1 || (echo 'Tests are failing. Fix them before stopping.' >&2 && exit 2)"
          }
        ]
      }
    ]
  }
}
```

exit 2 = Claude에게 "끝내지 마, 이거 먼저 해결해" 신호. 솔로 개발에서 QA 자동화의 핵심.

> 출처: [claudefa.st — Stop Hook 가이드](https://claudefa.st/blog/tools/hooks/stop-hook-task-enforcement)

### 키워드 감지 → 스킬 자동 로드 (UserPromptSubmit Hook)

프롬프트에 특정 키워드가 포함되면 관련 스킬을 자동으로 주입. 매번 `/skill-name` 수동 호출할 필요 없음.

```bash
# .claude/hooks/auto-skill-loader.sh
#!/bin/bash
INPUT=$(cat)
PROMPT=$(echo "$INPUT" | jq -r '.prompt // empty')

if echo "$PROMPT" | grep -qi "database\|migration\|schema"; then
  echo "DB 작업 감지됨. DB 가이드 스킬 참고: .claude/skills/db-guide/SKILL.md"
fi

if echo "$PROMPT" | grep -qi "deploy\|배포\|production"; then
  echo "배포 작업 감지됨. 배포 가이드 스킬 참고: .claude/skills/deploy-guide/SKILL.md"
fi

exit 0
```

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/auto-skill-loader.sh"
          }
        ]
      }
    ]
  }
}
```

여러 도메인을 넘나드는 풀스택 개발에서 특히 효과적.

> 출처: [claudefa.st — Hooks 완전 가이드](https://claudefa.st/blog/tools/hooks/hooks-guide)
