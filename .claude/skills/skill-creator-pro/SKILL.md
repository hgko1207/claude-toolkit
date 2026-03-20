---
name: skill-creator-pro
description: 고급 스킬을 체계적으로 생성. references/assets 구조 포함. "스킬 프로", "체계적인 스킬" 요청 시 사용.
argument-hint: [스킬 이름과 용도]
---

# 스킬 크리에이터 Pro

references/, assets/ 구조를 갖춘 체계적인 스킬을 생성합니다.

## 핵심 원칙

### 간결함이 핵심
컨텍스트 윈도우는 공유 자원. Claude가 이미 아는 것은 쓰지 않음. 모든 문단의 토큰 비용을 의심할 것.

### 자유도 조절
- **유연한 작업** (문서 작성, 코드 리뷰): 높은 자유도, 텍스트 지침
- **정밀한 작업** (배포, DB 마이그레이션): 낮은 자유도, 스크립트 사용

### 점진적 공개 (Progressive Disclosure)
1. **메타데이터**: 항상 로딩 (~100 단어) — name, description
2. **SKILL.md 본문**: 트리거 시 로딩 (<5000 단어) — 지침
3. **번들 리소스**: 필요 시 로딩 — references/, assets/

## 스킬 구조

```
.claude/skills/스킬이름/
├── SKILL.md          # 필수: 프론트매터 + 핵심 지침
├── scripts/          # 선택: 반복 작업용 실행 스크립트
├── references/       # 선택: 조건부 로딩 참고 문서
└── assets/           # 선택: 출력용 템플릿 (컨텍스트에 로딩 안됨)
```

| 폴더 | 용도 | 컨텍스트 로딩 |
|------|------|-------------|
| `scripts/` | 반복적, 결정적 작업 자동화 | 실행만 (내용 로딩 X) |
| `references/` | API 스키마, 정책 문서, 예시 | 필요 시 로딩 |
| `assets/` | 템플릿, 보일러플레이트 | 로딩 안됨 (출력 전용) |

## 프론트매터 필드

```yaml
---
name: 스킬이름                    # 필수
description: 트리거 조건 설명       # 필수 — 자동 트리거의 유일한 기준
argument-hint: [인자 설명]         # 선택
disable-model-invocation: true    # 선택: 도구 호출 비활성화
---
```

> **중요**: `description`이 트리거 여부를 결정. 본문에 "When to Use" 섹션을 써도 트리거 전에는 읽히지 않으므로 의미 없음.

## 생성 워크플로우

1. **예시로 이해**: 사용자에게 구체적인 사용 사례 수집
2. **구성 계획**: 스크립트, references, assets 중 필요한 것 식별
3. **구조 생성**: 폴더 구조 생성
4. **작성**: 리소스 파일 → SKILL.md 순으로 작성
5. **테스트**: `/스킬이름`으로 호출하여 동작 확인
6. **반복 개선**: 실사용 피드백으로 개선

## 작성 가이드

상세 가이드: [references/writing-guide.md](references/writing-guide.md)
예시 모음: [references/examples.md](references/examples.md)
템플릿: [assets/skill-template/](assets/skill-template/)