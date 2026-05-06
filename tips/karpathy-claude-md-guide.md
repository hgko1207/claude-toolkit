# 카파시 CLAUDE.md — GitHub 10만 스타 받은 65줄 파일의 정체

> Andrej Karpathy의 LLM 코딩 통찰을 65줄 마크다운으로 정리한 CLAUDE.md가 GitHub 10만 스타를 돌파했습니다. 4가지 핵심 원칙과 실제 적용 방법까지 정리합니다.

## 핵심 요약

- **카파시(Andrej Karpathy)**가 X에 올린 AI 코딩 3대 고질병 지적이 발단
- 개발자 **Forrest Chang**이 단 하루 만에 65줄 CLAUDE.md로 정리 → **10만+ 스타**
- 핵심은 4가지 원칙: **생각 → 단순함 → 수술적 수정 → 목표 기반 실행**
- 새 기술 없음. "**AI는 못해서 문제가 아니라 브레이크가 없어서 문제**"라는 통찰
- 적용은 30초 — 스킬로 설치하거나 CLAUDE.md 파일을 다운로드

## 목차

- [배경 — 카파시가 지적한 AI의 3대 고질병](#배경--카파시가-지적한-ai의-3대-고질병)
- [원칙 1: Think Before Coding (코딩 전에 생각하라)](#원칙-1-think-before-coding-코딩-전에-생각하라)
- [원칙 2: Simplicity First (단순함이 최우선)](#원칙-2-simplicity-first-단순함이-최우선)
- [원칙 3: Surgical Changes (수술하듯 수정하라)](#원칙-3-surgical-changes-수술하듯-수정하라)
- [원칙 4: Goal-Driven Execution (목표 기반 실행)](#원칙-4-goal-driven-execution-목표-기반-실행)
- [적용 방법 3가지](#적용-방법-3가지)
- [왜 65줄짜리 텍스트가 10만 스타를 받았나](#왜-65줄짜리-텍스트가-10만-스타를-받았나)

---

## 배경 — 카파시가 지적한 AI의 3대 고질병

이 모든 일은 **Andrej Karpathy**(OpenAI 공동창업자, 전 Tesla AI 디렉터)가 X에 올린 짧은 글에서 시작됐습니다. 카파시는 LLM이 코드를 짤 때 반복하는 3가지 문제를 지적했습니다.

| 고질병 | 증상 |
|--------|------|
| **잘못된 가정** | 묻지도 않고 멋대로 해석한 뒤 작업 시작 |
| **코드 부풀리기** | 몇 줄로 될 것을 1,000줄로 짜고, 안 쓰는 코드 방치 |
| **멋대로 수정** | 부탁하지도 않은 코드 변경, 주석 임의 삭제 |

카파시의 결론은 한 문장이었습니다.

> **"명령하지 말고 성공 기준을 주고 지켜보라."**

이 글을 본 개발자 **Forrest Chang**은 그 다음 날 카파시의 메시지를 4가지 원칙으로 추출해 **65줄 CLAUDE.md**로 정리했습니다. 며칠 만에 10만 스타를 돌파했습니다.

> 레포: [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills)

---

## 원칙 1: Think Before Coding (코딩 전에 생각하라)

> *"Don't assume. Don't hide confusion. Surface tradeoffs."*
> 추측하지 마라. 혼란을 숨기지 마라. 트레이드오프를 드러내라.

### 핵심 지침

- 실행 전 가정한 내용을 **명시적으로 밝힌다**
- 여러 해석이 가능하면 **선택하지 말고 모두 제시**한다
- 더 간단한 방법이 있으면 **모두 언급**한다 (필요하면 반박도)
- 이해 안 되면 **멈추고 다시 질문**한다

### Before / After 예시

**요청**: "로그인 기능 추가해 줘"

| 원칙 없음 (Before) | 원칙 적용 (After) |
|--------------------|-------------------|
| "알겠습니다. JWT 기반으로 구현했습니다." → 200줄 우르르 | "인증 방식이 JWT/Session/OAuth 3가지 있습니다. 프로젝트 컨텍스트상 JWT가 적합해 보이는데, 어떤 방식을 선호하시나요?" |
| 멋대로 가정 후 진행 | 트레이드오프 제시 후 질문 |
| 다시 걷어내고 재작성 | 한 번에 올바른 방향 |

**핵심**: AI에게 **선택지를 우리에게 돌려달라고** 명시하는 원칙입니다.

---

## 원칙 2: Simplicity First (단순함이 최우선)

> *"Minimum code that solves the problem. Nothing speculative."*
> 문제를 해결하는 최소한의 코드만. 추측성 코드 금지.

### 핵심 지침

- 요청한 기능 **외에는 추가 기능 없음**
- 일회용 코드에는 **추상화 불필요**
- 요청하지 않은 **유연성/설정 가능성 없음**
- 200줄 짰는데 50줄로 가능하면 **다시 써라**
- 시니어 개발자가 "이거 좀 복잡한데"라고 할 것 같으면 **단순화**

### Before / After 예시

**요청**: "두 숫자를 더하는 함수 만들어 줘"

```typescript
// ❌ 원칙 없음 (Before)
interface Calculator {
  calculate(a: number, b: number): number;
}

class AdditionCalculator implements Calculator {
  calculate(a: number, b: number): number {
    return a + b;
  }
}

const calc: Calculator = new AdditionCalculator();
const result = calc.calculate(1, 2);
```

```typescript
// ✅ 원칙 적용 (After)
function add(a: number, b: number): number {
  return a + b;
}
```

**핵심**: 복잡한 애플리케이션에서 인터페이스 추상화가 유효할 수도 있지만, 그건 **매우 특수한 경우**입니다. 대부분의 경우 단순한 구현이 이해, 토큰 절약, 유지보수 모든 면에서 유리합니다.

---

## 원칙 3: Surgical Changes (수술하듯 수정하라)

> *"Touch only what you must. Clean up only your own mess."*
> 꼭 필요한 것만 건드려라. 본인이 만든 쓰레기만 치워라.

### 핵심 지침

- 변경된 모든 줄이 **사용자 요청에 직접 연결**되어야 함
- 기존 코드 편집 시 근처의 **코드/주석/서식을 임의로 개선하지 말 것**
- 멀쩡한 것을 **굳이 리팩토링하지 말 것**
- 관련 없는 미사용 코드 발견 시 **삭제 말고 알려줄 것**
- 마지막에 "**시킨 것만 변경했는지**" 자체 확인

### Before / After 예시

**요청**: "이 함수의 버그 고쳐 줘"

| 원칙 없음 (Before) | 원칙 적용 (After) |
|--------------------|-------------------|
| 버그 수정 + 주석 스타일 변경 + 근처 함수 리팩토링 + 안 쓰는 import 삭제 | 버그 수정만. "이 함수에 안 쓰이는 import가 있는데 정리할까요?" 물어봄 |
| 리뷰 범위 폭증 | 리뷰 범위 최소 |
| 부수효과로 새 버그 발생 가능 | 변경 범위 명확 |

**핵심**: "**내가 시킨 것만 수정했는가?**" 이 질문 하나로 모든 게 정리됩니다.

---

## 원칙 4: Goal-Driven Execution (목표 기반 실행)

> *"Define success criteria. Loop until verified."*
> 성공 기준을 정의하라. 검증될 때까지 반복하라.

영상에서 **"가장 중요한 원칙"**으로 꼽힌 항목입니다. 카파시의 *"명령하지 말고 성공 기준을 주고 지켜보라"*를 코드 레벨로 풀어낸 것입니다.

### 핵심 지침

- 과제를 **검증 가능한 목표로 전환**한다
- "버그 수정해" → "버그를 재현하는 테스트 작성 후 통과시켜라"
- "리팩토링 해" → "리팩토링 전후 테스트가 통과하는지 확인하라"
- 작업이 여러 단계면 **각 단계에 검증 스텝**을 넣는다

### Before / After 예시

**요청**: "정렬 함수에 버그 있어, 고쳐 줘"

| 원칙 없음 (Before) | 원칙 적용 (After) |
|--------------------|-------------------|
| "인덱스 부분이 잘못된 것 같습니다. 수정했습니다. 완료." | 1️⃣ 실패 케이스로 테스트 작성 |
| 실행하면 버그 그대로 | 2️⃣ 코드 수정 |
| 그럴듯한 부분만 손대고 검증 없이 끝 | 3️⃣ 수정한 게 동작하는지 테스트로 검증 |
|  | 4️⃣ 회귀 확인 (기존 테스트 모두 통과) |

### 검증 가능한 목표의 예시

| 모호한 과제 | 검증 가능한 목표 |
|-------------|------------------|
| 버그 수정해 | 버그 재현 테스트 작성 → 통과시켜라 |
| 리팩토링 해 | 리팩토링 전후 테스트 모두 통과 확인 |
| 성능 개선해 | 벤치마크 작성 → 30% 이상 개선 검증 |
| API 추가해 | 통합 테스트 작성 → 통과 확인 |

**핵심**: "**무엇을 해라**" 대신 "**무엇이 통과되면 끝**"을 명시하는 것입니다.

---

## 적용 방법 3가지

### 방법 1: 스킬로 설치 (Claude Code Skills)

플러그인 마켓플레이스를 통해 스킬로 추가합니다. **명시적으로 호출**해야 적용됩니다.

```bash
# 1. 마켓플레이스에 스킬 추가
/plugin marketplace add forrestchang/andrej-karpathy-skills

# 2. 스킬 설치
/plugin install karpathy-guidelines@andrej-karpathy-skills

# 3. 확인
/ka  # → karpathy-guidelines 스킬이 보이면 OK
```

**장점**: 다른 CLAUDE.md를 덮어쓰지 않음. 필요할 때만 로드 → 컨텍스트 절약.

---

### 방법 2: CLAUDE.md 파일로 다운로드

세션 시작 시 **자동으로 로드**됩니다.

```bash
# 전역 설치 (~/.claude/CLAUDE.md)
curl -o ~/.claude/CLAUDE.md https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md

# 프로젝트만 설치 (./CLAUDE.md)
curl -o ./CLAUDE.md https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md
```

**⚠️ 주의**: 기존 CLAUDE.md를 **덮어씌우므로** 반드시 확인 후 적용하십시오. 기존 내용이 있다면 백업하거나 병합해야 합니다.

---

### 방법 3: 야매 (수동 복사)

```
1. 프로젝트 루트에 새 파일 생성 → CLAUDE.md
2. GitHub에서 원본 복사 → 그대로 붙여넣기
3. 저장
```

**언제 어떤 방법을 쓸까?**

| 방법 | 로드 방식 | 추천 상황 |
|------|-----------|-----------|
| 스킬 | 명시적 호출 | 가끔만 적용하고 싶을 때 |
| CLAUDE.md | 자동 로드 | 매 세션마다 무조건 적용하고 싶을 때 |
| 수동 복사 | 자동 로드 | 명령어 입력 귀찮을 때 |

---

## 65줄 원문 핵심 발췌

```markdown
# CLAUDE.md - Four Behavioral Principles for LLM Coding

## 1. Think Before Coding
"Don't assume. Don't hide confusion. Surface tradeoffs."

## 2. Simplicity First
"Minimum code that solves the problem. Nothing speculative."

## 3. Surgical Changes
"Touch only what you must. Clean up only your own mess."

## 4. Goal-Driven Execution
"Define success criteria. Loop until verified."
```

---

## 왜 65줄짜리 텍스트가 10만 스타를 받았나

새 기술도, 화려한 프레임워크도 없습니다. 그냥 **65줄 마크다운 한 장**입니다. 그런데도 10만 명이 스타를 누른 이유는 한 문장으로 정리됩니다.

> **AI는 코드를 못 짜는 게 아니라 너무 잘 짭니다. 너무 빨리, 너무 자신있게.**
> **문제는 능력 부족이 아니라 브레이크가 없다는 것이었습니다.**

CLAUDE.md는 새 능력을 추가한 게 아니라, **AI에게 브레이크를 달아준 것**입니다. 이 4가지 원칙을 적용했을 때의 효용:

- ✅ 불필요한 변경 사항 감소
- ✅ 과도한 복잡성으로 인한 재작성 감소
- ✅ Claude Code가 구현 전 명확하게 질문함
- ✅ 검증 단계가 자동으로 들어감

**가장 중요한 본질**: "**무엇을 해라**" 대신 "**성공 기준이 무엇이냐**"를 정의하는 사고방식입니다. 이건 AI 시대의 가장 중요한 협업 스킬 중 하나입니다.

---

## 우리 프로젝트의 적용 위치 — 어떻게 합쳐 쓰는가

이 레포의 [tips/templates/](templates/) 폴더에는 바이브 코딩, 웹, 모바일용 CLAUDE.md 템플릿이 있습니다. **카파시 4원칙은 AI 행동 규칙(브레이크)**이고, **템플릿은 작업 환경 설정(스택/명령어/워크플로우)**이라 서로 보완됩니다.

### 추천 방식 — 한 파일로 합치기

프로젝트 루트의 `CLAUDE.md` 하나에 두 내용을 합쳐 넣는 방식이 가장 깔끔합니다. 매 세션 자동 로드되고, 팀 공유도 됩니다.

```markdown
# CLAUDE.md

## AI 행동 원칙 (카파시 4원칙)
1. Think Before Coding — 추측 금지, 트레이드오프 제시
2. Simplicity First — 최소 코드, 추측성 금지
3. Surgical Changes — 시킨 것만 수정
4. Goal-Driven Execution — 검증 가능한 목표로 전환

## 바이브 코딩 워크플로우 (CLAUDE-vibe.md에서 복사)
- 빠르게 작동하는 것 먼저
- 작은 단계로
- ...

## 기술 스택 / 명령어 / 컨벤션 (CLAUDE-web.md 또는 CLAUDE-app.md에서 복사)
- TypeScript, pnpm
- pnpm dev / build / test
- ...
```

### 상황별 추천 조합

| 상황 | 합칠 템플릿 | 결과물 |
|------|-------------|--------|
| 바이브 코딩 (개인 사이드 프로젝트) | 카파시 4원칙 + [CLAUDE-vibe.md](templates/CLAUDE-vibe.md) | 빠른 구현 + 브레이크 |
| Next.js 등 웹 앱 | 카파시 4원칙 + [CLAUDE-web.md](templates/CLAUDE-web.md) | 웹 스택 + 행동 원칙 |
| Expo/RN 모바일 앱 | 카파시 4원칙 + [CLAUDE-app.md](templates/CLAUDE-app.md) | 모바일 스택 + 행동 원칙 |
| 팀 프로젝트 (자동 검증 강제) | 카파시 4원칙 + [hooks-guide.md](hooks-guide.md) | 행동 원칙 + 훅으로 코드 레벨 강제 |

### 분리 방식 (선택지)

특정 작업에서만 4원칙을 강하게 적용하고 싶다면 **카파시는 스킬로, 템플릿은 CLAUDE.md로** 분리할 수도 있습니다.

```
~/.claude/skills/karpathy-guidelines/   ← /ka로 명시 호출
프로젝트루트/CLAUDE.md                    ← 템플릿 내용만 (자동 로드)
```

| 방식 | 추천 상황 |
|------|-----------|
| **합치기** (CLAUDE.md 하나) | 항상 4원칙 적용, 팀 공유 |
| **분리** (스킬 + CLAUDE.md) | 특정 작업에서만 강하게 적용 |

---

**출처**:
- [원본 영상 (YouTube)](https://www.youtube.com/watch?v=gol5jv4wcfs)
- [forrestchang/andrej-karpathy-skills (GitHub, 10만+ 스타)](https://github.com/forrestchang/andrej-karpathy-skills)
- [CLAUDE.md 원문](https://github.com/forrestchang/andrej-karpathy-skills/blob/main/CLAUDE.md)
- 출발점: Andrej Karpathy의 X 포스트 (2026-01-26경)

**작성일**: 2026-04-26
**태그**: ClaudeCode, CLAUDE.md, 카파시, Karpathy, 프롬프트엔지니어링, 코딩규칙, 검증루프, 스킬
