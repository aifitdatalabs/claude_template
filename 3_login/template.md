# 로그인/인증 가이드 (Supabase)

---

## 1. Supabase 설정

### 설치
```bash
npm install @supabase/supabase-js @react-native-async-storage/async-storage
```

### 환경변수 (.env)
```env
SUPABASE_URL=https://[project-id].supabase.co
SUPABASE_ANON_KEY=[anon-key]
```

### src/services/supabase.ts
```typescript
import AsyncStorage from '@react-native-async-storage/async-storage';
import { createClient } from '@supabase/supabase-js';

// 타입 re-export (다른 파일에서 @supabase/supabase-js 직접 import 방지)
export type { Session, User } from '@supabase/supabase-js';

const SUPABASE_URL = process.env.SUPABASE_URL || '';
const SUPABASE_ANON_KEY = process.env.SUPABASE_ANON_KEY || '';

export const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY, {
  auth: {
    storage: AsyncStorage,
    autoRefreshToken: true,
    persistSession: true,
    detectSessionInUrl: false,
  },
});
```

---

## 2. 폴더 구조

> `guides/1_projects/template.md` 구조 준수

```
src/
├── services/
│   └── supabase.ts          # Supabase 클라이언트
├── hooks/
│   └── useAuth.ts           # 인증 훅
├── types/
│   └── auth.ts              # 인증 타입 정의
├── features/
│   └── auth/
│       ├── index.ts         # export 모음
│       └── AuthContainer.tsx    # 인증 플로우 컨테이너
├── screens/
│   └── auth/
│       ├── WelcomeScreen.tsx    # 초기 화면
│       ├── LoginScreen.tsx      # 로그인 화면
│       └── SignupScreen.tsx     # 회원가입 화면
└── components/
    └── auth/
        └── authStyles.ts        # 인증 화면 공통 스타일
```

| 폴더 | 역할 |
|------|------|
| `services/` | Supabase 클라이언트 (외부 서비스 연동) |
| `hooks/` | useAuth 훅 (인증 상태 관리) |
| `types/` | 공유 타입 정의 |
| `features/auth/` | 인증 도메인 로직 + 컨테이너 |
| `screens/auth/` | 네비게이션 연결 화면 |
| `components/auth/` | 인증 관련 공통 UI/스타일 |

---

## 3. 타입 정의

### src/types/auth.ts
```typescript
import { Session, User } from '@/services/supabase';

export type AuthScreen = 'welcome' | 'login' | 'signup';
export type ConnectionStatus = 'checking' | 'connected' | 'error';
export type SocialProvider = 'google' | 'apple' | 'github';

export interface UserProfile {
  user_id: string;
  email?: string;
  nickname?: string;
  avatar_url?: string;
  created_at?: string;
  updated_at?: string;
  // 프로젝트별 추가 필드
}

export interface AuthContextType {
  // 상태
  user: User | null;
  session: Session | null;
  userProfile: UserProfile | null;
  isLoading: boolean;
  isAuthenticated: boolean;
  connectionStatus: ConnectionStatus;
  error: string | null;

  // 액션
  signIn: (email: string, password: string) => Promise<void>;
  signUp: (email: string, password: string) => Promise<{ needsEmailConfirmation: boolean }>;
  signOut: () => Promise<void>;
  signInWithProvider: (provider: SocialProvider) => Promise<void>;
  updateProfile: (data: Partial<UserProfile>) => Promise<void>;
  refreshProfile: () => Promise<void>;
  clearError: () => void;
}
```

---

## 4. 인증 훅

