# GitHub 연결 가이드

---

## 1. Git 초기화 및 GitHub 연결

### 새 프로젝트 (로컬 → GitHub)
```bash
# 프로젝트 폴더에서
git init
git add .
git commit -m "Initial commit"

# GitHub에서 빈 저장소 생성 후
git remote add origin https://github.com/aifitdatalabs/wenowmeet.git
git branch -M main
git push -u origin main
```

### 기존 저장소 클론 (GitHub → 로컬)
```bash
git clone https://github.com/aifitdatalabs/wenowmeet.git
cd [repo-name]
```

---

## 2. .gitignore (React Native)

```gitignore
# Dependencies
node_modules/
.pnp/
.pnp.js

# Build
build/
dist/
*.hbc

# iOS
ios/Pods/
ios/build/
ios/*.xcworkspace
ios/*.xcuserdatad
*.ipa

# Android
android/build/
android/app/build/
android/.gradle/
*.apk
*.aab
*.keystore
!debug.keystore

# Env
.env
.env.*
!.env.example

# IDE
.idea/
.vscode/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Metro
.metro-health-check*

# Testing
coverage/

# Temporary
*.log
*.tmp
```

---

## 3. 브랜치 전략

### 기본 구조
```
main        ← 배포 가능한 안정 버전
  └── develop   ← 개발 통합 브랜치
        ├── feature/[기능명]   ← 기능 개발
        ├── fix/[버그명]       ← 버그 수정
        └── hotfix/[긴급수정]  ← 긴급 패치 (main에서 분기)
```

### 브랜치 네이밍
| 타입 | 형식 | 예시 |
|------|------|------|
| 기능 | `feature/[기능명]` | `feature/login` |
| 버그 | `fix/[버그명]` | `fix/auth-crash` |
| 긴급 | `hotfix/[설명]` | `hotfix/api-url` |
| 리팩토링 | `refactor/[대상]` | `refactor/navigation` |

### 브랜치 작업 흐름
```bash
# 1. develop에서 feature 브랜치 생성
git checkout develop
git pull origin develop
git checkout -b feature/login

# 2. 작업 후 커밋
git add .
git commit -m "feat: 로그인 화면 구현"

# 3. develop에 병합 (PR 권장)
git checkout develop
git merge feature/login
git push origin develop

# 4. feature 브랜치 삭제
git branch -d feature/login
```

---

## 4. 커밋 규칙 (Conventional Commits)

### 커밋 메시지 형식
```
<type>: <subject>

[body]

[footer]
```

### Type 종류
| Type | 설명 | 예시 |
|------|------|------|
| `feat` | 새 기능 | `feat: 로그인 화면 추가` |
| `fix` | 버그 수정 | `fix: 로그인 버튼 클릭 오류 수정` |
| `refactor` | 리팩토링 | `refactor: auth 로직 분리` |
| `style` | 코드 스타일 (포맷팅) | `style: prettier 적용` |
| `docs` | 문서 | `docs: README 업데이트` |
| `chore` | 빌드, 설정 | `chore: eslint 설정 추가` |
| `test` | 테스트 | `test: 로그인 테스트 추가` |

### 좋은 커밋 메시지 예시
```bash
# 기능 추가
git commit -m "feat: 소셜 로그인 (Google, Apple) 구현"

# 버그 수정
git commit -m "fix: 토큰 만료 시 자동 로그아웃 처리"

# 리팩토링
git commit -m "refactor: API 호출 로직 서비스로 분리"
```

---

## 5. 유용한 Git 명령어

### 상태 확인
```bash
git status              # 변경 파일 확인
git log --oneline -10   # 최근 커밋 10개
git branch -a           # 모든 브랜치 목록
```

### 변경 취소
```bash
git checkout -- [file]      # 파일 변경 취소
git reset HEAD [file]       # 스테이징 취소
git reset --soft HEAD~1     # 마지막 커밋 취소 (변경 유지)
git reset --hard HEAD~1     # 마지막 커밋 취소 (변경 삭제)
```

### 임시 저장
```bash
git stash                   # 변경사항 임시 저장
git stash pop               # 임시 저장 복원
git stash list              # 임시 저장 목록
```

### 원격 동기화
```bash
git fetch origin            # 원격 변경사항 가져오기
git pull origin [branch]    # 가져오기 + 병합
git push origin [branch]    # 푸시
```

---