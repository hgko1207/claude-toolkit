# 스킬 작성 가이드

## SKILL.md 작성 규칙

### 길이 제한
- SKILL.md는 **500줄 이하** 유지
- 초과 시 references/로 분리

### 문체
- **명령형** 사용 ("~합니다" 대신 "~하세요")
- 단계별 워크플로우는 **번호** 나열
- 규칙/제약은 **별도 섹션**으로 분리

### description 작성법

description은 스킬 트리거의 **유일한 기준**:

```yaml
# 좋음 — 구체적 트리거 키워드
description: 빌드 후 커밋하고 Vercel에 배포. "배포해줘" 요청 시 사용.

# 좋음 — 여러 트리거 포함
description: 프롬프트를 토큰 효율적으로 압축. "프롬프트 압축", "토큰 줄여줘" 요청 시 사용.

# 나쁨 — 모호
description: 코드 도움
```

### 리소스 분리 기준

| 내용 | 위치 |
|------|------|
| 핵심 워크플로우 | SKILL.md 본문 |
| API 스키마, 정책 | references/ |
| 길고 반복적인 예시 | references/examples.md |
| 출력 템플릿 | assets/ |
| 자동화 스크립트 | scripts/ |

## scripts/ 활용

스크립트로 뺄 수 있는 작업:
- 파일/폴더 초기 생성 (`init.sh`)
- 빌드/린트/타입체크 실행
- 데이터 변환/생성

```bash
#!/bin/bash
# scripts/init.sh — 스킬 폴더 구조 생성
SKILL_NAME=$1
mkdir -p ".claude/skills/$SKILL_NAME/references"
mkdir -p ".claude/skills/$SKILL_NAME/assets"
touch ".claude/skills/$SKILL_NAME/SKILL.md"
echo "Created skill: $SKILL_NAME"
```

## 토큰 효율 팁

1. Claude가 **이미 아는 것**은 쓰지 않기 (언어 문법, 프레임워크 기본 등)
2. **도메인 전문 용어**로 압축 ("SRP 적용" > "하나의 클래스는 하나의 책임만...")
3. `[조건] → [행동]` 패턴 사용
4. 중복 표현 통합