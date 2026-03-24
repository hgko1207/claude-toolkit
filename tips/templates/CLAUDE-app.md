# CLAUDE.md — Mobile App (React Native / Expo)

> 바이브 코딩용 앱 프로젝트 템플릿. 프로젝트에 맞게 수정해서 사용.

---

## 프로젝트 스택

- **Framework**: Expo (SDK 52+, New Architecture)
- **Language**: TypeScript (strict mode)
- **Navigation**: Expo Router v4 (파일 기반 라우팅)
- **Package Manager**: pnpm
- **Backend**: Supabase (Auth + DB + Storage)
- **State**: Zustand (전역), TanStack Query (서버 상태)
- **UI**: NativeWind (Tailwind for RN) + React Native Paper
- **Testing**: Jest + React Native Testing Library

> 위 스택을 실제 프로젝트에 맞게 수정해서 사용.

---

## 명령어

```bash
# 개발 서버
pnpm start          # Expo Go
pnpm ios            # iOS 시뮬레이터
pnpm android        # Android 에뮬레이터

# 빌드 (EAS)
eas build --platform ios --profile preview
eas build --platform android --profile preview

# 타입 체크
pnpm typecheck

# 테스트
pnpm test -- --testFile 경로  # 단일 파일 실행 선호

# 린트
pnpm lint
```

---

## 디렉터리 구조

```
app/                    # Expo Router 파일 기반 라우팅
├── (auth)/             # 인증 전 화면 (로그인, 회원가입)
├── (tabs)/             # 탭 네비게이션
│   ├── index.tsx       # 홈
│   └── profile.tsx     # 프로필
└── _layout.tsx         # 루트 레이아웃

src/
├── components/
│   ├── ui/             # 공통 UI 컴포넌트
│   └── [feature]/      # 기능별 컴포넌트
├── hooks/              # 커스텀 훅
├── stores/             # Zustand 스토어
├── lib/                # 유틸리티
└── types/              # TypeScript 타입
```

---

## 코딩 컨벤션

### 컴포넌트
- 함수형 컴포넌트만 사용
- 파일명: PascalCase (`UserCard.tsx`)
- Props 타입: 인터페이스로 정의 (`interface Props`)
- `StyleSheet.create()` 보다 NativeWind className 선호

### TypeScript
- `any` 타입 금지 — `unknown` 또는 명시적 타입 사용
- 플랫폼별 분기: `Platform.OS === 'ios'` 명시적 체크

### 네비게이션 (Expo Router)
- 파일 기반 라우팅 — 새 화면은 `app/` 폴더에 파일 생성
- 라우트 파라미터: `useLocalSearchParams()` 사용
- 화면 이동: `router.push()`, `router.replace()` 사용

### 스타일링
- NativeWind className 우선 사용
- 플랫폼별 스타일: `Platform.select()` 또는 `.ios.tsx` / `.android.tsx` 확장자

---

## 플랫폼 주의사항

- **iOS**: SafeAreaView 항상 사용, 노치/다이나믹 아일랜드 고려
- **Android**: 상태바 높이 `StatusBar.currentHeight` 처리
- **공통**: 키보드 올라올 때 `KeyboardAvoidingView` 사용

---

## Git 워크플로우

- **브랜치**: `feature/기능명`, `bugfix/이슈`, `chore/작업`
- **main 직접 push 금지** — PR 필수
- **커밋 메시지**: `feat:`, `fix:`, `chore:`, `refactor:` 프리픽스
- 새 네이티브 모듈 추가 시 반드시 팀에 알릴 것 (prebuild 필요)

---

## 환경 변수

- `.env.local` — 로컬 개발 전용 (git 커밋 금지)
- `app.config.ts`의 `extra` 필드로 Expo에 노출
- **환경 변수 파일 수정 금지** — 추가 필요 시 사용자에게 알려줄 것

---

## 주의사항

- `expo-router` 관련 파일(`app/_layout.tsx` 등) 구조 변경 시 주의
- 새 네이티브 패키지 설치 후 `expo prebuild` 필요 (bare workflow 시)
- `package.json` 의존성 직접 수정 금지 — `pnpm add` 사용
- 이미지: `expo-image` 사용, 기본 `Image` 컴포넌트 지양
- 큰 목록: `FlashList` 사용 (FlatList 대신, 성능 우수)

---

## 코드 작성 스타일 (바이브 코딩)

- 새 화면 추가: `app/` 폴더에 파일 생성 → 컴포넌트 작성 순서
- 공통 UI: `src/components/ui/`에 먼저 확인 후 없으면 생성
- API 호출: TanStack Query 훅으로 래핑 (`useQuery`, `useMutation`)
- 에러 처리: 사용자에게 보이는 토스트/알림 + 콘솔 로그 구분
- 로딩 상태: 항상 스켈레톤 또는 로딩 인디케이터 포함
