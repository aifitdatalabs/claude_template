# iOS Native 디자인 토큰

## 개요
Apple Human Interface Guidelines 기반. 시스템 느낌의 깔끔한 디자인.

## 색상

### Light Mode
```javascript
colors: {
  primary: '#007AFF',
  primaryLight: '#5AC8FA',
  secondary: '#5856D6',
  secondaryLight: '#AF52DE',

  success: '#34C759',
  warning: '#FF9500',
  error: '#FF3B30',

  background: {
    primary: '#FFFFFF',
    secondary: '#F2F2F7',
    tertiary: '#FFFFFF',
  },

  text: {
    primary: '#000000',
    secondary: '#3C3C43',
    tertiary: '#8E8E93',
    quaternary: '#C7C7CC',
  },

  separator: 'rgba(60, 60, 67, 0.36)',
  fill: 'rgba(120, 120, 128, 0.2)',
}
```

### Dark Mode
```javascript
colors: {
  primary: '#0A84FF',
  primaryLight: '#64D2FF',
  secondary: '#5E5CE6',
  secondaryLight: '#BF5AF2',

  success: '#30D158',
  warning: '#FF9F0A',
  error: '#FF453A',

  background: {
    primary: '#000000',
    secondary: '#1C1C1E',
    tertiary: '#2C2C2E',
  },

  text: {
    primary: '#FFFFFF',
    secondary: '#EBEBF5',
    tertiary: '#8E8E93',
    quaternary: '#636366',
  },

  separator: 'rgba(84, 84, 88, 0.65)',
  fill: 'rgba(120, 120, 128, 0.36)',
}
```

## 타이포그래피 (iOS Dynamic Type)

```javascript
typography: {
  largeTitle: { fontSize: 34, lineHeight: 41, fontWeight: '700' },
  title1: { fontSize: 28, lineHeight: 34, fontWeight: '700' },
  title2: { fontSize: 22, lineHeight: 28, fontWeight: '700' },
  title3: { fontSize: 20, lineHeight: 25, fontWeight: '600' },
  headline: { fontSize: 17, lineHeight: 22, fontWeight: '600' },
  body: { fontSize: 17, lineHeight: 22, fontWeight: '400' },
  callout: { fontSize: 16, lineHeight: 21, fontWeight: '400' },
  subhead: { fontSize: 15, lineHeight: 20, fontWeight: '400' },
  footnote: { fontSize: 13, lineHeight: 18, fontWeight: '400' },
  caption1: { fontSize: 12, lineHeight: 16, fontWeight: '400' },
  caption2: { fontSize: 11, lineHeight: 13, fontWeight: '400' },
}
```

## 둥근 모서리

```javascript
borderRadius: {
  xs: 4,
  sm: 8,
  md: 12,
  lg: 16,
  xl: 20,
  '2xl': 24,
  full: 9999,
}
```

### 용도별 권장값
| 요소 | 값 |
|------|-----|
| 작은 버튼/태그 | 8px (sm) |
| 입력 필드 | 12px (md) |
| 카드 | 20px (xl) |
| 모달/시트 | 24px (2xl) |
| 아바타 | full |

## 그림자

```javascript
shadows: {
  sm: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.05,
    shadowRadius: 2,
    elevation: 1,
  },
  md: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 8,
    elevation: 3,
  },
  lg: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.15,
    shadowRadius: 16,
    elevation: 5,
  },
}
```

## 간격

```javascript
spacing: {
  xs: 4,
  sm: 8,
  md: 16,
  lg: 24,
  xl: 32,
}
```

### iOS 표준 간격
- 화면 좌우 패딩: 16px
- 리스트 아이템 세로 패딩: 12px
- 섹션 간 간격: 32px
- 그룹 내 아이템 간격: 8px

## 컴포넌트 가이드

### 버튼
```
Primary: bg-primary, text-white, rounded-md (12px), py-3 px-6
Secondary: text-primary (텍스트만)
Destructive: text-error
```

### 카드
```
bg-background-secondary, rounded-xl (20px), p-4, shadow-md
```

### 리스트 아이템
```
py-3, 좌측 아이콘 (w-10 h-10 rounded-full), chevron-right
구분선: h-px, ml-16 (아이콘 너비 + 간격)
```

### 입력 필드
```
bg-background-secondary, rounded-md (12px), px-4 py-3
placeholder: text-tertiary
```

### 네비게이션 바
```
bg-background-primary/80 + blur, border-b separator
largeTitle 사용
```

## 특수 효과

### Blur (iOS Vibrancy)
```tsx
import { BlurView } from '@react-native-community/blur';

<BlurView
  blurType="light" // 'light' | 'dark' | 'xlight'
  blurAmount={10}
/>
```

## CLAUDE.md 색상/타이포 섹션

```markdown
### 색상 규칙 (iOS Native)
- Primary (#007AFF): 주요 액션, 링크
- Background Primary (#FFFFFF / #000000): 메인 배경
- Background Secondary (#F2F2F7 / #1C1C1E): 카드, 섹션
- Text Primary: 제목
- Text Secondary: 본문
- Text Tertiary (#8E8E93): 보조 정보, 힌트

### 타이포그래피 (iOS Dynamic Type)
- largeTitle (34px): 화면 대제목
- title2 (22px): 섹션 제목
- headline (17px, 600): 강조 텍스트
- body (17px): 본문
- footnote (13px): 보조 정보, 타임스탬프
```
