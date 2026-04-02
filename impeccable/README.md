# Impeccable — AI 코딩 도구의 프론트엔드 디자인 품질을 높이는 스킬+명령어 세트

> AI가 생성하는 "그럴듯하지만 어딘가 어색한" UI(AI Slop)를 방지하고, 전문 디자이너 수준의 디자인 어휘와 원칙을 Claude Code에 주입합니다.

---

## 핵심 개념

Impeccable은 Anthropic의 `frontend-design` 스킬을 확장한 프로젝트입니다.
Paul Bakaus가 만들었으며, AI 코딩 도구가 프론트엔드 코드를 생성할 때 발생하는 **AI Slop**(과도한 그라데이션, 의미 없는 장식, 일관성 없는 간격 등)을 체계적으로 잡아줍니다.

**AI Slop이란?**
AI가 "있어 보이게" 만들려고 넣는 불필요한 시각적 요소들입니다.
그림자 남발, 무의미한 둥근 모서리, 과한 애니메이션 등이 대표적입니다.
Impeccable은 이런 패턴을 안티패턴으로 정의하고, 명령어 하나로 탐지/수정할 수 있게 해줍니다.

**디자인 어휘 제공**
AI에게 "예쁘게 만들어줘"라고 말하면 AI Slop이 나옵니다.
Impeccable은 타이포그래피, 색상, 간격, 레이아웃, 반응형 등 구체적인 디자인 원칙을 시스템 프롬프트로 주입하여, AI가 정확한 디자인 언어로 코드를 작성하도록 유도합니다.

---

## 구성 요소

| 구성 요소 | 설명 |
|-----------|------|
| frontend-design 스킬 | 7개 레퍼런스 파일로 구성된 디자인 원칙 시스템입니다 |
| 명령어 (Commands) | 20개의 슬래시 명령어로 디자인 검사/수정/조정을 수행합니다 |
| 안티패턴 (Anti-patterns) | AI Slop의 구체적 패턴을 정의하고 자동 탐지합니다 |

### frontend-design 스킬 레퍼런스 (7개)

| 레퍼런스 | 역할 |
|----------|------|
| Typography | 글꼴 크기, 행간, 자간, 서체 조합 원칙입니다 |
| Color | 색상 팔레트, 대비, 접근성 가이드라인입니다 |
| Spacing | 여백, 패딩, 간격 시스템(4px/8px 그리드 등)입니다 |
| Layout | 플렉스/그리드 레이아웃, 정렬, 시각적 계층 구조입니다 |
| Responsive | 브레이크포인트, 모바일 우선, 유동적 타이포그래피입니다 |
| Components | 버튼, 카드, 모달 등 UI 컴포넌트 디자인 기준입니다 |
| Animation | 전환, 모션, 마이크로인터랙션 원칙입니다 |

---

## 명령어 카테고리

### 검사 / 리뷰

| 명령어 | 역할 |
|--------|------|
| `/ui-review` | 현재 UI의 디자인 품질을 종합 리뷰합니다 |
| `/check-contrast` | 색상 대비 접근성(WCAG) 검사를 수행합니다 |
| `/check-spacing` | 간격/여백 일관성을 검사합니다 |
| `/check-typography` | 타이포그래피 계층 구조를 검사합니다 |
| `/check-responsive` | 반응형 브레이크포인트별 레이아웃을 검사합니다 |

### 수정 / 정리

| 명령어 | 역할 |
|--------|------|
| `/fix-slop` | AI Slop 안티패턴을 탐지하고 자동 수정합니다 |
| `/clean-css` | 불필요한 CSS 속성을 정리합니다 |
| `/normalize-spacing` | 간격을 디자인 시스템 기준으로 정규화합니다 |
| `/remove-decorations` | 의미 없는 장식적 요소를 제거합니다 |

### 디자인 조정

| 명령어 | 역할 |
|--------|------|
| `/adjust-palette` | 색상 팔레트를 조화롭게 조정합니다 |
| `/refine-typography` | 타이포그래피 스케일과 계층을 다듬습니다 |
| `/polish` | 전체 UI를 한 단계 다듬어줍니다 |
| `/add-motion` | 적절한 마이크로인터랙션을 추가합니다 |

### 구조

| 명령어 | 역할 |
|--------|------|
| `/design-system` | 프로젝트의 디자인 시스템을 생성/분석합니다 |
| `/component-audit` | UI 컴포넌트의 일관성을 감사합니다 |
| `/layout-review` | 전체 레이아웃 구조를 리뷰합니다 |

### 특수

| 명령어 | 역할 |
|--------|------|
| `/before-after` | 변경 전후 스크린샷을 비교합니다 |
| `/design-brief` | 디자인 의도와 결정 사항을 문서화합니다 |
| `/impeccable-upgrade` | Impeccable을 최신 버전으로 업그레이드합니다 |
| `/accessibility` | 종합 접근성 검사를 수행합니다 |

---

## 30초 요약

```
디자인 시스템 설정  →  /design-system
코드 작성          →  (AI로 프론트엔드 개발)
AI Slop 제거       →  /fix-slop
디자인 검사        →  /ui-review
세부 조정          →  /refine-typography + /adjust-palette + /normalize-spacing
접근성 확인        →  /accessibility + /check-contrast
최종 다듬기        →  /polish
변경 확인          →  /before-after
```

---

## 가이드 목록

| 파일 | 내용 |
|------|------|
| [install-guide.md](install-guide.md) | 설치 방법 및 초기 설정 가이드입니다 |
| [commands-guide.md](commands-guide.md) | 전체 명령어 상세 사용법입니다 |
| [design-principles-guide.md](design-principles-guide.md) | 디자인 원칙과 안티패턴 레퍼런스입니다 |
| [workflows.md](workflows.md) | 실전 워크플로우 조합 패턴입니다 |
| [usage-guide.md](usage-guide.md) | 사용 가이드 — 설치부터 실전 시나리오까지 완전 가이드 (블로그용) |
| [compatibility-guide.md](compatibility-guide.md) | 호환성 가이드 — 웹/WPF/Flutter별 적용 범위 + gstack 병행법 |

---

**출처:** [github.com/pbakaus/impeccable](https://github.com/pbakaus/impeccable) | [impeccable.style](https://impeccable.style)
**만든 사람:** Paul Bakaus | **라이선스:** Apache 2.0 | **Stars:** 14,762 | **Forks:** 632

`#claude-code` `#frontend` `#design` `#ai-slop` `#impeccable` `#skill`
