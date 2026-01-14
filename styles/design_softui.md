# Soft UI (Neumorphism) 디자인 토큰

## 개요
뉴모피즘 스타일. 부드러운 볼록/오목 효과, 파스텔 배경.

## 색상

### Light Mode
```javascript
colors: {
  primary: '#6366F1',
  primaryLight: '#818CF8',
  primaryDark: '#4F46E5',
  secondary: '#EC4899',
  secondaryLight: '#F472B6',

  success: '#10B981',
  warning: '#F59E0B',
  error: '#EF4444',

  // 배경색 (그림자 색상 결정에 중요)
  background: '#E8EDF5',
  backgroundDark: '#D1D9E6',
  backgroundLight: '#FFFFFF',

  text: {
    primary: '#1E293B',
    secondary: '#64748B',
    tertiary: '#94A3B8',
  },

  // Neumorphic 그림자 색상
  shadow: {
    dark: '#A3B1C6',
    light: '#FFFFFF',
  },
}
```

### Dark Mode
```javascript
colors: {
  primary: '#818CF8',
  primaryLight: '#A5B4FC',
  primaryDark: '#6366F1',
  secondary: '#F472B6',
  secondaryLight: '#F9A8D4',

  success: '#34D399',
  warning: '#FBBF24',
  error: '#F87171',

  background: '#2D3748',
  backgroundDark: '#1A202C',
  backgroundLight: '#3D4A5C',

  text: {
    primary: '#F1F5F9',
    secondary: '#94A3B8',
    tertiary: '#64748B',
  },

  shadow: {
    dark: '#1A202C',
    light: '#3D4A5C',
  },
}
```

## 타이포그래피

```javascript
typography: {
  h1: { fontSize: 28, lineHeight: 36, fontWeight: '700' },
  h2: { fontSize: 24, lineHeight: 32, fontWeight: '600' },
  h3: { fontSize: 20, lineHeight: 28, fontWeight: '600' },
  h4: { fontSize: 18, lineHeight: 26, fontWeight: '500' },

  body: { fontSize: 16, lineHeight: 24, fontWeight: '400' },
  bodySmall: { fontSize: 14, lineHeight: 20, fontWeight: '400' },

  caption: { fontSize: 12, lineHeight: 16, fontWeight: '400' },
  button: { fontSize: 16, lineHeight: 24, fontWeight: '600' },
}
```

## 둥근 모서리

```javascript
borderRadius: {
  sm: 12,
  md: 16,
  lg: 24,
  xl: 32,
  full: 9999,
}
```

### 용도별 권장값
| 요소 | 값 |
|------|-----|
| 버튼 | 16px (md) |
| 카드 | 24px (lg) |
| 입력 필드 | 16px (md) |
| 아이콘 버튼 | full |
| 모달 | 32px (xl) |

## Neumorphic 그림자

### 볼록 효과 (Raised)
```javascript
// Light Mode
neumorphRaised: {
  // 어두운 그림자 (우하단)
  shadowColor: '#A3B1C6',
  shadowOffset: { width: 6, height: 6 },
  shadowOpacity: 1,
  shadowRadius: 12,
  elevation: 5,
  // + 별도 View로 밝은 그림자 (좌상단) 구현 필요
}

// 밝은 그림자용 별도 스타일
neumorphLight: {
  shadowColor: '#FFFFFF',
  shadowOffset: { width: -6, height: -6 },
  shadowOpacity: 1,
  shadowRadius: 12,
}
```

### 오목 효과 (Inset)
```javascript
// Inset은 inner shadow로 구현 어려움
// 대안: 배경색 + border로 표현
neumorphInset: {
  backgroundColor: '#D1D9E6', // 약간 어두운 배경
  borderWidth: 1,
  borderColor: '#FFFFFF', // 상단/좌측만 밝게 (부분 적용 필요)
}
```

### 유틸리티 함수
```typescript
// src/utils/neumorphism.ts
export const createNeumorphStyle = (
  backgroundColor: string,
  darkShadow: string,
  lightShadow: string,
  type: 'raised' | 'inset' = 'raised'
) => {
  if (type === 'raised') {
    return {
      backgroundColor,
      shadowColor: darkShadow,
      shadowOffset: { width: 6, height: 6 },
      shadowOpacity: 1,
      shadowRadius: 12,
      elevation: 5,
    };
  }
  // inset은 별도 구현
  return {
    backgroundColor: darkShadow,
  };
};
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
}
```

## 컴포넌트 가이드

### 버튼
```
볼록 효과 (raised)
bg-background, rounded-md (16px), py-3 px-6
neumorphic shadow
pressed: 오목 효과로 전환
```

### 카드
```
bg-background, rounded-lg (24px), p-6
neumorphic shadow (raised)
```

### 입력 필드
```
오목 효과 (inset)
bg-backgroundDark, rounded-md (16px), px-4 py-3
또는 inner border로 표현
```

### 아이콘 버튼
```
bg-background, rounded-full, w-12 h-12
neumorphic shadow (raised)
아이콘 색상: text-primary 또는 primary
```

### 토글/스위치
```
track: 오목 효과
thumb: 볼록 효과 + 이동
```

## 구현 팁

### 이중 View로 두 그림자 구현
```tsx
<View style={styles.outerShadow}>
  <View style={styles.innerShadow}>
    <View style={styles.content}>
      {/* 내용 */}
    </View>
  </View>
</View>
```

### 색상 조화
- 배경색과 그림자 색상이 조화를 이뤄야 함
- 어두운 그림자: 배경보다 15-20% 어둡게
- 밝은 그림자: 배경보다 15-20% 밝게 (또는 흰색)

## CLAUDE.md 색상/타이포 섹션

```markdown
### 색상 규칙 (Soft UI)
- Primary (#6366F1 / #818CF8): 주요 액션, 강조
- Background (#E8EDF5 / #2D3748): 메인 배경 (그림자 기준)
- Shadow Dark (#A3B1C6 / #1A202C): 어두운 그림자
- Shadow Light (#FFFFFF / #3D4A5C): 밝은 그림자
- Text Primary (#1E293B / #F1F5F9): 제목
- Text Secondary (#64748B / #94A3B8): 본문

### 타이포그래피
- h1 (28px, 700): 화면 제목
- h2 (24px, 600): 섹션 제목
- h3 (20px, 600): 서브 제목
- body (16px): 본문
- button (16px, 600): 버튼 텍스트

### Neumorphic 효과
- 볼록 (Raised): 버튼, 카드, 아이콘 - 두 방향 그림자
- 오목 (Inset): 입력 필드, 눌린 상태 - 반전 그림자
- 둥근 모서리 크게: 16px ~ 32px
- 배경색과 그림자 색상 조화 필수
```
