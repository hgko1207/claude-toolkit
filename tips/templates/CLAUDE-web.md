# CLAUDE.md — Web Project (Next.js / React)

> 바이브 코딩용 웹 프로젝트 템플릿. 프로젝트에 맞게 수정해서 사용.

---

## 프로젝트 스택

- **Framework**: Next.js 15 (App Router)
- **Language**: TypeScript (strict mode)
- **Styling**: Tailwind CSS v4
- **Package Manager**: pnpm (npm, yarn 사용 금지)
- **DB**: Supabase (PostgreSQL + Auth + Storage)
- **ORM**: Prisma
- **State**: Zustand (전역), TanStack Query (서버 상태)
- **UI Components**: shadcn/ui

> 위 스택을 실제 프로젝트에 맞게 수정해서 사용.

---

## 명령어

```bash
# 개발 서버
pnpm dev

# 빌드 & 타입 체크
pnpm build
pnpm typecheck

# 테스트
pnpm test                    # 전체 실행 금지
pnpm test -- --testFile 경로  # 단일 파일만 실행

# 린트
pnpm lint
pnpm lint:fix

# DB 마이그레이션
pnpm prisma migrate dev
pnpm prisma generate
```

**IMPORTANT**: 반드시 pnpm 사용. npm/yarn 사용 시 lock 파일 충돌 발생.

---

## 디렉터리 구조

```
src/
├── app/              # Next.js App Router 페이지
│   ├── (auth)/       # 인증 관련 라우트
│   ├── (dashboard)/  # 대시보드 라우트
│   └── api/          # API Route Handlers
├── components/
│   ├── ui/           # shadcn/ui 기본 컴포넌트 (수정 금지)
│   └── [feature]/    # 기능별 컴포넌트
├── lib/              # 유틸리티, 헬퍼
├── hooks/            # 커스텀 훅
├── stores/           # Zustand 스토어
└── types/            # TypeScript 타입 정의
```

---

## 코딩 컨벤션

### 컴포넌트
- 함수형 컴포넌트만 사용 (class 컴포넌트 금지)
- 컴포넌트 파일명: PascalCase (`UserProfile.tsx`)
- 컴포넌트당 파일 1개 원칙
- Props 타입은 인터페이스로 정의 (`interface ComponentProps`)

### TypeScript
- `any` 타입 사용 금지 — `unknown` 또는 정확한 타입 사용
- `!` non-null assertion 최소화 — 명시적 null 체크 선호
- 공개 API는 모두 타입 정의

### Import
- ES modules만 사용 (require 금지)
- 절대 경로 사용: `@/components/...`

### 서버/클라이언트 컴포넌트
- 기본: Server Component (성능 우선)
- 클라이언트 필요 시에만 `'use client'` 추가
- 데이터 페칭: 서버 컴포넌트에서 직접 처리

---

## Git 워크플로우

- **브랜치**: `feature/기능명`, `bugfix/이슈`, `chore/작업`
- **main 직접 push 금지** — PR 필수
- **커밋 메시지**: `feat:`, `fix:`, `chore:`, `refactor:` 프리픽스 사용
- PR 생성 시 관련 이슈 번호 연결

---

## 환경 변수

- `.env.local` — 로컬 개발 전용 (git에 절대 커밋 금지)
- `.env.example` — 필요한 변수 목록만 (값 없이)
- **환경 변수 파일 수정 금지** — 추가 필요 시 사용자에게 알려줄 것

---

## 주의사항

- `src/components/ui/` 폴더 파일 직접 수정 금지 (shadcn 컴포넌트)
- `prisma/migrations/` 파일 직접 수정 금지 — `prisma migrate dev` 사용
- `package.json`의 의존성 직접 수정 금지 — `pnpm add` 명령 사용
- 이미지: `next/image` 사용, `<img>` 태그 금지

---

## 코드 작성 스타일 (바이브 코딩)

- 새 기능 추가 시 **타입부터 정의** → 구현
- 컴포넌트는 **단일 책임 원칙** — 한 가지 역할만
- 스타일은 **Tailwind 클래스만** — 인라인 style, CSS 파일 최소화
- API Route는 **항상 try-catch** + 적절한 HTTP 상태코드 반환
- 에러는 **사용자에게 보이는 메시지**와 **로그용 메시지** 구분
