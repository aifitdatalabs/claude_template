# docs/structure 템플릿

## 파일 구조
```
docs/structure/
├── app.md              # 앱 전체 구조
├── tabs/layout.md      # 탭 네비게이션
└── [feature]/layout.md # 기능별 레이아웃
```

---

## app.md 템플릿
```markdown
# 앱 전체 구조

## 네비게이션 구조
App
└── RootNavigator
    └── MainTabs
        ├── HomeTab
        └── ProfileTab

## 주요 화면
| 화면 | 설명 |
|------|------|
| HomeScreen | 메인 홈 |
```

---

## layout.md 템플릿
```markdown
# [기능명] 레이아웃

## 시각적 레이아웃
┌─────────────────────────┐
│        Header           │
├─────────────────────────┤
│        Content          │
├─────────────────────────┤
│        TabBar           │
└─────────────────────────┘

## 컴포넌트 트리
Screen
├── Header
├── Content
└── Footer

## 주요 컴포넌트
| 컴포넌트 | 위치 | 설명 |
|----------|------|------|

## 상태 관리
- store: [store명]
- 주요 상태: [상태 목록]

## 관련 파일
- 화면: src/screens/
- 컴포넌트: src/features/[feature]/components/
```
