# Impeccable 실전 워크플로우

> 상황별 최적 명령어 조합 패턴입니다. 새 프로젝트 시작부터 출시 전 최종 점검까지 5가지 워크플로우를 다룹니다.

---

## 워크플로우 1: 새 프로젝트 시작

프로젝트 초기에 디자인 컨텍스트를 설정하고, 기반을 정리한 뒤 마무리합니다.

```
/teach-impeccable              → 디자인 원칙과 안티패턴 규칙을 Claude에 주입
  ↓
UI 개발                        → 프론트엔드 코드 작성
  ↓
/audit                         → 전체 디자인 품질 검사 (AI Slop 탐지 포함)
  ↓
발견된 문제 수정
  ↓
/normalize                     → 간격, 색상, 타이포그래피를 디자인 시스템 기준으로 정규화
  ↓
/polish                        → 전체 UI를 한 단계 다듬기
```

---

## 워크플로우 2: 기존 프로젝트 개선

이미 동작하는 프로젝트의 디자인 품질을 체계적으로 끌어올립니다.

```
/audit                         → 현재 상태의 디자인 품질 진단
  ↓
/critique                      → 구체적인 디자인 문제점과 개선 방향 제시
  ↓
문제 우선순위별 수정
  ↓
/normalize                     → 간격/색상/타이포그래피 정규화
  ↓
/refine                        → 타이포그래피 스케일과 색상 팔레트 미세 조정
  ↓
/polish                        → 최종 다듬기
```

---

## 워크플로우 3: 밋밋한 디자인 살리기

기능은 완성됐지만 디자인이 평범하고 생기가 없을 때 사용합니다.

```
/critique                      → 현재 디자인의 약점 분석
  ↓
/bolder                        → 타이포그래피 대비와 시각적 계층 강화
  ↓
/colorize                      → 색상 팔레트에 생기와 개성 추가
  ↓
/animate                       → 상태 전환에 적절한 모션 추가
  ↓
/delight                       → 마이크로인터랙션과 디테일 추가
  ↓
/polish                        → 전체 조화 확인 및 마무리
```

---

## 워크플로우 4: 과한 디자인 정리

AI가 생성한 코드에 그라데이션, 그림자, 애니메이션이 과하게 들어갔을 때 사용합니다. AI Slop 제거에 특화된 흐름입니다.

```
/audit                         → AI Slop 안티패턴 탐지
  ↓
/distill                       → 불필요한 장식 요소를 제거하고 핵심만 남기기
  ↓
/quieter                       → 시각적 소음(visual noise) 줄이기
  ↓
/normalize                     → 간격과 색상을 디자인 시스템 기준으로 재정렬
  ↓
/polish                        → 깔끔해진 UI 최종 다듬기
```

---

## 워크플로우 5: 출시 전 최종 점검

배포 직전에 디자인 품질, 접근성, 성능, 반응형을 종합적으로 점검합니다.

```
/audit                         → 전체 디자인 품질 종합 검사
  ↓
/harden                        → 접근성 강화 (대비, 포커스 상태, ARIA)
  ↓
/adapt                         → 반응형 레이아웃 점검 및 보완
  ↓
/optimize                      → CSS 성능 최적화 (불필요한 속성 제거, 리플로우 방지)
  ↓
/polish                        → 최종 마무리
```

---

## 상황별 빠른 선택 가이드

| 상황 | 명령어 |
|------|--------|
| 디자인 원칙 주입 (최초 1회) | `/teach-impeccable` |
| 전체 디자인 품질 진단 | `/audit` |
| 구체적 문제점 분석 | `/critique` |
| 간격/색상/타이포 정규화 | `/normalize` |
| 타이포그래피 미세 조정 | `/refine` |
| 시각적 계층 강화 | `/bolder` |
| 색상 팔레트에 생기 추가 | `/colorize` |
| 모션/애니메이션 추가 | `/animate` |
| 마이크로인터랙션 추가 | `/delight` |
| 장식 요소 제거 | `/distill` |
| 시각적 소음 줄이기 | `/quieter` |
| 접근성 강화 | `/harden` |
| 반응형 점검 | `/adapt` |
| CSS 성능 최적화 | `/optimize` |
| 최종 다듬기 | `/polish` |
| AI Slop 과한 디자인 → 정리 | `/audit` → `/distill` → `/quieter` → `/normalize` |
| 밋밋한 디자인 → 강화 | `/critique` → `/bolder` → `/colorize` → `/animate` |

