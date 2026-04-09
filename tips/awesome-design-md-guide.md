# Awesome DESIGN.md 가이드 — AI에게 디자인 시스템을 복사+붙여넣기로 전달하는 법

> 유명 웹사이트(Vercel, Stripe, Notion 등)의 디자인 시스템을 DESIGN.md 파일로 추출해둔 컬렉션입니다. 프로젝트에 복사하면 AI가 해당 디자인을 그대로 따라 만듭니다.

## 핵심 요약

- **DESIGN.md**는 Google Stitch가 도입한 개념으로, AI가 읽는 **디자인 시스템 문서**입니다
- 58개의 유명 사이트 디자인을 DESIGN.md로 추출해둔 **레디메이드 컬렉션**입니다
- 프로젝트 루트에 복사한 뒤 "이 디자인대로 만들어줘"라고 하면 **픽셀 퍼펙트 UI**가 나옵니다
- Claude Code, Cursor, Gemini CLI 등 **모든 AI 코딩 도구**에서 사용 가능합니다
- Stars **33,588** / Forks **4,177** — 출시 1주일 만에 폭발적 인기를 얻었습니다

---

## DESIGN.md란?

### AGENTS.md vs DESIGN.md

| 파일 | 누가 읽는가 | 무엇을 정의하는가 |
|------|-----------|:----------------:|
| `AGENTS.md` | 코딩 에이전트 | 프로젝트를 **어떻게 빌드**하는지 |
| `DESIGN.md` | 디자인 에이전트 | 프로젝트가 **어떻게 보여야** 하는지 |

DESIGN.md는 **마크다운 파일 하나**입니다. Figma 익스포트, JSON 스키마, 특별한 도구 없이 프로젝트 루트에 넣기만 하면 됩니다. LLM이 가장 잘 읽는 형식이 마크다운이기 때문에 별도 파싱이나 설정이 필요 없습니다.

### Google Stitch에서 시작