### src/hooks/useAuth.ts
```typescript
import { useState, useEffect, useCallback } from 'react';
import { supabase, Session, User } from '@/services/supabase';
import { UserProfile, ConnectionStatus, SocialProvider, AuthContextType } from '@/types/auth';

const REDIRECT_URL = 'https://[your-redirect-url]';

export const useAuth = (): AuthContextType => {
  const [user, setUser] = useState<User | null>(null);
  const [session, setSession] = useState<Session | null>(null);
  const [userProfile, setUserProfile] = useState<UserProfile | null>(null);
  const [isLoading, setIsLoading] = useState(true);
  const [connectionStatus, setConnectionStatus] = useState<ConnectionStatus>('checking');
  const [error, setError] = useState<string | null>(null);

  // 프로필 조회
  const fetchUserProfile = useCallback(async (userId: string): Promise<UserProfile | null> => {
    try {
      const { data, error: profileError } = await supabase
        .from('user_profiles')
        .select('*')
        .eq('user_id', userId)
        .single();

      if (profileError) return null;
      return data;
    } catch (err) {
      console.error('프로필 조회 에러:', err);
      return null;
    }
  }, []);

  // 초기화
  useEffect(() => {
    const initializeAuth = async () => {
      try {
        const { data: { session: currentSession } } = await supabase.auth.getSession();

        setSession(currentSession);
        setUser(currentSession?.user ?? null);

        if (currentSession?.user?.id) {
          const { error: connError } = await supabase
            .from('user_profiles')
            .select('count')
            .limit(1);

          setConnectionStatus(connError ? 'error' : 'connected');

          const profile = await fetchUserProfile(currentSession.user.id);
          setUserProfile(profile);
        }
      } catch (err) {
        console.error('초기화 에러:', err);
      } finally {
        setIsLoading(false);
      }
    };

    initializeAuth();

    const { data: { subscription } } = supabase.auth.onAuthStateChange(
      async (event, newSession) => {
        setSession(newSession);
        setUser(newSession?.user ?? null);

        if (event === 'SIGNED_IN' && newSession?.user?.id) {
          const profile = await fetchUserProfile(newSession.user.id);
          setUserProfile(profile);
          setConnectionStatus('connected');
          setError(null);
        } else if (event === 'SIGNED_OUT') {
          setUserProfile(null);
          setConnectionStatus('checking');
        }
      }
    );

    return () => subscription.unsubscribe();
  }, [fetchUserProfile]);

  // 이메일/비밀번호 로그인
  const signIn = useCallback(async (email: string, password: string) => {
    setIsLoading(true);
    setError(null);

    try {
      const { error: signInError } = await supabase.auth.signInWithPassword({
        email,
        password,
      });
      if (signInError) throw signInError;
    } catch (err: any) {
      setError(err.message || '로그인 실패');
      throw err;
    } finally {
      setIsLoading(false);
    }
  }, []);

  // 회원가입
  const signUp = useCallback(async (email: string, password: string) => {
    setIsLoading(true);
    setError(null);

    try {
      const { data, error: signUpError } = await supabase.auth.signUp({
        email,
        password,
        options: {
          emailRedirectTo: REDIRECT_URL,
        },
      });
      if (signUpError) throw signUpError;

      const needsEmailConfirmation = !!data.user && !data.session;
      return { needsEmailConfirmation };
    } catch (err: any) {
      setError(err.message || '회원가입 실패');
      throw err;
    } finally {
      setIsLoading(false);
    }
  }, []);

  // 로그아웃
  const signOut = useCallback(async () => {
    setError(null);
    try {
      const { error: signOutError } = await supabase.auth.signOut();
      if (signOutError) throw signOutError;

      setUser(null);
      setSession(null);
      setUserProfile(null);
    } catch (err: any) {
      setError(err.message || '로그아웃 실패');
      throw err;
    }
  }, []);

  // 소셜 로그인
  const signInWithProvider = useCallback(async (provider: SocialProvider) => {
    setError(null);
    try {
      const { error: oauthError } = await supabase.auth.signInWithOAuth({
        provider,
        options: {
          redirectTo: REDIRECT_URL,
        },
      });
      if (oauthError) throw oauthError;
    } catch (err: any) {
      setError(err.message || '소셜 로그인 실패');
      throw err;
    }
  }, []);

  // 프로필 업데이트
  const updateProfile = useCallback(async (data: Partial<UserProfile>) => {
    if (!user?.id) throw new Error('로그인이 필요합니다');
    setError(null);

    try {
      const { error: updateError } = await supabase
        .from('user_profiles')
        .update(data)
        .eq('user_id', user.id);
      if (updateError) throw updateError;

      const profile = await fetchUserProfile(user.id);
      setUserProfile(profile);
    } catch (err: any) {
      setError(err.message || '프로필 업데이트 실패');
      throw err;
    }
  }, [user?.id, fetchUserProfile]);

  // 프로필 새로고침
  const refreshProfile = useCallback(async () => {
    if (user?.id) {
      const profile = await fetchUserProfile(user.id);
      setUserProfile(profile);
    }
  }, [user?.id, fetchUserProfile]);

  // 에러 초기화
  const clearError = useCallback(() => setError(null), []);

  return {
    user,
    session,
    userProfile,
    isLoading,
    isAuthenticated: !!session,
    connectionStatus,
    error,
    signIn,
    signUp,
    signOut,
    signInWithProvider,
    updateProfile,
    refreshProfile,
    clearError,
  };
};
```

---

## 5. 인증 컨테이너

