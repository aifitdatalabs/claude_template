# StyleSheet (기본) 설정 가이드

## 개요
React Native 내장 StyleSheet 사용. 별도 의존성 없음.

## 테마 구조

### 폴더 구조
```
src/theme/
├── colors.ts       # 색상 정의
├── typography.ts   # 폰트 스타일
├── spacing.ts      # 간격
├── shadows.ts      # 그림자
├── index.ts        # 통합 export
└── ThemeContext.tsx # 테마 Context
```

### colors.ts
```typescript
// 선택한 디자인 스타일의 색상 토큰 적용
export const colors = {
  light: {
    primary: '#007AFF',
    background: { primary: '#FFFFFF', secondary: '#F2F2F7' },
    text: { primary: '#000000', secondary: '#3C3C43', tertiary: '#8E8E93' },
    // ...
  },
  dark: {
    primary: '#0A84FF',
    background: { primary: '#000000', secondary: '#1C1C1E' },
    text: { primary: '#FFFFFF', secondary: '#EBEBF5', tertiary: '#8E8E93' },
    // ...
  },
};
```

### typography.ts
```typescript
// 선택한 디자인 스타일의 타이포그래피 적용
export const typography = {
  largeTitle: { fontSize: 34, fontWeight: '700' as const, lineHeight: 41 },
  title1: { fontSize: 28, fontWeight: '700' as const, lineHeight: 34 },
  title2: { fontSize: 22, fontWeight: '700' as const, lineHeight: 28 },
  body: { fontSize: 17, fontWeight: '400' as const, lineHeight: 22 },
  // ...
};
```

### spacing.ts
```typescript
export const spacing = {
  xs: 4,
  sm: 8,
  md: 16,
  lg: 24,
  xl: 32,
};
```

### shadows.ts
```typescript
export const shadows = {
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
};
```

### ThemeContext.tsx
```typescript
import { createContext, useContext, useState, ReactNode } from 'react';
import { colors } from './colors';
import { typography } from './typography';
import { spacing } from './spacing';
import { shadows } from './shadows';

type ThemeMode = 'light' | 'dark';

const themes = {
  light: { colors: colors.light, typography, spacing, shadows },
  dark: { colors: colors.dark, typography, spacing, shadows },
};

type Theme = typeof themes.light;

interface ThemeContextType {
  theme: Theme;
  mode: ThemeMode;
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

export const ThemeProvider = ({ children }: { children: ReactNode }) => {
  const [mode, setMode] = useState<ThemeMode>('light');
  const toggleTheme = () => setMode(m => m === 'light' ? 'dark' : 'light');

  return (
    <ThemeContext.Provider value={{ theme: themes[mode], mode, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

export const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) throw new Error('useTheme must be used within ThemeProvider');
  return context;
};
```

### index.ts
```typescript
export * from './colors';
export * from './typography';
export * from './spacing';
export * from './shadows';
export * from './ThemeContext';
```

## 사용법

### App.tsx
```typescript
import { ThemeProvider } from '@/theme';

export const App = () => (
  <ThemeProvider>
    <RootNavigator />
  </ThemeProvider>
);
```

### 컴포넌트에서 사용
```typescript
import { StyleSheet, View, Text } from 'react-native';
import { useTheme } from '@/theme';

export const MyComponent = () => {
  const { theme } = useTheme();

  return (
    <View style={[styles.container, { backgroundColor: theme.colors.background.primary }]}>
      <Text style={[styles.title, { color: theme.colors.text.primary }]}>
        제목
      </Text>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    padding: 16,
  },
  title: {
    fontSize: 22,
    fontWeight: '700',
  },
});
```

## CLAUDE.md 스타일 섹션

```markdown
## 스타일 가이드

### 스타일링
- 라이브러리: StyleSheet (기본)
- 디자인 스타일: [선택한 디자인 스타일]

### 테마 구조
- src/theme/colors.ts: 색상 정의
- src/theme/typography.ts: 폰트 스타일
- src/theme/spacing.ts: 간격
- src/theme/shadows.ts: 그림자
- src/theme/ThemeContext.tsx: 테마 Provider

### 사용 규칙
- useTheme() 훅으로 테마 접근
- StyleSheet.create()로 정적 스타일 정의
- 동적 색상은 style prop 배열로 적용
- 테마 토큰 직접 사용 금지, 항상 theme 객체 통해 접근
```
