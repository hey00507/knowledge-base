---
tags:
  - growth/ai
date: '2026-03-10'
related: null
---

# Obsidian MCP 구성 가이드

## 개요
Claude Code(CLI)에서 **MCP(Model Context Protocol)** 를 통해 Obsidian vault에 직접 접근하여 노트를 읽고, 쓰고, 검색할 수 있다. Obsidian 앱이 열려 있는 상태에서 로컬 REST API를 통해 통신한다.

## 현재 로컬 구성

### 환경 정보
| 항목 | 값 |
|------|-----|
| OS | macOS (Darwin 25.3.0) |
| Claude 클라이언트 | Claude Code CLI (claude-opus-4-6) |
| MCP 서버 | obsidian (Obsidian Local REST API 기반) |
| Vault 위치 | Obsidian 앱에서 열린 vault (로컬) |

### 설정 파일 위치
- **Claude Code 설정**: `~/.claude/settings.local.json`
  - 여기에 MCP 도구별 자동 허용(permissions.allow) 설정이 포함됨
  - 예: `mcp__obsidian__get_vault_stats`, `mcp__obsidian__list_directory`
- **Claude Desktop 설정**: `~/Library/Application Support/Claude/claude_desktop_config.json`
  - 현재 `mcpServers: {}` — Desktop에서는 별도 MCP 서버 미설정
  - Claude Code CLI 쪽에서 Obsidian MCP가 연결되어 있음

### 동작 방식 (아키텍처)
```
[Claude Code CLI] 
    ↓ MCP Protocol (JSON-RPC over stdio/SSE)
[Obsidian MCP Server] 
    ↓ HTTP REST API (localhost)
[Obsidian 앱 - Local REST API 플러그인]
    ↓ 직접 파일 접근
[Vault 파일시스템]
```

1. **Obsidian 앱**에서 `Local REST API` 커뮤니티 플러그인을 설치하고 활성화
2. 플러그인이 로컬에서 HTTP 서버를 실행 (기본 포트: 27124)
3. **Obsidian MCP Server**가 이 REST API를 감싸서 MCP 프로토콜로 변환
4. **Claude Code**가 MCP 프로토콜을 통해 도구(tool)로 호출

### 핵심 포인트
- Obsidian 앱이 **반드시 실행 중**이어야 함 (REST API 서버가 앱 내에서 동작)
- vault를 여러 개 사용하는 경우, **현재 열려있는 vault**에만 접근 가능
- 인증은 API Key 기반 (Local REST API 플러그인 설정에서 확인)

## 사용 가능한 MCP 도구 목록

### 읽기 (Read)
| 도구 | 설명 |
|------|------|
| `read_note` | 특정 경로의 노트 읽기 (frontmatter + content) |
| `read_multiple_notes` | 여러 노트를 한 번에 읽기 |
| `search_notes` | 노트 내용/제목 검색 |
| `get_notes_info` | 노트 메타 정보 조회 |
| `get_vault_stats` | vault 전체 통계 (노트 수, 폴더 수, 크기, 최근 수정) |
| `list_directory` | 디렉터리 내 파일/폴더 목록 |
| `get_frontmatter` | 노트의 YAML frontmatter 조회 |

### 쓰기 (Write)
| 도구 | 설명 |
|------|------|
| `write_note` | 노트 생성/덮어쓰기/추가(append)/앞에추가(prepend) |
| `patch_note` | 노트 내 특정 문자열 교체 (부분 수정) |
| `update_frontmatter` | frontmatter만 수정 |

### 관리 (Manage)
| 도구 | 설명 |
|------|------|
| `move_note` | 노트 이동 |
| `move_file` | 파일 이동 |
| `delete_note` | 노트 삭제 |
| `manage_tags` | 태그 추가/제거/이름변경 |

## 활용 예시

### 일일 로그 자동 기록
Claude와의 대화에서 유의미한 내용을 `030.Logs/yyyy-mm-dd claude.md`에 자동 기록
- 작업 요약, 문제 해결, 학습 내용, 회의 정리 등

### 노트 검색 및 참조
대화 중 vault 내 기존 노트를 검색하여 맥락 파악 후 답변에 활용

### 프로젝트 문서화
코드 작업 결과를 Study 폴더에 정리하여 지식 축적


---
## Related
- [[Claude MCP]]
- [[2026-03-10 Calendar MCP 연동 가이드]]
