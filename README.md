# Knowledge Base

AI한테 휘둘리지 말고, 직접 운전하자는 생각으로 만든 개인 지식 저장소.
Obsidian vault를 Claude MCP로 연결해서, 대화하면서 자동으로 정리되는 구조를 만들었다.

## 이게 뭔데

일하면서 생긴 회의록, 공부하면서 정리한 노트, 삽질 로그 같은 것들이 쌓이는 곳.
노트 앱에 묻혀서 다시 안 보게 되는 걸 방지하려고, 구조를 잡고 서로 연결해뒀다.

```
000.Home/       홈 — 전체 구조 진입점
010.Work/       실무 — 회의록, 설계서, DDL
020.Growth/     공부 — 기술 학습, 운동, 독서
030.Plans/      계획 — 일정, 약속
040.Logs/       로그 — 작업 기록, 회고
050.Etc/        기타 — 일기, 사색
```

## 어떻게 돌아가는지

```
Claude Code CLI → MCP → Obsidian Local REST API → Vault
                → MCP → Google Calendar
                → AppleScript → Apple Reminders / Calendar
```

- 대화하면서 작업하면 노트가 자동으로 적재됨
- 일정 얘기하면 캘린더/리마인더에 등록하고 vault에도 기록
- 그래프 뷰로 노트 간 연결 관계를 시각적으로 확인

## 지금까지 한 것들

**의성 마늘 데이터 포털** — 농업 데이터 개방 플랫폼 (Spring Boot, PostgreSQL)
- 데이터 표준사전 관리 시스템 (용어/단어/도메인/코드 CRUD + 엑셀 일괄처리)
- JWT 인증/인가 (Access/Refresh Token, BCrypt, Role 기반 접근제어)
- 데이터 품질관리 모듈

**AI 연동 자동화**
- Obsidian MCP — vault 읽기/쓰기/검색 연동
- Google Calendar MCP — OAuth 2.0 인증, 이벤트 CRUD
- Apple Reminders — AppleScript 기반 조회/생성
- Claude Code 커스텀 명령어 (`/plan`) — 일정 브리핑 자동화

## 기술

Java · Spring Boot · Spring Security · PostgreSQL · JWT
Obsidian · Claude MCP · Google Cloud OAuth 2.0 · AppleScript
