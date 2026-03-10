---
tags:
  - growth/ai
date: '2026-03-10'
related: null
---

# Apple Reminders & Google Calendar MCP 연동 가이드

## 개요
Claude Code CLI에서 MCP를 통해 Apple Reminders와 Google Calendar에 직접 접근하여 일정/리마인더를 생성, 조회, 수정, 삭제할 수 있도록 구성한다.

---

## 1. Apple Reminders MCP

### 패키지 정보
| 항목 | 값 |
|------|-----|
| 패키지명 | `mcp-server-apple-reminders` |
| GitHub | [FradSer/mcp-server-apple-reminders](https://github.com/FradSer/mcp-server-apple-reminders) |
| 기반 기술 | macOS EventKit (네이티브 API) |
| 요구사항 | macOS, Node.js 18+, Xcode Command Line Tools |

### 설치 명령
```bash
claude mcp add -s user apple-reminders -- npx -y mcp-server-apple-reminders
```

### 동작 방식
```
[Claude Code CLI]
    ↓ MCP Protocol (stdio)
[mcp-server-apple-reminders]
    ↓ EventKit (Swift 바인딩)
[Apple Reminders 앱]
    ↓ iCloud 동기화
[iPhone/iPad/Mac Reminders]
```

- macOS의 EventKit 프레임워크를 사용하여 네이티브로 Reminders에 접근
- AppleScript 기반이 아니라 성능이 좋고 안정적
- 최초 실행 시 macOS에서 **Reminders 접근 권한** 요청 팝업이 뜸 → 반드시 허용

### 주요 기능
- 리마인더 목록(List) 조회
- 리마인더 생성 (제목, 날짜, 우선순위)
- 리마인더 완료/미완료 토글
- 리마인더 삭제

### 참고: mcp-server-apple-events
- 같은 저자가 만든 확장 버전으로, Reminders + Calendar 모두 지원
- 패키지명: `mcp-server-apple-events`
- 추후 이걸로 전환하면 Apple Calendar도 함께 관리 가능

---

## 2. Google Calendar MCP

### 패키지 정보
| 항목 | 값 |
|------|-----|
| 패키지명 | `@cocal/google-calendar-mcp` |
| GitHub | [nspady/google-calendar-mcp](https://github.com/nspady/google-calendar-mcp) |
| 버전 | 2.6.1 |
| GitHub Stars | 17,000+ |
| 라이선스 | MIT |

### 설치 명령
```bash
claude mcp add -s user google-calendar \
  -e GOOGLE_OAUTH_CREDENTIALS="$HOME/.config/gcp-oauth.keys.json" \
  -- npx -y @cocal/google-calendar-mcp
```

### 동작 방식
```
[Claude Code CLI]
    ↓ MCP Protocol (stdio)
[@cocal/google-calendar-mcp]
    ↓ Google Calendar API (OAuth 2.0)
[Google Calendar]
    ↓ 구글 계정 동기화
[모든 기기의 Google Calendar]
```

### Google Cloud OAuth 설정 (필수)

> **이 단계를 완료해야 Google Calendar MCP가 동작합니다.**

#### Step 1: Google Cloud Console에서 프로젝트 생성
1. [Google Cloud Console](https://console.cloud.google.com/) 접속
2. 새 프로젝트 생성 (예: `claude-calendar`)

#### Step 2: Calendar API 활성화
1. **API 및 서비스** → **라이브러리** 이동
2. "Google Calendar API" 검색 → **사용** 클릭

#### Step 3: OAuth 동의 화면 설정
1. **API 및 서비스** → **OAuth 동의 화면**
2. 사용자 유형: **외부** 선택
3. 앱 이름, 이메일 등 기본 정보 입력
4. 범위(Scopes) 추가: `https://www.googleapis.com/auth/calendar`
5. 테스트 사용자에 본인 구글 이메일 추가

#### Step 4: OAuth 클라이언트 ID 생성
1. **API 및 서비스** → **사용자 인증 정보**
2. **사용자 인증 정보 만들기** → **OAuth 클라이언트 ID**
3. 애플리케이션 유형: **데스크톱 앱**
4. JSON 다운로드 → `~/.config/gcp-oauth.keys.json`에 저장

```bash
# 다운로드한 파일을 지정 위치로 이동
mv ~/Downloads/client_secret_*.json ~/.config/gcp-oauth.keys.json
```

#### Step 5: 최초 인증 실행
```bash
GOOGLE_OAUTH_CREDENTIALS="$HOME/.config/gcp-oauth.keys.json" \
  npx @cocal/google-calendar-mcp auth
```
- 브라우저가 열리면서 Google 로그인 → Calendar 접근 권한 승인
- 토큰이 자동 저장되어 이후에는 재인증 불필요

### 주요 기능
- 다중 계정/캘린더 지원
- 이벤트 CRUD (생성, 조회, 수정, 삭제)
- 반복 일정 지원
- Free/Busy 조회
- 교차 계정 충돌 감지
- 자연어 날짜/시간 이해 ("내일 오후 3시")
- 스마트 스케줄링

---

## 3. 현재 설정 상태

### 등록된 MCP 서버
| 서버 | 상태 | 비고 |
|------|------|------|
| `obsidian` | 활성 | Vault 읽기/쓰기 |
| `apple-reminders` | 활성 | 최초 실행 시 권한 허용 필요 |
| `google-calendar` | 활성 | OAuth 인증 완료 (프로젝트: claude-calendar-489801) |

### 설정 파일 위치
- MCP 서버 목록: `~/.claude.json`
- Google OAuth 키: `~/.config/gcp-oauth.keys.json` (생성 필요)
- 권한 설정: `~/.claude/settings.local.json`

---

## 4. 활용 시나리오

### Obsidian + Reminders + Calendar 연계 플로우
```
사용자: "내일 오후 2시에 의성 마늘 프로젝트 회의 잡아줘"
    ↓
Claude:
  1. Google Calendar에 이벤트 생성 (2시, 의성 마늘 회의)
  2. Apple Reminders에 리마인더 생성 (30분 전 알림)
  3. Obsidian 03.Plans/에 일정 노트 생성
  4. 04.Logs/에 대화 로그 기록
```

### 일정 조회
```
사용자: "이번 주 일정 정리해줘"
    ↓
Claude:
  1. Google Calendar에서 이번 주 이벤트 조회
  2. Apple Reminders에서 미완료 항목 조회
  3. Obsidian 03.Plans/에 주간 요약 노트 생성
```

---

## 5. 대안 패키지 비교

### Apple Reminders
| 패키지 | 특징 |
|--------|------|
| `mcp-server-apple-reminders` (FradSer) | npx 간편 설치, EventKit 네이티브 |
| `apple-reminders-mcp-server` (mggrim) | 18개 도구, 자연어 날짜, 고급 검색 |
| `apple-reminders-mcp` (shadowfax92) | 경량, AppleScript 기반 |

### Google Calendar
| 패키지 | 특징 |
|--------|------|
| `@cocal/google-calendar-mcp` (nspady) | 17K+ stars, 가장 활발한 커뮤니티 |
| `@takumi0706/google-calendar-mcp` | 소규모, Claude Desktop 중심 |
| `google_workspace_mcp` | Gmail, Drive, Docs 등 통합 |

---

*관련 노트: [[2026-03-10 Obsidian MCP 구성 가이드]] · [[Claude MCP]]*


---
## Related
- [[Claude MCP]]
- [[2026-03-10 Obsidian MCP 구성 가이드]]
