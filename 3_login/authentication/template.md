[Google]
  1. https://console.cloud.google.com 접속
  
  2. 좌측 상단 프로젝트 클릭 후 > 신규 프로젝트 생성

  3. API 및 서비스 클릭

  4. OAuth2.0 클라이언트 생성
  
  client id :
  client secret : 

[Kakao]
  1. 앱 > 앱 설정 > 앱 > 제품 링크 관리 : 웹도메인 등록
  2. 앱 > 앱 설정 > 앱 > 플랫폼 키 : rest api, client secret 키 획득
  3. 앱 > 제품 설정 > 카카오 로그인 > 일반 : 사용설정 ON
  4. 앱 > 제품 설정 > 카카오 로그인 > 동의항목 : 동의 항목 지정(email 수집도 체크 해야함)

  rest api : 
  client secret : 

[Supabase]
  1. 신규 프로젝트 생성 필요
  2. Authentification > Sign In / Providers > Kakao, Google 체크 하고, 정보 입력
  3. Authentification > URL Configuration redirect URLs 입력
  4. user_profiles table 생성
     ```sql
      -- 기존 테이블 초기화 (필요시)
      DROP SCHEMA public CASCADE;
      CREATE SCHEMA public;
      GRANT ALL ON SCHEMA public TO postgres;
      GRANT ALL ON SCHEMA public TO public;

      -- user_profiles 테이블 생성
      create table public.user_profiles (
        id uuid default gen_random_uuid() primary key,
        user_id uuid references auth.users(id) on delete cascade unique not null,
        email text,
        nickname text,
        avatar_url text,
        created_at timestamp with time zone default now() not null,
        updated_at timestamp with time zone default now() not null
      );

      -- RLS 정책
      alter table public.user_profiles enable row level security;

      create policy "Users can view own profile" on public.user_profiles
        for select using (auth.uid() = user_id);

      create policy "Users can update own profile" on public.user_profiles
        for update using (auth.uid() = user_id);

      create policy "Users can insert own profile" on public.user_profiles
        for insert with check (auth.uid() = user_id);

      -- 회원가입 시 자동 프로필 생성 트리거
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