---
name: project-init-pro
description: 새 프로젝트에 Claude Code 환경을 체계적으로 세팅. CLAUDE.md, plan.md, 에이전트, 스킬을 일괄 생성. "프로젝트 세팅", "클로드 초기화" 요청 시 사용.
argument-hint: [프로젝트 유형: web/api/library]
---

# 프로젝트 초기화 Pro

새 프로젝트에 CLAUDE.md + plan.md + 에이전트 + 스킬을 일괄 세팅합니다.

## 워크플로우

1. **프로젝트 분석**
   - package.json, tsconfig.json 등으로 기술 스택 파악
   - 폴더 구조 탐색
   - 빌드/테스트/배포 명령어 확인
2. **CLAUDE.md 생성** — [assets/CLAUDE-template.md](assets/CLAUDE-template.md) 기반
   - 기술 스택, 컨벤션, 명령어, 폴더 구조 정리
3. **plan.md 생성** — [assets/plan-template.md](assets/plan-template.md) 기반
   - 현재 상태 + 향후 계획 구조
4. **에이전트 생성** — 프로젝트 유형별 세트
5. **스킬 생성** — 배포, 플랜 등 프로젝트 맞춤
6. **결과 요약** — 생성된 파일 목록과 사용법 안내

## 프로젝트 유형별 구성

### Web/App (Next.js, React, Vue 등)

에이전트:
- `implementer` — plan.md 기반 구현
- `reviewer` — 코드 리뷰 (읽기 전용)
- `deployer` — 빌드 → 커밋 → 배포

스킬:
- `plan` — 요청사항을 plan.md에 정리
- `deploy` — 빌드 → 커밋 → push

### API (Express, FastAPI, NestJS 등)

에이전트:
- `implementer` — 엔드포인트 구현
- `tester` — API 테스트 작성/실행
- `deployer` — 빌드 → 배포

스킬:
- `plan` — 요청사항을 plan.md에 정리
- `deploy` — 빌드 → 배포

### Library (npm 패키지, Python 패키지 등)

에이전트:
- `implementer` — 기능 구현
- `tester` — 테스트 작성/실행

스킬:
- `plan` — 요청사항을 plan.md에 정리

## 규칙

- 기존 `.claude/` 폴더가 있으면 덮어쓰지 않고 병합
- CLAUDE.md가 이미 있으면 기존 내용을 보존하며 보강
- 에이전트는 프로젝트 기술 스택에 맞게 시스템 프롬프트 커스터마이징