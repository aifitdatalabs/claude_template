# Restyle (Shopify) 설정 가이드

## 개요
Shopify에서 만든 타입 안전한 테마 기반 스타일링 라이브러리.

## 설치

```bash
npm install @shopify/restyle
```

## 설정 파일

### src/theme/index.ts
```typescript
import { createTheme } from '@shopify/restyle';

// 선택한 디자인 스타일의 토큰 적용
const palette = {
  // design_[style].md 참고
  primary: '#007AFF',
  primaryLight: '#5AC8FA',
  primaryDark: '#0A84FF',

  backgroundPrimary: '#FFFFFF',
  backgroundSecondary: '#F2F2F7',

  textPrimary: '#000000',
  textSecondary: '#3C3C43',
  textTertiary: '#8E8E93',

  // Dark mode
  backgroundPrimaryDark: '#000000',
  backgroundSecondaryDark: '#1C1C1E',
  textPrimaryDark: '#FFFFFF',
  textSecondaryDark: '#EBEBF5',
};

const theme = createTheme({
  colors: {
    primary: palette.primary,
    primaryLight: palette.primaryLight,
    backgroundPrimary: palette.backgroundPrimary,
    backgroundSecondary: palette.backgroundSecondary,
    textPrimary: palette.textPrimary,
    textSecondary: palette.textSecondary,
    textTertiary: palette.textTertiary,
  },
  spacing: {
    xs: 4,
    s: 8,
    m: 16,
    l: 24,
    xl: 32,
  },
  borderRadii: {
    xs: 4,
    sm: 8,
    md: 12,
    lg: 16,
    xl: 20,
    full: 9999,
  },
  textVariants: {
    defaults: {},
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
  },
  cardVariants: {
    defaults: {
      padding: 'm',
      borderRadius: 'xl',
      backgroundColor: 'backgroundSecondary',
    },
    elevated: {
      shadowColor: 'textPrimary',
      shadowOffset: { width: 0, height: 2 },
      shadowOpacity: 0.1,
      shadowRadius: 8,
      elevation: 3,
    },
  },
});

export type Theme = typeof theme;

// Dark theme
export const darkTheme: Theme = {
  ...theme,
  colors: {
    ...theme.colors,
    backgroundPrimary: palette.backgroundPrimaryDark,
    backgroundSecondary: palette.backgroundSecondaryDark,
    textPrimary: palette.textPrimaryDark,
    textSecondary: palette.textSecondaryDark,
  },
};

export default theme;
```

### src/theme/components.ts
```typescript
import { createBox, createText } from '@shopify/restyle';
import { Theme } from './index';

export const Box = createBox<Theme>();
export const Text = createText<Theme>();
```

## 사용법

### App.tsx
```tsx
import { ThemeProvider } from '@shopify/restyle';
import theme from '@/theme';

export const App = () => (
  <ThemeProvider theme={theme}>
    <RootNavigator />
  </ThemeProvider>
);
```

### 컴포넌트에서 사용
```tsx
import { Box, Text } from '@/theme/components';

export const MyComponent = () => (
  <Box backgroundColor="backgroundPrimary" padding="m">
    <Text variant="title2" color="textPrimary">제목</Text>
    <Text variant="body" color="textSecondary">본문</Text>
  </Box>
);
```

### 카드 컴포넌트
```tsx
<Box variant="elevated" backgroundColor="backgroundSecondary" padding="m" borderRadius="xl">
  <Text variant="headline" color="textPrimary">카드 제목</Text>
</Box>
```

### 다크 모드 전환
```tsx
import { ThemeProvider } from '@shopify/restyle';
import theme, { darkTheme } from '@/theme';

const App = () => {
  const [isDark, setIsDark] = useState(false);

  return (
    <ThemeProvider theme={isDark ? darkTheme : theme}>
      {/* ... */}
    </ThemeProvider>
  );
};
```

### useTheme 사용
```tsx
import { useTheme } from '@shopify/restyle';
import { Theme } from '@/theme';

export const MyComponent = () => {
  const theme = useTheme<Theme>();

  return (
    <View style={{ padding: theme.spacing.m }}>
      {/* ... */}
    </View>
  );
};
```

## CLAUDE.md 스타일 섹션

```markdown
## 스타일 가이드

### 스타일링
- 라이브러리: Restyle (Shopify)
- 디자인 스타일: [선택한 디자인 스타일]

### 컴포넌트
- Box: 레이아웃 컴포넌트 (View 대체)
- Text: 텍스트 컴포넌트

### 사용 규칙
- Box/Text에 테마 속성 직접 전달
- backgroundColor="backgroundPrimary" 형식
- variant로 미리 정의된 스타일 적용
- spacing, borderRadii는 테마 키 사용

### 예시
- <Box backgroundColor="backgroundPrimary" padding="m">
- <Text variant="title2" color="textPrimary">
- <Box borderRadius="lg" marginTop="s">
```
