# Google Workspace(Docs · Drive) MCP 연결 가이드

이력서·포트폴리오 최종본을 Google Docs로 만들고 Drive에 저장하려면 **Google Workspace MCP
서버**를 연결합니다. 팀 공유 설정은 루트 `.mcp.json`에 이미 들어 있고, **각자 본인 계정으로
인증(OAuth)만 1회** 하면 됩니다.

> 사용하는 MCP 서버: [`google-workspace-mcp` (workspace-mcp)](https://github.com/taylorwilsdon/google_workspace_mcp)
> **이 팀은 `docs`·`drive` 도구만 활성화**합니다(Sheets/Slides/Calendar 미사용 — 권한 표면 최소화).

---

## A. 사전 설치
- `uv` 설치 (uvx 포함):
  - macOS/Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`
  - Windows(PowerShell): `irm https://astral.sh/uv/install.ps1 | iex`

## B. Google Cloud OAuth 클라이언트 만들기
1. https://console.cloud.google.com → 프로젝트 생성/선택
2. **API 및 서비스 → 라이브러리**에서 다음을 사용 설정:
   - Google Drive API
   - Google Docs API
   (이 팀은 Docs·Drive만 쓰므로 Sheets/Slides/Calendar API는 켤 필요 없습니다.)
3. **OAuth 동의 화면** 구성(내부 또는 외부, 테스트 사용자에 본인 이메일 추가)
4. **사용자 인증 정보 → OAuth 클라이언트 ID 만들기 → 데스크톱 앱**
5. 발급된 **client_secret JSON 파일을 다운로드**해 안전한 경로에 보관(예: `%USERPROFILE%\.google-mcp\client_secret.json`). **레포에 커밋 금지.**

## C. 환경변수 설정
`.mcp.json`이 읽는 변수: `GOOGLE_CLIENT_SECRET_PATH` · `USER_GOOGLE_EMAIL` · `WORKSPACE_MCP_CREDENTIALS_DIR`
(클라이언트 ID/시크릿을 따로 넣는 게 아니라, 다운로드한 **JSON 파일 경로**를 지정합니다.)

**Windows (PowerShell, 영구 설정):**
```powershell
setx GOOGLE_CLIENT_SECRET_PATH    "%USERPROFILE%\.google-mcp\client_secret.json"
setx USER_GOOGLE_EMAIL            "본인@gmail.com"
setx WORKSPACE_MCP_CREDENTIALS_DIR "%USERPROFILE%\.google-mcp"
```
(설정 후 터미널/Claude Code 재시작)

**macOS/Linux (`~/.zshrc` 또는 `~/.bashrc`):**
```bash
export GOOGLE_CLIENT_SECRET_PATH="$HOME/.google-mcp/client_secret.json"
export USER_GOOGLE_EMAIL="본인@gmail.com"
export WORKSPACE_MCP_CREDENTIALS_DIR="$HOME/.google-mcp"
```

## D. Claude Code에서 MCP 승인 & 인증
1. 이 폴더에서 `claude` 실행 → `.mcp.json`의 `google-workspace` 서버 사용을 **승인(Yes)**.
2. `/mcp`로 서버가 `connected` 인지 확인.
3. 첫 Drive 작업 시 브라우저로 **Google 로그인/동의** → 승인. 토큰이 `WORKSPACE_MCP_CREDENTIALS_DIR`에 저장돼 재인증 불필요.

## E. 연결 확인
```
/drive-sync 테스트 — 내 Drive 폴더 목록을 읽어줘
```
폴더/문서가 보이면 성공입니다.

---

## F. 문제해결
| 증상 | 원인 | 해결 |
|---|---|---|
| `/mcp`에 서버 안 보임 | 승인 안 함 / uvx 없음 | `claude` 재시작 후 승인, `uvx --version` 확인 |
| `connected`인데 도구 없음 | 권한 범위 부족 | Drive/Docs API 사용 설정, 동의 재진행 |
| 401/403 권한 오류 | 계정 불일치 | `USER_GOOGLE_EMAIL` 확인 |
| `access_denied` | 동의 화면 테스트 사용자 미등록 | OAuth 동의 화면에 본인 이메일 추가 |

## G. 폴백 (MCP 없이)
MCP를 못 쓰면 `docs-formatter`가 서식 적용된 마크다운을 산출합니다. 사용자가 복사해 Google Docs에 붙여넣으세요.

> ⚠️ 클라이언트 시크릿·토큰은 절대 레포에 커밋하지 마세요. `.gitignore`로 이미 차단돼 있습니다.
