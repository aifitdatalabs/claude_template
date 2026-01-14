# Tamagui 설정 가이드

## 개요
고성능 UI 라이브러리. 테마 시스템 + 컴포넌트 내장.

## 설치

```bash
npm install tamagui @tamagui/config @tamagui/babel-plugin
```

## 설정 파일

### tamagui.config.ts
```typescript
import { createTamagui, createTokens } from 'tamagui';
import { createInterFont } from '@tamagui/font-inter';

// 선택한 디자인 스타일의 토큰 적용
const tokens = createTokens({
  color: {
    primary: '#007AFF',
    primaryLight: '#5AC8FA',
    primaryDark: '#0A84FF',

    backgroundPrimary: '#FFFFFF',
    backgroundSecondary: '#F2F2F7',
    backgroundPrimaryDark: '#000000',
    backgroundSecondaryDark: '#1C1C1E',

    textPrimary: '#000000',
    textSecondary: '#3C3C43',
    textTertiary: '#8E8E93',
    textPrimaryDark: '#FFFFFF',
    textSecondaryDark: '#EBEBF5',
  },
  space: {
    xs: 4,
    sm: 8,
    md: 16,
    lg: 24,
    xl: 32,
    true: 16, // default
  },
  size: {
    sm: 8,
    md: 16,
    lg: 24,
    xl: 32,
    true: 16,
  },
  radius: {
    xs: 4,
    sm: 8,
    md: 12,
    lg: 16,
    xl: 20,
    full: 9999,
    true: 12,
  },
});

const headingFont = createInterFont({
  weight: {
    6: '600',
    7: '700',
  },
});

const bodyFont = createInterFont();

const config = createTamagui({
  tokens,
  fonts: {
    heading: headingFont,
    body: bodyFont,
  },
  themes: {
    light: {
      background: tokens.color.backgroundPrimary,
      backgroundSecondary: tokens.color.backgroundSecondary,
      color: tokens.color.textPrimary,
      colorSecondary: tokens.color.textSecondary,
      colorTertiary: tokens.color.textTertiary,
      primary: tokens.color.primary,
    },
    dark: {
      background: tokens.color.backgroundPrimaryDark,
      backgroundSecondary: tokens.color.backgroundSecondaryDark,
      color: tokens.color.textPrimaryDark,
      colorSecondary: tokens.color.textSecondaryDark,
      colorTertiary: tokens.color.textTertiary,
      primary: tokens.color.primaryDark,
    },
  },
});

export type AppConfig = typeof config;

declare module 'tamagui' {
  interface TamaguiCustomConfig extends AppConfig {}
}

export default config;
```

### babel.config.js
```javascript
module.exports = {
  presets: ['module:@react-native/babel-preset'],
  plugins: [
    [
      '@tamagui/babel-plugin',
      {
        components: ['tamagui'],
        config: './tamagui.config.ts',
      },
    ],
  ],
};
```

## 사용법

### App.tsx
```tsx
import { TamaguiProvider, Theme } from 'tamagui';
import config from './tamagui.config';

export const App = () => {
  const [isDark, setIsDark] = useState(false);

  return (
    <TamaguiProvider config={config}>
      <Theme name={isDark ? 'dark' : 'light'}>
        <RootNavigator />
      </Theme>
    </TamaguiProvider>
  );
};
```

### 기본 사용
```tsx
import { YStack, XStack, Text, Button } from 'tamagui';

export const MyComponent = () => (
  <YStack backgroundColor="$background" padding="$md" flex={1}>
    <Text fontSize={22} fontWeight="700" color="$color">
      제목
    </Text>
    <Text fontSize={17} color="$colorSecondary">
      본문 텍스트
    </Text>
    <XStack gap="$sm" marginTop="$md">
      <Button theme="blue">확인</Button>
      <Button variant="outlined">취소</Button>
    </XStack>
  </YStack>
);
```

### 카드 컴포넌트
```tsx
import { YStack, Text } from 'tamagui';

export const Card = ({ children }) => (
  <YStack
    backgroundColor="$backgroundSecondary"
    padding="$md"
    borderRadius="$xl"
    shadowColor="$color"
    shadowOffset={{ width: 0, height: 2 }}
    shadowOpacity={0.1}
    shadowRadius={8}
    elevation={3}
  >
    {children}
  </YStack>
);
```

### 입력 필드
```tsx
import { Input, Label, YStack } from 'tamagui';

export const FormField = () => (
  <YStack gap="$xs">
    <Label htmlFor="email">이메일</Label>
    <Input
      id="email"
      placeholder="이메일 입력"
      backgroundColor="$backgroundSecondary"
      borderRadius="$md"
      padding="$sm"
    />
  </YStack>
);
```

### 테마 토글
```tsx
import { useThemeName } from 'tamagui';

export const ThemeToggle = () => {
  const themeName = useThemeName();
  // 부모에서 Theme name 변경으로 전환
};
```

## CLAUDE.md 스타일 섹션

```markdown
## 스타일 가이드

### 스타일링
- 라이브러리: Tamagui
- 디자인 스타일: [선택한 디자인 스타일]

### 컴포넌트
- YStack: 세로 레이아웃 (View flex-direction: column)
- XStack: 가로 레이아웃 (View flex-direction: row)
- Text: 텍스트
- Button, Input: 내장 컴포넌트

### 토큰 사용
- 색상: $background, $color, $primary
- 간격: $xs, $sm, $md, $lg, $xl
- 둥근 모서리: $sm, $md, $lg, $xl

### 사용 규칙
- 토큰은 $ 프리픽스로 참조
- 테마 색상은 시맨틱 이름 사용 ($background, $color)
- Theme 컴포넌트로 다크 모드 전환
```
