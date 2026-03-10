---
tags:
  - work/dev
date: '2026-03-10'
---

# Daily Mail — TODO

## 즉시 해야 할 것 (내일)
- [x] Anthropic API 크레딧 충전 ($10)
- [x] CS Daily 실제 메일 발송 테스트 → 성공 (보낸편지함에서 확인)
- [x] 메일 템플릿 디자인 개선 (마크다운→HTML 변환 + CSS 리뉴얼)
- [ ] **받은편지함 수신 문제 해결** — 본인 Gmail→본인 Gmail 발송 시 보낸편지함에만 표시됨. `+` 별칭으로도 해결 안 됨. 별도 수신 메일 주소 사용 또는 다른 방법 조사 필요
- [ ] News Brief 실제 메일 발송 테스트 (`./gradlew bootRun --args="--mail.module=news-brief"`)

## 환경 세팅
- [x] CLAUDE_API_KEY 발급 및 .zshrc 등록 (personal-api workspace에서 재발급)
- [x] Gmail 앱 비밀번호 발급 및 .zshrc 등록
- [x] Anthropic 크레딧 충전 ($10)
- [ ] Google Calendar API 서비스 계정 세팅 (GOOGLE_CREDENTIALS)
  - Google Cloud Console → Calendar API 사용 설정
  - 서비스 계정 생성 → JSON 키 발급
  - Google Calendar에서 서비스 계정에 캘린더 공유
  - `base64 -i credentials.json | tr -d '\n'` → .zshrc에 등록

## 개발 TODO
- [x] 마크다운 → HTML 변환 (commonmark 라이브러리)
- [x] CS Daily 메일 템플릿 디자인 개선
- [ ] News Brief / Today Brief 메일 템플릿도 동일 수준으로 개선
- [ ] GitHub Actions workflow 실제 동작 검증
- [ ] GitHub Actions Secrets 등록 (CLAUDE_API_KEY, GMAIL_ADDRESS, GMAIL_APP_PASSWORD, MAIL_SENDER)
- [ ] GitHub Actions에서 history.json commit & push 동작 확인
- [ ] 프롬프트 튜닝 (CS Daily 응답 품질 확인 후)
- [ ] 뉴스 RSS 피드 URL 유효성 점검 (일부 피드 접속 불가 가능)

## 확장 후보 (나중에)
- [ ] 주간 회고 메일
- [ ] 채용 공고 모니터링
- [ ] GitHub 트렌딩 요약
- [ ] 웹 대시보드 (발송 이력, 설정 관리)

## 다음 프로젝트
- Google Spreadsheet 연동 가계부

## 참고 링크
- GitHub: https://github.com/hey00507/daily-mail
- Anthropic Console: https://console.anthropic.com/
- Gmail 앱 비밀번호: https://myaccount.google.com/apppasswords
- Google Cloud Console: https://console.cloud.google.com/