### src/features/auth/AuthContainer.tsx
```typescript
import React, { useState } from 'react';
import { View } from 'react-native';
import { AuthScreen } from '@/types/auth';
import { useAuth } from '@/hooks/useAuth';
import { WelcomeScreen } from '@/screens/auth/WelcomeScreen';
import { LoginScreen } from '@/screens/auth/LoginScreen';
import { SignupScreen } from '@/screens/auth/SignupScreen';

export const AuthContainer: React.FC = () => {
  const [screen, setScreen] = useState<AuthScreen>('welcome');
  const auth = useAuth();

  const renderScreen = () => {
    switch (screen) {
      case 'login':
        return (
          <LoginScreen
            onBack={() => setScreen('welcome')}
            onSignIn={auth.signIn}
            onSocialLogin={auth.signInWithProvider}
            isLoading={auth.isLoading}
            error={auth.error}
          />
        );
      case 'signup':
        return (
          <SignupScreen
            onBack={() => setScreen('welcome')}
            onSignUp={auth.signUp}
            isLoading={auth.isLoading}
            error={auth.error}
          />
        );
      default:
        return (
          <WelcomeScreen
            onLogin={() => setScreen('login')}
            onSignup={() => setScreen('signup')}
          />
        );
    }
  };

  return <View style={{ flex: 1 }}>{renderScreen()}</View>;
};
```

---

## 6. export 모음

### src/features/auth/index.ts
```typescript
// 컨테이너
export { AuthContainer } from './AuthContainer';
```

### src/hooks/index.ts (추가)
```typescript
export { useAuth } from './useAuth';
```

### src/types/index.ts (추가)
```typescript
export * from './auth';
```

---

## 7. App.tsx 연동

```typescript
import React from 'react';
import { View, ActivityIndicator } from 'react-native';
import { AuthContainer } from '@/features/auth';
import { useAuth } from '@/hooks/useAuth';
import { MainApp } from '@/MainApp';

const App: React.FC = () => {
  const { isAuthenticated, isLoading } = useAuth();

  if (isLoading) {
    return (
      <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
        <ActivityIndicator size="large" />
      </View>
    );
  }

  return isAuthenticated ? <MainApp /> : <AuthContainer />;
};

export { App };
```

---

## 8. Supabase 테이블 (user_profiles)

### SQL
```sql
create table public.user_profiles (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users(id) on delete cascade unique not null,
  email text,
  nickname text,
  avatar_url text,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null,
  updated_at timestamp with time zone default timezone('utc'::text, now()) not null
);

-- RLS 정책
alter table public.user_profiles enable row level security;

create policy "Users can view own profile"
  on public.user_profiles for select
  using (auth.uid() = user_id);

create policy "Users can update own profile"
  on public.user_profiles for update
  using (auth.uid() = user_id);

create policy "Users can insert own profile"
  on public.user_profiles for insert
  with check (auth.uid() = user_id);

-- 자동 프로필 생성 트리거
create or replace function public.handle_new_user()
returns trigger as $$
begin
  insert into public.user_profiles (user_id, email)
  values (new.id, new.email);
  return new;
end;
$$ language plpgsql security definer;

create trigger on_auth_user_created
  after insert on auth.users
  for each row execute procedure public.handle_new_user();
```

---

## 9. CLAUDE.md 인증 섹션

프로젝트 CLAUDE.md에 추가:

```markdown
## 인증 규칙

### Supabase 타입 import
- 항상 `@/services/supabase`에서 import (polyfill 순서 보장)
- `@supabase/supabase-js` 직접 import 금지

### 인증 상태 접근
- `useAuth()` 훅 사용 (`@/hooks/useAuth`)
- `isAuthenticated`로 로그인 여부 확인
- `user?.id`로 현재 사용자 ID 접근

### 폴더 규칙
- 인증 훅: `src/hooks/useAuth.ts`
- 인증 타입: `src/types/auth.ts`
- 인증 컨테이너: `src/features/auth/`
- 인증 화면: `src/screens/auth/`

### 세션 관리
- AsyncStorage에 자동 저장
- 앱 재시작 시 자동 복원
- 토큰 자동 갱신 (autoRefreshToken: true)

### 프로필 연동
- user_profiles 테이블 사용
- 회원가입 시 트리거로 자동 생성
- `updateProfile()`로 업데이트
```

---

## 10. 프롬프트 예시

### 인증 설정
```text
Supabase 인증을 설정해줘.
- src/services/supabase.ts 생성
- src/hooks/useAuth.ts 훅 생성
- src/types/auth.ts 타입 정의
- src/features/auth/ 컨테이너 생성
- src/screens/auth/ 로그인/회원가입 화면 생성
```

### 소셜 로그인 추가
```text
Google, Apple 소셜 로그인 추가해줘.
signInWithProvider 함수 사용.
```
