---
name: project-init
description: 새 프로젝트에 Claude Code 에이전트/스킬 환경을 세팅. "프로젝트 초기화", "클로드 세팅해줘" 요청 시 사용.
argument-hint: [프로젝트 유형: web/app/api/library]
---

# 프로젝트 초기화

새 프로젝트에 Claude Code 에이전트 + 스킬 + CLAUDE.md를 한 번에 세팅합니다.

## 워크플로우

1. **프로젝트 분석**: 기술 스택, 폴더 구조, package.json 등을 파악
2. **CLAUDE.md 생성**: 프로젝트 컨벤션, 규칙, 기술 스택 정리
3. **plan.md 생성**: 구현 플랜 템플릿
4. **에이전트 생성**: 프로젝트에 맞는 에이전트 세트
5. **스킬 생성**: 배포, 플랜 등 프로젝트 맞춤 스킬

## CLAUDE.md 템플릿

```markdown
# 프로젝트명

## 기술 스택

- 프레임워크:
- 언어:
- 스타일:
- 배포:

## 컨벤션

- 타입: any/unknown 사용 금지
- 커밋: 한글로 작성
- 브랜치: main

## 주요 명령어

- `npm run dev` — 개발 서버
- `npm run build` — 빌드
- `npx tsc --noEmit` — 타입체크

## 폴더 구조

(프로젝트 분석 후 자동 작성)
```

## 프로젝트 유형별 에이전트

### Web/App (Next.js, React 등)

- **implementer**: plan.md 기반 구현
- **deployer**: 빌드 → 커밋 → 배포
- **reviewer**: 코드 리뷰 (수정 권한 없음)

### API (Express, FastAPI 등)

- **implementer**: 엔드포인트 구현
- **tester**: API 테스트 작성/실행
- **deployer**: 빌드 → 배포

### Library (npm 패키지 등)

- **implementer**: 기능 구현
- **tester**: 테스트 작성/실행
- **publisher**: 버전 범프 → npm publish

## 프로젝트 맞춤 스킬

### 모든 프로젝트 공통

- **plan**: 요청사항을 plan.md에 정리
- **deploy**: 빌드 확인 → 커밋 → 배포

### 웹 프로젝트 추가

- **content**: 콘텐츠 추가/수정

## 실행

1. 프로젝트 루트에서 `/project-init web` 실행
2. 프로젝트 구조 자동 분석
3. CLAUDE.md, plan.md, 에이전트, 스킬 일괄 생성
4. 사용자에게 생성된 구조 요약