---

## gstack과의 조합

Impeccable은 디자인 품질에 집중하고, gstack은 개발 워크플로우 전체를 관리합니다. 두 도구를 조합하면 디자인부터 배포까지 빈틈없는 파이프라인을 구성할 수 있습니다.

### 디자인 시스템 수립 → 디자인 컨텍스트 설정

```
gstack /design-consultation    → DESIGN.md 생성 (색상, 타이포, 컴포넌트 가이드)
  ↓
CLAUDE.md에 @DESIGN.md 추가
  ↓
Impeccable /teach-impeccable   → 디자인 원칙 + 안티패턴 규칙 주입
  ↓
UI 개발 시작
```

gstack의 `/design-consultation`이 프로젝트의 디자인 방향(DESIGN.md)을 잡아주고, Impeccable의 `/teach-impeccable`이 세부 디자인 원칙을 AI에 주입합니다. 이 조합으로 AI가 프로젝트 맥락에 맞는 고품질 UI 코드를 생성하도록 유도할 수 있습니다.

### 시각적 감사 → 기술적 감사

```
gstack /design-review          → 라이브 스크린샷 기반 시각적 감사 + 자동 수정
  ↓
Impeccable /audit              → CSS/마크업 수준의 기술적 디자인 감사
  ↓
Impeccable /polish             → 최종 다듬기
  ↓
gstack /qa                     → 기능 QA
```

gstack의 `/design-review`는 실제 렌더링된 화면을 스크린샷으로 캡처하여 시각적으로 검토합니다. Impeccable의 `/audit`는 코드 레벨에서 안티패턴, 접근성, 일관성을 검사합니다. 두 방향의 감사를 모두 수행하면 놓치는 부분이 크게 줄어듭니다.

---

## 꿀팁 모음

### 1. /teach-impeccable은 세션마다 한 번

```
새 Claude Code 세션을 시작할 때마다 /teach-impeccable을 실행합니다.
디자인 원칙이 컨텍스트에 로드되어야 이후 명령어들이 제대로 동작합니다.
```

### 2. /audit 먼저, 수정은 나중에

```
문제를 수정하기 전에 항상 /audit으로 전체 현황을 파악합니다.
개별 문제를 하나씩 고치다 보면 전체 일관성을 놓치기 쉽습니다.
```

### 3. /polish는 항상 마지막

```
모든 워크플로우의 마지막 단계는 /polish입니다.
개별 수정 후 전체 조화를 확인하는 용도로, 마무리 외에는 사용하지 않습니다.
```

### 4. 과한 수정보다 점진적 조정

```
/bolder → /colorize → /animate를 한 번에 전부 실행하지 않습니다.
한 단계씩 적용하고, 결과를 확인한 뒤 다음 단계로 넘어갑니다.
과하게 적용된 부분은 /quieter나 /distill로 되돌릴 수 있습니다.
```

### 5. DESIGN.md와 함께 사용

```
프로젝트에 DESIGN.md가 있으면 Impeccable이 해당 디자인 시스템을 기준으로 검사합니다.
gstack /design-consultation으로 생성하거나, 직접 작성하여 CLAUDE.md에서 참조합니다.
```

### 6. AI Slop이 의심되면 워크플로우 4

```
AI가 생성한 UI가 "화려하지만 어딘가 어색하다"고 느껴지면 워크플로우 4를 실행합니다.
/distill → /quieter 조합이 불필요한 장식을 걷어내고 본질을 드러냅니다.
```

---

**출처:** [github.com/pbakaus/impeccable](https://github.com/pbakaus/impeccable) | [impeccable.style](https://impeccable.style)

`#claude-code` `#impeccable` `#워크플로우` `#실전패턴` `#gstack-조합`
