# 📮 팀 피드백 — GitHub MCP 인증 방식 문서 오류 (OAuth ❌ → PAT ✅)

> 작성일: 2026-06-01 · 대상: 공유되는 `resume-agents` 팀(README·CLAUDE.md·docs) 유지보수자
> 분류: 문서 정합성 버그 (실제 동작과 안내 불일치) · 우선순위: 높음(신규 팀원 온보딩 차단)

---

## 1. 한 줄 요약

문서 여러 곳이 GitHub MCP를 **"브라우저 OAuth 1회, PAT 불필요"** 라고 안내하지만,
실제로는 **OAuth가 불가능하고 fine-grained PAT를 환경변수로 주입**해야만 연결됩니다.
안내대로 따라 하면 `/mcp`에서 **`HTTP 400`** 으로 연결 실패합니다.

## 2. 증상 (재현)

```
/mcp
  ⎿  Failed to reconnect to github: HTTP 400 at https://api.githubcopilot.com/mcp/
```

## 3. 근본 원인

- `github` 서버는 원격 **HTTP MCP**이고, 인증 헤더가 `Authorization: Bearer ${GITHUB_PAT}` 입니다.
- 환경변수 `GITHUB_PAT`이 비어 있으면 `Authorization: Bearer ` (빈 토큰)로 전송 → 서버가 **400**으로 거부.
- 이 엔드포인트(`https://api.githubcopilot.com/mcp/`)는 **동적 클라이언트 등록(DCR) 미지원**이라
  Claude Code의 자동 브라우저 OAuth 흐름이 **작동하지 않습니다.** → OAuth 안내 자체가 틀림.

> 근거: `.mcp.json`의 `$comment`가 이미 PAT 방식임을 명시하고 있습니다(아래). 즉 **`.mcp.json`은 올바르고,
> README·CLAUDE.md·docs/GITHUB-MCP.md의 안내만 옛 정보로 남아 모순**된 상태입니다.

```json
// .mcp.json (올바름 — 수정 불필요)
"github": {
  "type": "http",
  "url": "https://api.githubcopilot.com/mcp/",
  "headers": { "Authorization": "Bearer ${GITHUB_PAT}" },
  "$comment": "... 이 엔드포인트는 동적 클라이언트 등록(DCR) 미지원 → 자동 OAuth 불가. 대신 fine-grained PAT를 ..."
}
```

## 4. 올바른 연결 절차 (정정안의 핵심)

1. GitHub에서 **fine-grained PAT** 발급 — 읽기 전용 권장: **Contents = Read-only**, **Metadata = Read-only**
   (이 팀은 경력 근거 '읽기'만 함). 발급: https://github.com/settings/personal-access-tokens/new
2. 환경변수 `GITHUB_PAT`에 토큰 등록 (Windows, User 스코프, 영구):
   ```powershell
   [Environment]::SetEnvironmentVariable("GITHUB_PAT", "github_pat_…", "User")
   ```
   - macOS/Linux: `~/.zshrc`(또는 `~/.bashrc`)에 `export GITHUB_PAT="github_pat_…"` 추가 후 새 셸.
3. **Claude Code와 터미널을 완전히 재시작** (환경변수는 프로세스 시작 시 1회만 읽힘).
4. `/mcp`에서 `github` → `connected` 확인. `/github-career <ID>`로 레포가 보이면 성공.

## 5. 수정해야 할 파일 / 위치 (공유 레포에 동일 적용)

| 파일 | 위치 | 현재(❌) | 정정(✅) |
|---|---|---|---|
| `README.md` | L20 | `docs/GITHUB-MCP.md` (브라우저 OAuth 1회, PAT 불필요) | `docs/GITHUB-MCP.md` (fine-grained PAT를 `GITHUB_PAT` 환경변수로 주입) |
| `README.md` | L88 | `… PAT 불필요, 브라우저 OAuth 1회.` | `… fine-grained PAT를 `GITHUB_PAT` 환경변수로 주입(읽기 전용 권장). OAuth 자동연결 미지원.` |
| `CLAUDE.md` | L127 (§9) | `…(OAuth, …). PAT 불필요, 브라우저 OAuth 1회.` | `…(HTTP, …). fine-grained PAT를 `GITHUB_PAT`로 주입. 이 엔드포인트는 자동 OAuth(DCR) 미지원.` |
| `docs/GITHUB-MCP.md` | 전체 | OAuth 중심 안내 | PAT 발급·환경변수 등록·재시작 중심으로 재작성(문제해결 표의 `OAuth 창 안 뜸` 행 → `400` 행으로 교체) |

> 참고: `.claude/agents/github-career-extractor.md`, `.claude/skills/github-career/SKILL.md`는
> "GitHub MCP(읽기 전용)"라고만 언급 → **수정 불필요**(인증 방식 단정 없음).
> `CLAUDE.md` L116(§8)의 OAuth 언급은 **Google Workspace** 대상이라 올바름 → 건드리지 말 것.

## 6. 보안 메모

- 토큰은 **레포에 절대 커밋 금지** — `${GITHUB_PAT}` 치환 + `.gitignore`로 차단.
- 채팅/로그에 토큰이 노출됐다면 즉시 **revoke 후 재발급**: https://github.com/settings/tokens?type=beta
- 권한은 최소(Contents·Metadata Read-only)로. 비공개 레포가 꼭 필요할 때만 범위 확대.

---

🤖 이 피드백을 공유 팀 레포에 반영하면, 같은 `HTTP 400`로 막히는 온보딩을 막을 수 있습니다.
