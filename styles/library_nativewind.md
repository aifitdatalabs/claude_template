# NativeWind 설정 가이드

## 개요
Tailwind CSS 문법을 React Native에서 사용. 클래스 기반 빠른 스타일링.

## 설치

```bash
npm install nativewind
npm install -D tailwindcss
npx tailwindcss init
```

## 설정 파일

### tailwind.config.js
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ['./App.{js,jsx,ts,tsx}', './src/**/*.{js,jsx,ts,tsx}'],
  presets: [require('nativewind/preset')],
  theme: {
    extend: {
      // 선택한 디자인 스타일의 토큰 적용
      colors: {
        // design_[style].md 참고
      },
      fontSize: {
        // design_[style].md 참고
      },
      borderRadius: {
        // design_[style].md 참고
      },
    },
  },
  plugins: [],
};
```

### babel.config.js
```javascript
module.exports = {
  presets: [
    ['module:@react-native/babel-preset', { unstable_transformProfile: 'hermes-stable' }],
    'nativewind/babel',
  ],
};
```

### metro.config.js
```javascript
const { getDefaultConfig, mergeConfig } = require('@react-native/metro-config');
const { withNativeWind } = require('nativewind/metro');

const config = mergeConfig(getDefaultConfig(__dirname), {});

module.exports = withNativeWind(config, { input: './global.css' });
```

### global.css
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### App.tsx에서 import
```typescript
import './global.css';
```

## 사용법

### 기본 사용
```tsx
<View className="bg-background-primary p-4">
  <Text className="text-title2 text-text-primary">제목</Text>
</View>
```

### 다크 모드
```tsx
<View className="bg-background-primary dark:bg-backgroundDark-primary">
  <Text className="text-text-primary dark:text-textDark-primary">
    텍스트
  </Text>
</View>
```

### useColorScheme 활용
```tsx
import { useColorScheme } from 'nativewind';

export const ThemeToggle = () => {
  const { colorScheme, toggleColorScheme } = useColorScheme();

  return (
    <TouchableOpacity onPress={toggleColorScheme}>
      <Text>{colorScheme === 'dark' ? '라이트 모드' : '다크 모드'}</Text>
    </TouchableOpacity>
  );
};
```

### 그림자 (별도 처리 필요)
```tsx
// NativeWind에서 그림자는 직접 스타일 적용
import { shadows } from '@/utils/shadows';

<View className="bg-white rounded-xl p-4" style={shadows.md}>
  <Text>카드</Text>
</View>
```

### src/utils/shadows.ts
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

## CLAUDE.md 스타일 섹션

```markdown
## 스타일 가이드

### 스타일링
- 라이브러리: NativeWind
- 디자인 스타일: [선택한 디자인 스타일]

### 사용 규칙
- className으로 Tailwind 클래스 적용
- 다크 모드: dark: 프리픽스 사용
- 그림자: style prop으로 shadows 유틸 적용
- 커스텀 값보다 테마 토큰 클래스 우선 사용

### 자주 쓰는 클래스
- 레이아웃: flex-1, flex-row, items-center, justify-between
- 간격: p-4, px-4, py-3, m-2, gap-2
- 색상: bg-[color], text-[color]
- 둥근 모서리: rounded-sm, rounded-md, rounded-xl
- 크기: w-full, h-10, w-10
```
