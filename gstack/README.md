# gstack — Claude Code 가상 엔지니어링 팀

> Garry Tan (Y Combinator CEO)이 60일 만에 60만 줄 프로덕션 코드를 혼자 작성하며 사용한 워크플로우.
> Claude Code를 기획자 · 설계자 · 개발자 · QA · 배포 담당으로 구성된 팀으로 만들어준다.

---

## 설치

→ [install-guide.md](install-guide.md) 참고

---

## 전체 명령어 목록

### 기획 & 전략

| 명령어 | 역할 | 핵심 질문 |
|--------|------|----------|
| `/office-hours` | YC 스타트업 멘토링 | "이 기능이 진짜 필요한가?" |
| `/plan-ceo-review` | CEO 시각으로 플랜 검토 | "더 크게 생각할 수 있나?" |
| `/plan-eng-review` | 엔지니어링 매니저 플랜 검토 | "아키텍처는 맞는가?" |
| `/plan-design-review` | 디자이너 시각으로 플랜 검토 | "UX는 제대로 됐나?" |
| `/autoplan` | 3개 리뷰(CEO+Eng+Design) 자동 순차 실행 | 전체 자동 검토 |

### 디자인

| 명령어 | 역할 |
|--------|------|
| `/design-consultation` | 제품 분석 → 전체 디자인 시스템 제안 → DESIGN.md 생성 |
| `/design-review` | 실행 중인 사이트에서 시각적 문제 발견 → 코드 수정 |

### 개발 & 코드 품질

| 명령어 | 역할 |
|--------|------|
| `/review` | PR diff 코드 리뷰 (SQL 안전성, LLM 경계, 조건 분기 등) |
| `/investigate` | 4단계 체계적 디버깅 (원인 없이 수정 금지) |
| `/codex` | OpenAI Codex로 3가지 모드 추가 검토 |
| `/simplify` | 변경 코드 품질/재사용성 리뷰 후 수정 |

### QA & 테스트

| 명령어 | 역할 |
|--------|------|
| `/browse` | 헤드리스 브라우저 — 스크린샷, 요소 조작, diff |
| `/qa` | QA 테스트 + 버그 자동 수정 |
| `/qa-only` | QA 테스트 + 리포트만 (수정 안 함) |
| `/benchmark` | 페이지 로드, Core Web Vitals 성능 측정 |
| `/setup-browser-cookies` | 로컬 크롬 쿠키 → 헤드리스 세션으로 가져오기 |

### 배포

| 명령어 | 역할 |
|--------|------|
| `/setup-deploy` | 배포 플랫폼 자동 감지 + CLAUDE.md에 설정 저장 |
| `/ship` | 테스트 → 리뷰 → VERSION 업 → CHANGELOG → PR 생성 |
| `/land-and-deploy` | PR 머지 → CI 대기 → 프로덕션 헬스체크 |
| `/canary` | 배포 후 라이브 앱 모니터링 |
| `/document-release` | 배포 후 README/CHANGELOG/CLAUDE.md 문서 업데이트 |

### 보안 & 안전

| 명령어 | 역할 |
|--------|------|
| `/cso` | 보안 감사 (시크릿, 의존성, CI/CD, OWASP Top 10) |
| `/careful` | 위험 명령어 실행 전 경고 |
| `/freeze [경로]` | 특정 디렉터리 외 편집 차단 |
| `/guard` | `/careful` + `/freeze` 조합 — 최강 안전 모드 |
| `/unfreeze` | freeze 해제 |

### 회고 & 유지관리

| 명령어 | 역할 |
|--------|------|
| `/retro` | 주간 커밋 히스토리 분석 + 팀원별 기여 회고 |
| `/gstack-upgrade` | gstack 최신 버전으로 업그레이드 |

---

## 가이드 목록

| 파일 | 내용 |
|------|------|
| [install-guide.md](install-guide.md) | 설치 방법 (Windows 트러블슈팅 포함) |
| [planning-guide.md](planning-guide.md) | 기획/플래닝 명령어 상세 가이드 |
| [development-guide.md](development-guide.md) | 개발/디버깅/코드리뷰 상세 가이드 |
| [design-guide.md](design-guide.md) | 디자인 명령어 상세 가이드 |
| [qa-deploy-guide.md](qa-deploy-guide.md) | QA + 배포 워크플로우 상세 가이드 |
| [workflows.md](workflows.md) | 실전 워크플로우 조합 패턴 |

---

## 30초 요약

```
아이디어  →  /office-hours
플랜 작성 →  /plan-ceo-review + /plan-eng-review + /plan-design-review
          →  (또는 /autoplan 하나로)
개발      →  코딩
코드 리뷰 →  /review
QA        →  /qa
배포      →  /ship → /land-and-deploy
모니터링  →  /canary
회고      →  /retro
```
