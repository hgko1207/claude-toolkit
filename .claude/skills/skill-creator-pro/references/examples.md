# 스킬 예시 모음

## 1. 배포 스킬 (간단)

```yaml
---
name: deploy
description: 빌드 후 커밋하고 배포. "배포해줘" 요청 시 사용.
disable-model-invocation: true
---
```

```markdown
1. `npm run build`로 빌드. 실패 시 에러 수정 후 재시도.
2. `git status`와 `git diff --stat`으로 변경 확인.
3. 변경 파일만 `git add`. 민감 파일 제외.
4. 한글 커밋 메시지 작성 (HEREDOC).
5. `git push origin main`.
6. 배포 URL 안내.
```

## 2. 플랜 스킬 (인자 사용)

```yaml
---
name: plan
description: 요청사항을 plan.md에 정리. "플랜 작성해줘" 요청 시 사용.
argument-hint: [요청사항]
---
```

```markdown
1. 사용자 요청 분석: $ARGUMENTS
2. plan.md 읽기
3. "다음 작업 (구현 예정)" 섹션에 추가
4. 사용자에게 요약 반환
```

## 3. 콘텐츠 추가 스킬 (scripts 활용)

```
.claude/skills/content-add/
├── SKILL.md
└── scripts/
    └── generate.sh    # npm run generate 래핑
```

```yaml
---
name: content-add
description: 콘텐츠 .md 파일을 추가하고 데이터를 재생성. "콘텐츠 추가" 요청 시 사용.
---
```

## 4. API 연동 스킬 (references 활용)

```
.claude/skills/api-client/
├── SKILL.md
└── references/
    └── api-schema.md   # OpenAPI 스키마 요약
```

```yaml
---
name: api-client
description: 외부 API 엔드포인트를 호출하는 코드 작성. "API 연동" 요청 시 사용.
---
```

```markdown
API 스키마는 [references/api-schema.md](references/api-schema.md) 참고.
```

## 5. 컴포넌트 생성 스킬 (assets 활용)

```
.claude/skills/component/
├── SKILL.md
└── assets/
    ├── component-template.tsx
    └── story-template.tsx
```

```yaml
---
name: component
description: React 컴포넌트를 템플릿 기반으로 생성. "컴포넌트 만들어줘" 요청 시 사용.
argument-hint: [컴포넌트 이름]
---
```

```markdown
[assets/component-template.tsx](assets/component-template.tsx)를 복사하여 생성.
```