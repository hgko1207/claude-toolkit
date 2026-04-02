# Impeccable 호환성 가이드 — 웹 전용인가? 데스크탑에서도 쓸 수 있는가?

> Impeccable은 웹 프론트엔드(HTML/CSS/JS)에 특화되어 있지만, UX 리뷰와 디자인 원칙은 기술 스택과 무관하게 활용할 수 있습니다.

## 핵심 요약

- Impeccable의 코드 수정 명령어(`/normalize`, `/colorize`, `/animate` 등)는 **CSS 기반**이라 웹 프로젝트 전용입니다
- UX 리뷰 명령어(`/critique`, `/clarify`, `/distill`)는 **기술 스택과 무관**하게 사용 가능합니다
- C#/WPF, Flutter, Swift UI 등 데스크탑/모바일 프로젝트에서는 **디자인 사고방식과 안티패턴**을 참조하는 용도로 활용합니다
- 기존 프로젝트와 신규 프로젝트 **모두 사용 가능**합니다

---

## 기술 스택별 호환성

### 레퍼런스 파일 호환성

| 레퍼런스 | 사용 기술 | 웹 | WPF/C# | Flutter | Swift UI |
|---------|----------|:--:|:------:|:-------:|:--------:|
| Typography | CSS `clamp()`, Google Fonts | O | X | X | X |
| Color & Contrast | CSS `oklch()`, `color-mix()` | O | X | X | X |
| Spatial Design | CSS Grid, container queries | O | X | X | X |
| Motion Design | CSS `transition`, `@keyframes` | O | X | X | X |
| Responsive Design | 미디어 쿼리, fluid 사이징 | O | X | 일부 | 일부 |
| Interaction Design | `:focus-visible`, HTML `<dialog>` | O | X | X | X |
| **UX Writing** | 버튼 레이블, 에러 메시지, 빈 상태 | **O** | **O** | **O** | **O** |

### 명령어 호환성

#### 기술 스택 무관 — 어디서든 사용 가능

| 명령어 | 용도 | 왜 가능한가 |
|--------|------|------------|
| `/critique` | UX 디자인 리뷰 (위계, 인지 부하, 감성) | 코드가 아닌 **디자인 관점** 평가이므로 |
| `/clarify` | UX 카피, 에러 메시지, 라벨 개선 | **텍스트 품질** 리뷰이므로 |
| `/distill` | 불필요한 복잡성 제거 | UI **구조 단순화** 원칙이므로 |
| `/onboard` | 온보딩 플로우, 빈 상태 설계 | **사용자 경험** 설계이므로 |
| `/teach-impeccable` | 프로젝트 디자인 컨텍스트 수집 | 질문-답변 기반이므로 |

#### 일부 적용 가능

| 명령어 | 용도 | 적용 범위 |
|--------|------|----------|
| `/audit` | 기술적 품질 검사 | 안티패턴, 접근성 **원칙**은 적용 가능. CSS 구체 항목은 해당 없음 |
| `/arrange` | 레이아웃, 스페이싱 개선 | **시각적 리듬** 원칙은 적용 가능. CSS 코드 수정은 해당 없음 |
| `/typeset` | 타이포그래피 개선 | **폰트 위계** 원칙은 적용 가능. CSS 구현은 해당 없음 |
| `/extract` | 재사용 컴포넌트 추출 | **패턴 식별**은 적용 가능. CSS 토큰 추출은 해당 없음 |

#### 웹 프론트엔드 전용

| 명령어 | 이유 |
|--------|------|
| `/normalize` | CSS 디자인 토큰, 스페이싱 토큰 기반 |
| `/polish` | CSS 세부 수정 (정렬, 간격, 마이크로 디테일) |
| `/colorize` | CSS 색상 함수 (`oklch`, `color-mix`) 사용 |
| `/animate` | CSS `transition`, `@keyframes` 기반 |
| `/bolder` | CSS 시각적 강도 조절 |
| `/quieter` | CSS 시각적 강도 축소 |
| `/delight` | CSS 마이크로 인터랙션 추가 |
| `/adapt` | CSS 미디어 쿼리, 브레이크포인트 기반 |
| `/optimize` | 웹 성능 (번들 사이즈, 이미지, 렌더링) |
| `/harden` | HTML/CSS 기반 에러 처리, i18n |
| `/overdrive` | WebGL 셰이더, CSS 스크롤 효과 |

---

## 기존 프로젝트 vs 신규 프로젝트

**둘 다 사용 가능합니다.** 오히려 기존 프로젝트에서 더 효과적입니다.

