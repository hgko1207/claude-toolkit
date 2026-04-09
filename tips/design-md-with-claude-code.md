# Claude Code에서 DESIGN.md 활용하기 — 디자인 시스템을 대화 한 줄로 적용하는 법

> DESIGN.md 파일 하나를 프로젝트에 넣으면 Claude Code가 색상, 폰트, 레이아웃을 자동으로 따릅니다. 설치 없이, 설정 없이, 복사 한 번이면 끝입니다.

## 핵심 요약

- DESIGN.md는 **마크다운 파일 하나**입니다. 설치할 것도 설정할 것도 없습니다
- 프로젝트 루트에 놓으면 Claude Code가 **자동으로 읽고** 디자인을 따릅니다
- CLAUDE.md에서 `@DESIGN.md`로 참조하면 **모든 UI 작업에 자동 적용**됩니다
- [awesome-design-md](https://github.com/VoltAgent/awesome-design-md)에서 58개 사이트의 레디메이드 DESIGN.md를 가져올 수 있습니다

---

## 왜 DESIGN.md가 필요한가

Claude Code에게 "대시보드 만들어줘"라고 하면 매번 다른 결과가 나옵니다. 폰트도 다르고, 색상도 다르고, 간격도 제각각입니다. 매번 "이 색상 쓰지 마", "이 폰트로 바꿔줘"라고 지시하는 것은 비효율적입니다.

DESIGN.md는 이 문제를 해결합니다. **한 번 정의해두면 Claude Code가 매 작업마다 자동으로 참조**합니다.

| 방식 | 매번 해야 하는 것 | 결과 일관성 |
|------|:----------------:|:-----------:|
| DESIGN.md **없이** | "폰트는 이거, 색상은 이거, 간격은 이거..." | 낮음 |
| DESIGN.md **있으면** | "대시보드 만들어줘" 한 줄 | 높음 |

---

## Claude Code에서 사용하는 3가지 방법

### 방법 1: 레디메이드 가져오기 (30초)

[awesome-design-md](https://github.com/VoltAgent/awesome-design-md)에서 원하는 사이트의 DESIGN.md를 복사합니다.

```bash
# Vercel 스타일로 시작하고 싶다면
curl -o DESIGN.md https://raw.githubusercontent.com/VoltAgent/awesome-design-md/main/design-md/vercel/DESIGN.md
```

| 원하는 느낌 | 추천 DESIGN.md |
|-----------|---------------|
| 깔끔한 SaaS | `vercel`, `linear`, `resend` |
| 따뜻한 에디토리얼 | `claude`, `notion`, `cal` |
| 프리미엄 다크 | `raycast`, `superhuman`, `cursor` |
| 개발자 도구 | `supabase`, `sentry`, `posthog` |
| 결제/핀테크 | `stripe`, `coinbase`, `revolut` |
| 플레이풀/친근한 | `figma`, `lovable`, `miro` |

### 방법 2: Claude Code에게 만들게 하기

기존 프로젝트가 있다면 Claude Code가 분석해서 DESIGN.md를 생성해줍니다.

```
현재 프로젝트의 디자인 시스템을 분석해서 DESIGN.md로 정리해줘.
색상, 폰트, 컴포넌트 스타일, 스페이싱을 포함해줘.
```

### 방법 3: 레퍼런스 믹스 앤 매치

여러 사이트의 DESIGN.md를 참고해서 새로운 것을 만들 수도 있습니다.

```
Stripe의 색상 팔레트 + Vercel의 레이아웃 + Notion의 타이포그래피를
조합해서 우리 프로젝트용 DESIGN.md를 만들어줘
```

---

## CLAUDE.md와 연결하기

DESIGN.md를 만들었다면 CLAUDE.md에서 참조해야 **모든 작업에 자동 적용**됩니다.

CLAUDE.md에 다음을 추가합니다:

```markdown
## 디자인 시스템

@DESIGN.md 참고. 모든 UI 작업은 이 문서의 컬러, 타이포그래피, 스페이싱 규칙을 따를 것.
```

이렇게 하면:
- 새 페이지를 만들 때 → DESIGN.md의 색상/폰트 자동 적용
- 컴포넌트를 추가할 때 → DESIGN.md의 스타일 자동 적용
- 수정할 때 → DESIGN.md와 일관성 유지

**매번 지시할 필요 없이 자동으로 적용**됩니다.

---

## 실전 프롬프트 예시

### UI 생성

```
DESIGN.md를 참고해서 사용자 설정 페이지를 만들어줘.
사이드바 네비게이션 + 프로필/알림/보안 탭 구성으로.
```

### 기존 UI 리디자인

```
현재 로그인 페이지를 DESIGN.md의 디자인 시스템에 맞게 리디자인해줘.
```

### 컴포넌트 생성

```
DESIGN.md의 버튼 스타일을 기반으로 Button 컴포넌트를 만들어줘.
primary, secondary, ghost 3가지 variant로.
```

### 다크모드 적용

```
DESIGN.md의 다크모드 색상을 적용해서 전체 앱에 다크모드를 추가해줘.
```

---

## Impeccable과 함께 쓰기

DESIGN.md는 **"무엇을 따를지"**를 정하고, Impeccable은 **"잘 따랐는지"**를 검증합니다.

```
1. DESIGN.md 복사 (디자인 방향 설정)
2. CLAUDE.md에 @DESIGN.md 참조 추가
3. 구현
4. /audit (Impeccable — 디자인 품질 점검)
5. /normalize (디자인 토큰 통일)
6. /polish (최종 다듬기)
```

### /teach-impeccable과의 관계

| | DESIGN.md | /teach-impeccable |
|---|---|---|
| **정의하는 것** | 색상 HEX, 폰트 사이즈, 컴포넌트 스타일 (구체적 값) | 브랜드 성격, 타겟 유저, 감성적 방향 (추상적 맥락) |
| **형식** | 마크다운 파일 | 대화형 질문-답변 → .impeccable.md 생성 |

둘 다 있으면 가장 좋습니다. DESIGN.md가 **"어떻게 보여야 하는지"**, .impeccable.md가 **"왜 그렇게 보여야 하는지"**를 담당합니다.

---

## DESIGN.md 안에 들어있는 내용

각 파일은 9개 섹션으로 구성되어 있습니다:

| 섹션 | Claude Code가 활용하는 방식 |
|------|--------------------------|
| Color Palette | 색상 변수 생성, 테마 적용 |
| Typography | 폰트 import, 사이즈/위계 적용 |
| Component Stylings | 버튼/카드/인풋 스타일 직접 적용 |
| Layout Principles | 그리드, 스페이싱, 여백 적용 |
| Depth & Elevation | 그림자, 표면 위계 적용 |
| Do's and Don'ts | 안티패턴 회피 |
| Responsive | 브레이크포인트, 모바일 대응 |
| Agent Prompt Guide | 빠른 참조용 프롬프트 |

---

## 정리

| 질문 | 답변 |
|------|------|
| 설치가 필요한가? | **아닙니다.** 파일 복사가 끝입니다 |
| Claude Code에서만 쓸 수 있는가? | 아닙니다. Cursor, Gemini CLI 등 모든 AI 코딩 도구에서 사용 가능합니다 |
| 직접 만들 수 있는가? | **가능합니다.** Claude Code에게 분석/생성을 시키면 됩니다 |
| 기존 프로젝트에 적용 가능한가? | **가능합니다.** DESIGN.md 복사 후 "이 디자인에 맞게 리디자인해줘" |
| Impeccable과 충돌하는가? | **보완적입니다.** DESIGN.md = 방향, Impeccable = 품질 관리 |

---
**출처**: [Awesome DESIGN.md — VoltAgent](https://github.com/VoltAgent/awesome-design-md)
**작성일**: 2026-04-08
**태그**: DESIGN.md, Claude Code, 디자인시스템, Impeccable, 바이브코딩, UI, 프론트엔드
