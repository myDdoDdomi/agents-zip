# GitHub MCP 연결 가이드 (경력 근거 읽기 전용)

`github-career-extractor`가 GitHub 레포·기여·스택을 **경력 근거**로 읽으려면 **GitHub 공식 원격
MCP 서버**를 연결합니다. 팀 공유 설정은 루트 `.mcp.json`에 들어 있고, 인증은 **fine-grained PAT
(개인 액세스 토큰)** 를 환경변수 `GITHUB_PAT`로 주입해서 합니다.

> 사용하는 서버: **GitHub 공식 원격 MCP** — `https://api.githubcopilot.com/mcp/` (HTTP 타입).
> 이 엔드포인트는 **동적 클라이언트 등록(DCR)을 지원하지 않아** Claude Code의 자동 브라우저 OAuth
> 흐름이 작동하지 않습니다. 그래서 PAT를 `Authorization: Bearer ${GITHUB_PAT}` 헤더로 직접 넣습니다.
> 이 팀은 레포·기여를 **읽기만** 합니다. 이슈/PR/머지 등 쓰기 작업은 하지 않습니다.

---

## A. fine-grained PAT 발급 (읽기 전용 권장)
1. 발급 페이지로 이동: <https://github.com/settings/personal-access-tokens/new>
2. **Repository access**: 경력 근거로 쓸 레포 범위를 고릅니다(공개만 다루면 Public 정도면 충분).
3. **Permissions(읽기 전용 권장)** — 이 팀은 경력을 '읽기'만 하므로:
   - **Contents** → **Read-only**
   - **Metadata** → **Read-only** (보통 자동 포함)
   - 쓰기 권한(Issues/PR/Contents=Write 등)은 **부여하지 마세요.**
4. 만료일을 설정하고 생성하면 `github_pat_...` 형식의 토큰이 한 번만 표시됩니다 — 안전한 곳에 복사.

> ⚠️ 토큰은 **절대 레포·`.mcp.json`에 직접 넣지 마세요.** 반드시 아래 환경변수(`GITHUB_PAT`)로만 주입합니다.

## B. 환경변수 `GITHUB_PAT` 등록
- **Windows (User 영구):** PowerShell에서
  ```powershell
  [Environment]::SetEnvironmentVariable("GITHUB_PAT", "github_pat_여기에토큰", "User")
  ```
- **macOS / Linux:** `~/.zshrc`(또는 `~/.bashrc`)에 추가 후 새 셸:
  ```bash
  export GITHUB_PAT="github_pat_여기에토큰"
  ```
  ```bash
  source ~/.zshrc   # 또는 source ~/.bashrc
  ```

## C. Claude Code · 터미널 완전 재시작
- 환경변수는 **프로세스 시작 시 1회만** 읽힙니다. 등록 후에는 **터미널과 Claude Code를 완전히
  종료했다가 다시 실행**해야 `${GITHUB_PAT}` 치환이 적용됩니다(기존 세션에서는 반영 안 됨).

## D. 승인 & 연결 확인
1. 이 폴더에서 `claude` 실행 → `.mcp.json`의 `github` 서버 사용을 **승인(Yes)**.
2. `/mcp`에서 `github` → **connected** 확인.
3. 다음으로 레포가 보이면 성공:
   ```
   /github-career  <본인 GitHub ID>
   ```
   레포 목록·언어·기여가 보이면 정상입니다.

---

## E. 문제해결
| 증상 | 원인 | 해결 |
|---|---|---|
| `/mcp`에 서버 안 보임 | 승인 안 함 | `claude` 재시작 후 서버 사용 승인 |
| `HTTP 400` (at `https://api.githubcopilot.com/mcp/`) | `GITHUB_PAT`가 비어 빈 Bearer 전송 / 자동 OAuth(DCR) 미지원 | PAT 발급 → `GITHUB_PAT` 등록 → **터미널·Claude Code 완전 재시작** |
| `401 / 403` | 토큰 무효·만료·권한 부족 | PAT 재발급(만료 확인), Contents/Metadata=Read-only 권한 확인 |
| 비공개 레포 안 보임 | PAT의 Repository access에 미포함 | 필요한 레포만 PAT 범위에 추가(읽기 전용) |
| `403/429` | 레이트리밋 | 잠시 후 재시도, 조회 범위 축소 |

## F. 폴백 (MCP 없이)
MCP를 못 쓰면, 사용자가 **레포 목록·README·기여 내용을 붙여넣으면** `github-career-extractor`가 그
입력을 근거로 경력 근거를 정리합니다(추측 없이 붙여넣은 사실만 사용).

---

## G. 거버넌스 (이 팀 1순위와 직결)
- **실제 레포·커밋·기여만 근거**로 씁니다. fork·튜토리얼·연습 레포를 "주도 개발"로 부풀리지 않습니다.
- 공동 레포는 **본인 기여 기준**으로 역할을 기술합니다. 불확실하면 `(확인 필요)`.
- 쓰기 작업(이슈/PR/머지)은 하지 않습니다 — 이 MCP는 근거 읽기 용도입니다(PAT도 읽기 전용 권장).

> ⚠️ 토큰(`GITHUB_PAT` 값)은 절대 레포에 커밋하지 마세요. 환경변수로만 주입하며, `.gitignore`로
> `.env`·`*token*` 류가 차단돼 있습니다.