| 상황 | 추천 흐름 |
|------|----------|
| **기존 웹 프로젝트** | `/teach-impeccable` → `/audit` → 문제 파악 → `/normalize`, `/polish` 등으로 수정 |
| **신규 웹 프로젝트** | `/teach-impeccable` → 구현 → `/audit` → `/polish` |
| **기존 WPF 프로젝트** | `/teach-impeccable` → `/critique` → UX 개선점 파악 → 수동 반영 |
| **신규 WPF 프로젝트** | `/teach-impeccable` → 구현 → `/critique` → `/clarify` |

핵심은 **`/teach-impeccable`을 프로젝트마다 1번** 실행하는 것입니다. 프로젝트 루트에 `.impeccable.md`가 생기고, 이후 모든 명령어가 이 컨텍스트를 참조합니다.

---

## WPF/C# 프로젝트에서의 현실적 활용법

### 활용 가능한 것

1. **`/critique`로 UX 리뷰** — 시각적 위계, 인지 부하, 감성적 공감을 평가받을 수 있습니다
2. **`/clarify`로 텍스트 품질 개선** — 에러 메시지, 버튼 레이블, 안내문을 더 명확하게 다듬습니다
3. **`/distill`로 UI 단순화** — 불필요한 요소를 식별하고 제거 방향을 제시합니다
4. **안티패턴 참조** — 카드 남용, 위계 문제, 색상 대비 부족 등은 WPF에도 동일하게 적용됩니다
5. **`/onboard`로 첫 실행 경험 설계** — 온보딩 플로우와 빈 상태는 기술 스택과 무관합니다

### 활용할 수 없는 것

- CSS 코드 직접 수정 (`/normalize`, `/colorize`, `/animate` 등)
- 웹 성능 최적화 (`/optimize` — 번들 사이즈, 이미지 최적화)
- 반응형 디자인 (`/adapt` — 미디어 쿼리 기반)
- 웹 접근성 세부 항목 (`/audit`의 ARIA, 시맨틱 HTML 체크)

### XAML에 디자인 원칙 적용하기

Impeccable의 원칙을 WPF XAML에 수동으로 적용할 수 있습니다:

| Impeccable 원칙 | XAML 적용 |
|-----------------|----------|
| 순수 검정(#000) 사용 금지 | `Color="#1A1A2E"` 처럼 틴티드 다크 사용 |
| 카드 남용 금지 | `Border` 중첩을 최소화하고 `Margin`으로 그루핑 |
| 모듈러 타입 스케일 | `FontSize`를 12/14/16/20/24/32 등 일관된 스케일로 |
| 색상 대비 4.5:1 이상 | WPF에서도 접근성 대비 비율 준수 |
| 의미 있는 여백 변화 | `Margin`/`Padding`에 일관된 스페이싱 시스템 (4/8/12/16/24/32) |

---

## gstack과의 병행

| | gstack | Impeccable |
|---|---|---|
| 초점 | 개발 워크플로우 전체 (기획→배포) | 프론트엔드 **디자인 품질** |
| 디자인 범위 | DESIGN.md 생성, 스크린샷 기반 시각적 감사 | 코드 레벨 디자인 원칙, 안티패턴 |
| WPF 호환 | `/review`, `/investigate` 등 코드 리뷰 가능 | `/critique`, `/clarify` 등 UX 리뷰 가능 |

### 웹 프로젝트에서의 조합

```
gstack /design-consultation   → DESIGN.md 생성
/teach-impeccable             → .impeccable.md 생성
구현
/audit                        → 기술적 품질 점검
gstack /design-review         → 스크린샷 기반 시각적 감사
/normalize → /polish          → 코드 레벨 수정
gstack /ship                  → 배포
```

### WPF 프로젝트에서의 조합

```
/teach-impeccable             → 디자인 컨텍스트 설정
구현
/critique                     → UX 디자인 리뷰
/clarify                      → 텍스트/메시지 개선
gstack /review                → 코드 리뷰
gstack /ship                  → 배포
```

---

## 정리

| 질문 | 답변 |
|------|------|
| 웹에서 사용 가능? | **O** — 모든 명령어 100% 호환 |
| 모바일 앱(React Native, Flutter)에서? | **일부** — UX 리뷰 명령어 + 디자인 원칙 참조 |
| 데스크탑(WPF, C#)에서? | **일부** — UX 리뷰 명령어 + 디자인 원칙 참조 |
| 기존 프로젝트에서? | **O** — 오히려 기존 프로젝트에서 더 효과적 |
| gstack과 충돌? | **X** — 역할이 달라서 보완적으로 병행 가능 |

---
**출처**: [Impeccable GitHub](https://github.com/pbakaus/impeccable) — Paul Bakaus
**작성일**: 2026-03-30
**태그**: #Impeccable #ClaudeCode #WPF #데스크탑 #호환성 #웹디자인 #UX #gstack
