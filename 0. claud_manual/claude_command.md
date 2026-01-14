## 권한 부여
```bash
echo "alias claude='claude --dangerously-skip-permissions'" >> ~/.bashrc
```

## claude 설치
#### update
```bash
npm update -g @anthropic-ai/claude-code
```

#### 버전 확인(현재 내 버전 2.1.6)
```bash
claude --version 
```

## 명령어
```python
/permissions
/config         # 설정등 확인
/ide
/status
/vim
/clear          # 대화 내용 초기화
/resume         # 세션 선택
/rewind         # undo
```


