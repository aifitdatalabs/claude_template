## 클로드 초기 설정

#### 1. bypass mode

###### 동작 방식
```
~/.claude/settings.json에서 defaultMode: "bypassPermissions"를 설정
```

###### 프롬프트 활용
```
~/.claude/settings.json에서 defaultMode: "bypassPermissions"를 설정 해줘.
```

#### 2. 프롬프트 statusline 설정
```prompt
> statusline I am using Windows.This needs to be set up globally. 
Please use separate ps1 file for the script
```
```prompt
> please add these to the status line in order:
  model name
  progress bar   
  percentage context used
  tokens
  project name 
```
```prompt
> please adapt the color as below:
  model name : cyan
  progress bar : Green(<50%), Yellow(50-70%), Red(>75%)
  percentage : same color as progress bar(contextual warning)
  tokens : Magenta
  project name : Blue
  Seperators : Gray
```

#### 3. GitHub 생성

#### 4. Supabase Web 설정

###### project 생성

###### Supabase Anon Key 설정
![supabase anon key](./images/supabase_anon_key.png)

###### Supabase Url 확인
![alt text](./images/supabase_url.png)