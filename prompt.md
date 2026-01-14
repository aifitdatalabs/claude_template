# 프로젝트 프롬프트 전략

## 1. 프로젝트 템플릿 작성
1. `template.md` 복사
2. 프로젝트 정보 채우기 (이름, 설명, 기술 스택)

## 2. 스타일 선택
1. `guides/styles/library_[name].md`에서 라이브러리 선택 (NativeWind 추천)
2. `guides/styles/design_[name].md`에서 디자인 선택
3. 각 파일의 CLAUDE.md 섹션 복사하여 템플릿에 병합

## 3. 프로젝트 생성

### 프로젝트 생성 프롬프트
```md
/guides/1_projects/[프로젝트명.md] 파일 기준으로 초기 앱을 만들어줘.
[프로젝트명] 폴더 생성 후 프로젝트 셋업하고,
tab 네비게이션 구조와 기본 스타일만 구현해줘.
```

## 4. github 연결

### 1) 저장소 연결 프롬프트
```md
guides/2_github/[프로젝트명].md를 참고 해서 프로젝트에 Git 초기화하고 GitHub 저장소 연결해줘.
.gitignore도 React Native용으로 설정해줘.
```

### 2) 브랜치 생성 프롬프트
```md
guides/2_github/[프로젝트명].md를 참고 해서 develop 브랜치 만들고, feature/login 브랜치에서 작업 시작해줘.
```
```md
홈화면 부터 작업 할 건데, guides/2_github/[프로젝트명].md 파일 참고 해서, 적절한  브랜치 생성해서 작업 시작할 수 있도록 해줘.
```

## 5. 로그인

## 6. DB