[Google Stitch](https://stitch.withgoogle.com/docs/design-md/overview/)에서 처음 도입한 개념입니다. Google Stitch 외에도 Claude Code, Cursor 등 모든 AI 코딩 도구가 DESIGN.md를 읽을 수 있습니다.

---

## DESIGN.md 안에는 무엇이 있는가

모든 파일은 9개 섹션으로 구성되어 있습니다:

| # | 섹션 | 담고 있는 내용 |
|:-:|------|-------------|
| 1 | **Visual Theme & Atmosphere** | 무드, 밀도, 디자인 철학 |
| 2 | **Color Palette & Roles** | 시맨틱 이름 + HEX 코드 + 기능적 역할 |
| 3 | **Typography Rules** | 폰트 패밀리, 전체 위계 테이블 |
| 4 | **Component Stylings** | 버튼, 카드, 인풋, 내비게이션 + 상태별 스타일 |
| 5 | **Layout Principles** | 스페이싱 스케일, 그리드, 여백 철학 |
| 6 | **Depth & Elevation** | 그림자 시스템, 표면 위계 |
| 7 | **Do's and Don'ts** | 디자인 가드레일, 안티패턴 |
| 8 | **Responsive Behavior** | 브레이크포인트, 터치 타겟, 접기 전략 |
| 9 | **Agent Prompt Guide** | 빠른 색상 참조, 바로 쓸 수 있는 프롬프트 |

### 각 사이트에 포함된 파일

| 파일 | 용도 |
|------|------|
| `DESIGN.md` | 디자인 시스템 (AI가 읽는 파일) |
| `preview.html` | 색상, 타이포, 버튼, 카드를 시각적으로 보여주는 카탈로그 |
| `preview-dark.html` | 다크모드 버전 카탈로그 |

---

## 사용법

### 기본 사용 (2단계)

```bash
# 1. 원하는 사이트의 DESIGN.md를 프로젝트 루트에 복사
cp path/to/awesome-design-md/design-md/vercel/DESIGN.md ./DESIGN.md

# 2. AI에게 지시
```

```
이 DESIGN.md를 참고해서 대시보드 페이지를 만들어줘
```

이것이 전부입니다. AI가 DESIGN.md의 색상, 폰트, 컴포넌트 스타일을 그대로 따라 만듭니다.

### Claude Code에서 사용하기

```bash
# 방법 1: 직접 복사
git clone https://github.com/VoltAgent/awesome-design-md.git /tmp/awesome-design-md
cp /tmp/awesome-design-md/design-md/stripe/DESIGN.md ./DESIGN.md

# 방법 2: CLAUDE.md에서 참조
echo "## 디자인 시스템\n@DESIGN.md 참고" >> CLAUDE.md
```

CLAUDE.md에서 `@DESIGN.md`로 참조하면, Claude Code가 모든 UI 작업 시 자동으로 디자인 시스템을 따릅니다.

### Impeccable과 함께 사용하기

Awesome DESIGN.md와 Impeccable은 **보완적**입니다:

| | Awesome DESIGN.md | Impeccable |
|---|---|---|
| **역할** | "**이 사이트처럼** 만들어줘" (레퍼런스) | "**잘** 만들어줘" (품질 관리) |
| **제공하는 것** | 색상, 폰트, 컴포넌트의 구체적 값 | 디자인 원칙, 안티패턴 체크 |
| **사용 시점** | 프로젝트 시작 시 (방향 설정) | 구현 후 (품질 점검) |

```
DESIGN.md 복사 (Vercel 스타일로 시작)
  → 구현
  → /audit (Impeccable으로 품질 점검)
  → /normalize → /polish (수정)
```

---

## 58개 사이트 컬렉션

### AI & 머신러닝 (12개)

| 사이트 | 특징 |
|--------|------|
| **Claude** | 따뜻한 테라코타 액센트, 에디토리얼 레이아웃 |
| **Cohere** | 생동감 있는 그라데이션, 데이터 대시보드 |
| **ElevenLabs** | 다크 시네마틱 UI, 오디오 웨이브폼 |
| **Mistral AI** | 프렌치 미니멀리즘, 퍼플 톤 |
| **Ollama** | 터미널 퍼스트, 모노크롬 |
| **Replicate** | 클린 화이트, 코드 포워드 |
| **RunwayML** | 시네마틱 다크, 미디어 리치 |
| **Together AI** | 블루프린트 스타일 |
| **VoltAgent** | 보이드 블랙, 에메랄드 액센트 |
| **xAI** | 스타크 모노크롬, 미래적 미니멀리즘 |
| **Minimax** | 볼드 다크, 네온 액센트 |
| **OpenCode AI** | 개발자 중심 다크 테마 |

### 개발자 도구 (14개)

| 사이트 | 특징 |
|--------|------|
| **Vercel** | 블랙 앤 화이트, Geist 폰트 |
| **Cursor** | 슬릭 다크, 그라데이션 액센트 |
| **Linear** | 울트라 미니멀, 퍼플 액센트 |
| **Supabase** | 다크 에메랄드, 코드 퍼스트 |
| **Stripe** | 시그니처 퍼플, 엘레강스 |
| **Raycast** | 슬릭 다크 크롬, 비비드 그라데이션 |
| **Resend** | 미니멀 다크, 모노스페이스 |
| **Sentry** | 다크 대시보드, 핑크-퍼플 |
| **PostHog** | 플레이풀, 개발자 친화 다크 |
| **Expo** | 다크, 코드 중심 |
| **Lovable** | 플레이풀 그라데이션 |
| **Mintlify** | 클린 그린, 가독성 최적화 |
| **Superhuman** | 프리미엄 다크, 키보드 퍼스트 |
| **Warp** | IDE 스타일, 블록 기반 |

### 디자인 & 생산성 (10개)

| 사이트 | 특징 |
|--------|------|
| **Notion** | 따뜻한 미니멀리즘, 세리프 헤딩 |
| **Figma** | 멀티컬러, 플레이풀 |
| **Framer** | 볼드 블랙+블루, 모션 퍼스트 |
| **Webflow** | 블루 액센트, 폴리시드 |
| **Miro** | 브라이트 옐로, 무한 캔버스 |
| **Airtable** | 컬러풀, 데이터 미학 |
| **Cal.com** | 클린 뉴트럴, 개발자 지향 |
| **Clay** | 오가닉, 소프트 그라데이션 |
| **Intercom** | 프렌들리 블루, 대화형 UI |
| **Pinterest** | 레드 액센트, 매이슨리 그리드 |

### 핀테크 & 엔터프라이즈 & 자동차 (22개)

Coinbase, Kraken, Revolut, Wise, Airbnb, Apple, IBM, NVIDIA, SpaceX, Spotify, Uber, Zapier, BMW, Ferrari, Lamborghini, Renault, Tesla 등이 포함되어 있습니다.

---

## 실전 활용 시나리오

### 시나리오 1: "Vercel 같은 느낌의 SaaS 대시보드를 만들고 싶다"

```bash
cp /tmp/awesome-design-md/design-md/vercel/DESIGN.md ./DESIGN.md
```

```
DESIGN.md를 참고해서 사용자 대시보드 페이지를 만들어줘.
사이드바 네비게이션 + 메인 콘텐츠 영역 + 통계 카드 구성으로.
```

### 시나리오 2: "Notion 스타일의 문서 편집기를 만들고 싶다"

```bash
cp /tmp/awesome-design-md/design-md/notion/DESIGN.md ./DESIGN.md
```

```
DESIGN.md를 참고해서 블록 기반 문서 편집기 UI를 만들어줘.
```

### 시나리오 3: "여러 사이트를 참고해서 나만의 디자인을 만들고 싶다"

```
Stripe의 색상 팔레트 + Vercel의 레이아웃 + Notion의 타이포그래피를
조합해서 DESIGN.md를 새로 만들어줘
```

AI에게 여러 DESIGN.md를 참고하라고 지시하면 믹스 앤 매치도 가능합니다.

---

## 새로운 DESIGN.md 요청하기

원하는 사이트의 DESIGN.md가 없다면 [GitHub Issue](https://github.com/VoltAgent/awesome-design-md/issues/new?template=design-md-request.yml)에서 요청할 수 있습니다.

---

## gstack, Impeccable과의 관계 정리

| 도구 | 역할 | 언제 쓰는가 |
|------|------|-----------|
| **Awesome DESIGN.md** | 디자인 방향 설정 (레퍼런스) | 프로젝트 시작 시 |
| **Impeccable** | 디자인 품질 관리 (20개 명령어) | 구현 후 점검 |
| **gstack** | 전체 개발 워크플로우 | 기획→배포 전 과정 |

### 3가지를 조합한 풀 워크플로우

```
1. Awesome DESIGN.md에서 레퍼런스 선택 → DESIGN.md 복사
2. /teach-impeccable → 디자인 컨텍스트 설정
3. 구현
4. /audit → 기술적 품질 점검
5. gstack /design-review → 시각적 감사
6. /normalize → /polish → 최종 다듬기
7. gstack /ship → 배포
```

---

## 마무리

Awesome DESIGN.md의 핵심은 **"0에서 시작하지 않는 것"**입니다. 이미 검증된 디자인 시스템을 가져와서 시작하면, AI가 만드는 UI의 출발점 자체가 달라집니다.

DESIGN.md 하나만 복사해도 "Inter 폰트 + 보라색 그라데이션"의 AI Slop에서 벗어날 수 있습니다.

---
**출처**: [Awesome DESIGN.md — VoltAgent](https://github.com/VoltAgent/awesome-design-md)
**작성일**: 2026-04-08
**태그**: DESIGN.md, AI디자인, 디자인시스템, Claude Code, Impeccable, Google Stitch, Vercel, Stripe, Notion, 바이브코딩, AI Slop
