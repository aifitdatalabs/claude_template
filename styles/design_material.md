# Material 디자인 토큰

## 개요
Google Material Design 3 기반. Elevation으로 계층 표현, 명확한 구조.

## 색상

### Light Mode
```javascript
colors: {
  primary: '#6200EE',
  primaryLight: '#BB86FC',
  primaryDark: '#3700B3',
  secondary: '#03DAC6',
  secondaryDark: '#018786',

  success: '#4CAF50',
  warning: '#FF9800',
  error: '#B00020',

  background: '#FAFAFA',
  surface: '#FFFFFF',

  text: {
    primary: 'rgba(0, 0, 0, 0.87)',
    secondary: 'rgba(0, 0, 0, 0.6)',
    disabled: 'rgba(0, 0, 0, 0.38)',
  },

  divider: 'rgba(0, 0, 0, 0.12)',
  outline: 'rgba(0, 0, 0, 0.23)',
}
```

### Dark Mode
```javascript
colors: {
  primary: '#BB86FC',
  primaryLight: '#E1BEE7',
  primaryDark: '#6200EE',
  secondary: '#03DAC6',
  secondaryDark: '#018786',

  success: '#81C784',
  warning: '#FFB74D',
  error: '#CF6679',

  background: '#121212',
  surface: '#1E1E1E',
  surfaceVariant: '#2C2C2C',

  text: {
    primary: 'rgba(255, 255, 255, 0.87)',
    secondary: 'rgba(255, 255, 255, 0.6)',
    disabled: 'rgba(255, 255, 255, 0.38)',
  },

  divider: 'rgba(255, 255, 255, 0.12)',
  outline: 'rgba(255, 255, 255, 0.23)',
}
```

## 타이포그래피 (Material Type Scale)

```javascript
typography: {
  displayLarge: { fontSize: 57, lineHeight: 64, fontWeight: '400' },
  displayMedium: { fontSize: 45, lineHeight: 52, fontWeight: '400' },
  displaySmall: { fontSize: 36, lineHeight: 44, fontWeight: '400' },

  headlineLarge: { fontSize: 32, lineHeight: 40, fontWeight: '400' },
  headlineMedium: { fontSize: 28, lineHeight: 36, fontWeight: '400' },
  headlineSmall: { fontSize: 24, lineHeight: 32, fontWeight: '400' },

  titleLarge: { fontSize: 22, lineHeight: 28, fontWeight: '500' },
  titleMedium: { fontSize: 16, lineHeight: 24, fontWeight: '500', letterSpacing: 0.15 },
  titleSmall: { fontSize: 14, lineHeight: 20, fontWeight: '500', letterSpacing: 0.1 },

  bodyLarge: { fontSize: 16, lineHeight: 24, fontWeight: '400', letterSpacing: 0.5 },
  bodyMedium: { fontSize: 14, lineHeight: 20, fontWeight: '400', letterSpacing: 0.25 },
  bodySmall: { fontSize: 12, lineHeight: 16, fontWeight: '400', letterSpacing: 0.4 },

  labelLarge: { fontSize: 14, lineHeight: 20, fontWeight: '500', letterSpacing: 0.1 },
  labelMedium: { fontSize: 12, lineHeight: 16, fontWeight: '500', letterSpacing: 0.5 },
  labelSmall: { fontSize: 11, lineHeight: 16, fontWeight: '500', letterSpacing: 0.5 },
}
```

## 둥근 모서리

```javascript
borderRadius: {
  none: 0,
  xs: 4,
  sm: 8,
  md: 12,
  lg: 16,
  xl: 28,
  full: 9999,
}
```

### 용도별 권장값
| 요소 | 값 |
|------|-----|
| 버튼 | 20px (full for FAB) |
| 카드 | 12px (md) |
| 칩 | 8px (sm) |
| 다이얼로그 | 28px (xl) |
| 입력 필드 | 4px (xs) top only |

## Elevation (그림자)

```javascript
elevation: {
  0: {}, // flat
  1: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.18,
    shadowRadius: 1,
    elevation: 1,
  },
  2: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 2,
    elevation: 2,
  },
  3: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 3 },
    shadowOpacity: 0.22,
    shadowRadius: 3,
    elevation: 3,
  },
  4: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.22,
    shadowRadius: 4,
    elevation: 4,
  },
  6: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 6 },
    shadowOpacity: 0.24,
    shadowRadius: 6,
    elevation: 6,
  },
  8: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 8 },
    shadowOpacity: 0.24,
    shadowRadius: 8,
    elevation: 8,
  },
}
```

### Elevation 사용 가이드
| 요소 | Elevation |
|------|-----------|
| 카드 (기본) | 1 |
| 카드 (호버) | 2 |
| 앱바 | 4 |
| FAB | 6 |
| 네비게이션 드로어 | 8 |
| 다이얼로그/모달 | 8 |

## 간격

```javascript
spacing: {
  xs: 4,
  sm: 8,
  md: 16,
  lg: 24,
  xl: 32,
  xxl: 48,
}
```

## 컴포넌트 가이드

### 버튼
```
Filled: bg-primary, text-white, rounded-full (20px), py-2.5 px-6, elevation-2
Outlined: border-outline, text-primary, rounded-full
Text: text-primary만
FAB: rounded-full, w-14 h-14, elevation-6
```

### 카드
```
bg-surface, rounded-md (12px), elevation-1
호버/프레스: elevation-2
```

### 입력 필드 (Outlined)
```
border-outline, rounded-t-xs, py-4 px-4
focused: border-primary (2px)
label: 상단에 floating
```

### 칩
```
bg-surface, border-outline, rounded-sm (8px), px-3 py-1.5
selected: bg-primaryLight, border-primary
```

### 리스트 아이템
```
py-3 px-4
leading icon: w-10 h-10
trailing: 텍스트 또는 아이콘
divider: full width
```

## 특수 효과

### Ripple
```tsx
import { Pressable } from 'react-native';

<Pressable
  android_ripple={{ color: 'rgba(0,0,0,0.1)', borderless: false }}
>
  {/* content */}
</Pressable>
```

## CLAUDE.md 색상/타이포 섹션

```markdown
### 색상 규칙 (Material)
- Primary (#6200EE / #BB86FC): 주요 액션, FAB
- Secondary (#03DAC6): 보조 액션
- Surface (#FFFFFF / #1E1E1E): 카드, 시트 배경
- Background (#FAFAFA / #121212): 전체 배경
- Text Primary (87% opacity): 제목, 중요 텍스트
- Text Secondary (60% opacity): 본문
- Text Disabled (38% opacity): 비활성 텍스트

### 타이포그래피 (Material Type Scale)
- headlineMedium (28px): 화면 제목
- titleLarge (22px): 섹션 제목
- titleMedium (16px, 500): 카드 제목
- bodyLarge (16px): 본문
- labelLarge (14px, 500): 버튼 텍스트

### Elevation 사용
- elevation-1: 카드 기본
- elevation-2: 호버/프레스 상태
- elevation-4: 앱바
- elevation-6: FAB
- elevation-8: 모달, 다이얼로그
```
