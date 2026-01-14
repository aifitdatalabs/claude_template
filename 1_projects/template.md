# [프로젝트명]

---

## 프로젝트 정보

### 개요
- **설명**: [프로젝트 설명]
- **플랫폼**: React Native (Bare / CLI)

### 환경 설정
```env
[환경변수]
```

### 기술 스택
- **프레임워크**: React Native CLI
- **상태관리**: [Zustand / Redux / Jotai]
- **스타일링**: [StyleSheet / NativeWind / Restyle / Tamagui]
- **디자인 스타일**: [iOS Native / Material / Minimal / Soft UI]
- **네비게이션**: React Navigation
- **웹 디버깅**: react-native-web (선택)

---

## 개발 규칙

### 프로젝트 구조
```
project-root/
├── android/
├── ios/
├── web/                    # react-native-web 사용 시
│   └── index.html
├── docs/                   # 구조 문서
│   └── structure/
│       ├── app.md          # 앱 전체 구조
│       ├── tabs/
│       │   └── layout.md   # 탭 네비게이션 레이아웃
│       ├── home/
│       │   └── layout.md   # 홈 화면 레이아웃
│       └── [feature]/
│           └── layout.md   # 기능별 레이아웃
├── src/
│   ├── navigation/
│   ├── screens/
│   ├── features/
│   ├── components/
│   ├── hooks/
│   ├── services/
│   ├── stores/
│   ├── theme/
│   ├── types/
│   ├── utils/
│   └── constants/
├── assets/
├── index.js
├── index.web.js            # 웹 진입점
├── App.tsx
├── CLAUDE.md
├── webpack.config.js       # 웹 번들러 설정
├── tailwind.config.js      # NativeWind 사용 시
├── package.json
└── tsconfig.json
```

### 폴더 역할
| 폴더 | 역할 |
|------|------|
| `docs/structure/` | 화면/기능별 레이아웃 문서 |
| `screens/` | 네비게이션 연결 페이지 |
| `features/` | 도메인별 로직 + 전용 컴포넌트 |
| `components/` | 공통 UI 컴포넌트 |
| `theme/` | 테마 토큰, 색상, 타이포그래피 |

### 코드 스타일
- TypeScript strict mode
- 함수형 컴포넌트 + hooks (클래스형 금지)
- Named export (export default 금지)
- 서비스 추상화 인터페이스 사용

### 네이밍 규칙
| 종류 | 규칙 | 예시 |
|------|------|------|
| 컴포넌트 | PascalCase.tsx | `UserCard.tsx` |
| 화면 | PascalCaseScreen.tsx | `ProfileScreen.tsx` |
| hooks | useCamelCase.ts | `useAuth.ts` |
| 서비스 | camelCaseService.ts | `authService.ts` |

### 경로 alias
```
@/*  → src/*
```

---

## 스타일 가이드 선택

### 1단계: 스타일링 라이브러리 선택 → `guides/styles/library_[name].md`
| 옵션 | 파일 |
|------|------|
| StyleSheet (기본) | `library_stylesheet.md` |
| NativeWind(추천) | `library_nativewind.md` |
| Restyle | `library_restyle.md` |
| Tamagui | `library_tamagui.md` |

### 2단계: 디자인 스타일 선택 → `guides/styles/design_[name].md`
| 옵션 | 파일 |
|------|------|
| iOS Native | `design_ios.md` |
| Material | `design_material.md` |
| Minimal | `design_minimal.md` |
| Soft UI | `design_softui.md` |

### 3단계: CLAUDE.md 작성
1. 아래 **기본 템플릿** 복사
2. 선택한 라이브러리 파일에서 **CLAUDE.md 스타일 섹션** 복사
3. 선택한 디자인 파일에서 **색상/타이포 섹션** 복사하여 병합

---

## CLAUDE.md 기본 템플릿

```markdown
# 프로젝트 규칙

## 코드 스타일
- TypeScript strict mode 사용
- 함수형 컴포넌트 + hooks만 사용 (클래스형 컴포넌트 금지)
- Named export만 사용 (export default 금지)
- 서비스는 추상화 인터페이스 사용

## 네이밍 규칙
- 컴포넌트: PascalCase.tsx (예: UserCard.tsx)
- 화면: PascalCaseScreen.tsx (예: ProfileScreen.tsx)
- hooks: useCamelCase.ts (예: useAuth.ts)
- 서비스: camelCaseService.ts (예: authService.ts)
- 타입: PascalCase, 접미사 없음 (예: User, Post)

## 폴더 구조
- screens/: 네비게이션에 연결되는 페이지 컴포넌트만
- features/[feature]/: 도메인별 비즈니스 로직 + 전용 컴포넌트
- components/: 여러 곳에서 재사용되는 공통 UI 컴포넌트

## 경로 alias
- @/* → src/*

## 타입 정의
- 공유 타입: src/types/
- 기능별 타입: src/features/[feature]/types.ts
- 타입 먼저 정의 후 구현

## 스타일 가이드
[선택한 library_*.md의 CLAUDE.md 섹션 + design_*.md의 색상/타이포 섹션 붙여넣기]

## docs/structure 작성
> 상세 템플릿: `guides/1_projects/templates/docs_structure.md`

```
docs/structure/
├── app.md              # 앱 전체 구조 (네비게이션, 주요 화면)
├── tabs/layout.md      # 탭 네비게이션
└── [feature]/layout.md # 기능별 (시각적 레이아웃 + 컴포넌트 트리)
```
