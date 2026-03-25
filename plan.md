# 고급 꿀팁 추가 플랜

## 목표
조사에서 발견한 고급 팁들을 **초보자도 이해할 수 있게** 정리해서 프로젝트에 추가.
기존 `tips/` 가이드들은 기능별 레퍼런스(설정, 훅, 스킬 등)이고,
이번에 추가하는 건 **"이렇게 하면 진짜 달라진다"** 류의 실전 팁.

---

## 파일 구성

### 1. `tips/pro-tips.md` (새 파일)

**컨셉**: 기존 `claude-code-basics.md`(입문 10가지)의 **다음 단계**.
기술적 설정 방법보다 **왜 좋은지, 어떤 상황에서 쓰는지**를 중심으로 작성.

**작성 톤 예시:**
```
❌ Stop Hook에 exit code 2를 반환하는 스크립트를 PostToolUse에 등록
✅ Claude가 "다 했어요" 하고 끝내려는데 테스트가 실패?
   → 테스트 통과할 때까지 끝내지 못하게 설정할 수 있다.
```

**구성 (카테고리별):**

#### 습관편 — 오늘부터 바로 (설정 없이)
- Claude 실수 목록 기록하기 (CLAUDE.md에 "이거 하지 마" 누적)
- 대화 50~60%에서 새로 시작하기 (/context로 확인)
- Esc+Esc 체크포인트로 실험 두려움 없애기
- Ctrl+B로 긴 빌드 백그라운드 전환

#### 설정편 — 한 번 설정하면 계속 효과
- 테스트 통과 전 종료 불가 (Stop Hook)
- 코드 저장하면 자동 포맷 (PostToolUse Hook)
- 위험한 명령어 자동 차단 (PreToolUse Hook)
- opusplan — 설계는 Opus, 코딩은 Sonnet (비용 절감)

#### 도구편 — 연결하면 Claude가 더 똑똑해지는 것들
- Playwright/Puppeteer MCP — Claude가 만든 UI를 직접 브라우저로 확인
- Supabase MCP — DB 스키마 자동 싱크, 타입 자동 생성
- Gemini CLI 서브에이전트 — 거대한 코드베이스 분석 위임 (1M 컨텍스트)

#### 비용편 — API 요금제 사용자를 위한 절감 전략
- CLAUDE.md 50줄 미만 유지 + 나머지는 스킬로 분산
- ccusage로 세션별 토큰 소비 모니터링
- 작업별 모델 라우팅 (Haiku/Sonnet/Opus)
- 전체 조합 시 최대 70% 절감 가능

#### 보안편 — 혼자 개발할 때도 챙겨야 하는 것
- Parry 훅 — 외부 데이터의 프롬프트 인젝션 자동 스캔
- .env 파일 deny 목록 필수 설정
- Git Shadow Index — 세션별 독립 브랜치로 실험 안전망

**각 팁 형식:**
```
### 팁 제목 (한 줄 요약)

**문제**: 이런 상황 겪어본 적 있지?
**해결**: 이렇게 하면 된다.
**설정 방법**: (코드 블록)
**출처**: (링크)
```

---

### 2. `tips/hooks-guide.md` 보강

기존 파일에 2개 레시피 추가:
- [ ] Stop Hook 테스트 강제 레시피 (실전 훅 레시피 섹션에)
- [ ] UserPromptSubmit 키워드 → 스킬 자동 로드 레시피

---

### 3. README.md 참고 리소스 섹션 추가

기존 "참고" 섹션에 추가:
- [ ] ykdojo/claude-code-tips (45개 팁)
- [ ] awesome-claude-code (커뮤니티 큐레이션)
- [ ] ccusage (토큰 분석 도구)
- [ ] claude-devtools (도구 호출 인스펙터)
- [ ] ccswarm (병렬 세션 자동화)

---

## 작업 순서

- [x] 1. `tips/pro-tips.md` 작성 (핵심 — 가장 큰 파일)
- [x] 2. `tips/hooks-guide.md`에 2개 레시피 추가
- [x] 3. README.md에 참고 리소스 섹션 추가
- [x] 4. README.md 꿀팁 모음 테이블에 pro-tips.md 추가
- [ ] 5. 커밋 & 푸시

---

## 기존 가이드와 겹치지 않는 이유

| 기존 파일 | 성격 | pro-tips.md |
|-----------|------|-------------|
| claude-code-basics.md | 입문 요약 (유튜브 기반) | 고급 실전 팁 |
| setup-guide.md | settings.json 레퍼런스 | "왜" 이렇게 설정하면 좋은지 |
| hooks-guide.md | 훅 종류/문법 레퍼런스 | 특정 훅 활용 사례 중심 |
| workflow-guide.md | Plan/Context/Worktree 설명 | 습관/패턴 중심 |
| optimization-guide.md | 모델/비용/프롬프트 레퍼런스 | 실제 절감 수치/사례 |
