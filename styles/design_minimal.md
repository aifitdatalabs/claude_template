# Minimal 디자인 토큰

## 개요
여백 중심, 흑백 기반의 깔끔한 디자인. 콘텐츠에 집중.

## 색상

### Light Mode
```javascript
colors: {
  primary: '#000000',
  primaryLight: '#333333',
  accent: '#0066FF', // 포인트 컬러 (최소 사용)

  success: '#22C55E',
  warning: '#F59E0B',
  error: '#EF4444',

  background: {
    primary: '#FFFFFF',
    secondary: '#FAFAFA',
    tertiary: '#F5F5F5',
  },

  text: {
    primary: '#0A0A0A',
    secondary: '#525252',
    tertiary: '#A3A3A3',
    placeholder: '#D4D4D4',
  },

  border: '#E5E5E5',
  divider: '#F0F0F0',
}
```

### Dark Mode
```javascript
colors: {
  primary: '#FFFFFF',
  primaryLight: '#E5E5E5',
  accent: '#3B82F6',

  success: '#4ADE80',
  warning: '#FBBF24',
  error: '#F87171',

  background: {
    primary: '#0A0A0A',
    secondary: '#141414',
    tertiary: '#1F1F1F',
  },

  text: {
    primary: '#FAFAFA',
    secondary: '#A3A3A3',
    tertiary: '#525252',
    placeholder: '#404040',
  },

  border: '#2E2E2E',
  divider: '#1F1F1F',
}
```

## 타이포그래피

```javascript
typography: {
  display: { fontSize: 48, lineHeight: 52, fontWeight: '700', letterSpacing: -1 },
  h1: { fontSize: 32, lineHeight: 40, fontWeight: '600', letterSpacing: -0.5 },
  h2: { fontSize: 24, lineHeight: 32, fontWeight: '600' },
  h3: { fontSize: 20, lineHeight: 28, fontWeight: '500' },
  h4: { fontSize: 18, lineHeight: 26, fontWeight: '500' },

  body: { fontSize: 16, lineHeight: 24, fontWeight: '400' },
  bodySmall: { fontSize: 14, lineHeight: 20, fontWeight: '400' },

  caption: { fontSize: 12, lineHeight: 16, fontWeight: '400' },
  overline: { fontSize: 10, lineHeight: 14, fontWeight: '500', letterSpacing: 1, textTransform: 'uppercase' },
}
```

## 둥근 모서리

```javascript
borderRadius: {
  none: 0,
  sm: 4,
  md: 8,
  lg: 12,
  full: 9999,
}
```

### 용도별 권장값
| 요소 | 값 |
|------|-----|
| 버튼 | 4px (sm) 또는 0 |
| 카드 | 0 또는 4px (sm) |
| 입력 필드 | 0 (underline) |
| 태그/배지 | full |
| 아바타 | full |

## 그림자

```javascript
// Minimal은 그림자 최소화
shadows: {
  sm: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.03,
    shadowRadius: 2,
    elevation: 1,
  },
}
// 대부분 border로 구분
```

## 간격

```javascript
spacing: {
  xs: 4,
  sm: 8,
  md: 16,
  lg: 24,
  xl: 32,
  xxl: 48,
  xxxl: 64,
}
```

### Minimal 간격 원칙
- 여백을 넉넉하게 사용
- 화면 좌우 패딩: 24px ~ 32px
- 섹션 간 간격: 48px ~ 64px
- 요소 간 간격: 16px ~ 24px

## 컴포넌트 가이드

### 버튼
```
Primary: bg-primary, text-white (반전), py-3 px-6, border-0
Secondary: border border-primary, text-primary, bg-transparent
Text: text-primary 만, underline on hover
```

### 카드
```
border border-border (그림자 대신 보더)
또는 배경색 차이로 구분 (bg-secondary)
padding 넉넉하게 (p-6)
```

### 입력 필드
```
Underline 스타일: border-b border-border
label 위에 작게
focused: border-primary
```

### 리스트 아이템
```
py-4 (여유있게)
border-b border-divider
아이콘 최소화
```

### 구분선
```
h-px bg-divider (얇은 선)
또는 여백으로 구분 (선 없이)
```

## 디자인 원칙

1. **여백 우선**: 요소 간 충분한 간격
2. **흑백 기반**: 포인트 색상 최소 사용
3. **장식 제거**: 불필요한 그림자, 그라데이션 없음
4. **콘텐츠 중심**: 타이포그래피로 계층 표현
5. **일관성**: 동일한 간격, 동일한 두께

## CLAUDE.md 색상/타이포 섹션

```markdown
### 색상 규칙 (Minimal)
- Primary (#000000 / #FFFFFF): 주요 액션, 강조
- Accent (#0066FF): 링크, 포인트 (최소 사용)
- Background Primary (#FFFFFF / #0A0A0A): 메인 배경
- Border (#E5E5E5 / #2E2E2E): 구분선, 카드 테두리
- Text Primary: 제목, 강조
- Text Secondary (#525252 / #A3A3A3): 본문
- Text Tertiary (#A3A3A3 / #525252): 힌트, 메타 정보

### 타이포그래피
- h1 (32px, 600): 화면 제목
- h2 (24px, 600): 섹션 제목
- h3 (20px, 500): 서브 제목
- body (16px): 본문
- bodySmall (14px): 보조 텍스트
- caption (12px): 메타 정보

### 스타일 원칙
- 여백 넉넉하게 (p-6, gap-6)
- 그림자 대신 border 사용
- 포인트 색상 최소화
- 콘텐츠 중심
```
