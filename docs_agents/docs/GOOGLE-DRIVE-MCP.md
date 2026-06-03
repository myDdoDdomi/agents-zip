# Google Workspace(Drive · Docs · Sheets · Slides · Calendar) MCP 연결 가이드

에이전트가 Google Drive 문서·스프레드시트·일정을 직접 읽고 쓰려면 **Google Workspace MCP
서버**를 연결해야 합니다. 팀 공유 설정은 레포 루트 `.mcp.json`에 이미 들어 있고,
**각 팀원은 본인 계정으로 인증(OAuth)만 1회** 하면 됩니다.

> 사용하는 MCP 서버: [`google-workspace-mcp` (workspace-mcp)](https://github.com/taylorwilsdon/google_workspace_mcp)
> — Docs·Sheets·Slides·Drive 읽기·쓰기와 Calendar(일정 조회·생성·수정)를 지원합니다.
> (다른 Drive MCP를 쓰고 싶으면 §대안 참고)

---

## A. 팀 공유 Drive 준비 (팀 리드가 1회)

"하나의 계정 문서를 연결"하는 가장 안전한 방법:

1. 팀 공유용 Google 계정(또는 공유 드라이브)에 **루트 폴더**를 만든다. (예: `우리팀-문서`)
2. 그 폴더를 팀원들에게 **편집자(Editor) 권한**으로 공유한다.
3. 폴더 URL의 ID를 `config/drive-config.md`에 채워 커밋한다.
   `https://drive.google.com/drive/folders/`**`이부분`**
4. 문서 종류별 하위 폴더(`01_기획`, `02_요구사항` …)를 만들고 ID를 표에 채운다.

이렇게 하면 누가 작업하든 **같은 폴더/문서**를 읽고 수정합니다.

---

## B. 개인 인증 설정 (각 팀원 1회)

### 1) 사전 설치
- `uv` 설치 (uvx 포함):
  - macOS/Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`
  - Windows(PowerShell): `irm https://astral.sh/uv/install.ps1 | iex`

### 2) Google Cloud OAuth 클라이언트 만들기
1. https://console.cloud.google.com → 프로젝트 생성/선택
2. **API 및 서비스 → 라이브러리**에서 다음을 모두 사용 설정:
   - Google Drive API
   - Google Docs API
   - Google Sheets API
   - Google Slides API
   - Google Calendar API
3. **OAuth 동의 화면** 구성 (내부 또는 외부, 테스트 사용자에 본인 이메일 추가)
   - 동의(스코프) 단계에서 위 API들(특히 **Calendar 권한**)이 포함되도록 진행합니다.
     첫 인증 시 브라우저 동의 창에 캘린더 접근 항목이 보이면 함께 허용하세요.
4. **사용자 인증 정보 → OAuth 클라이언트 ID 만들기 → 데스크톱 앱**
5. 발급된 **client_secret JSON 파일을 다운로드**해 안전한 경로에 보관합니다
   (예: `%USERPROFILE%\.google-mcp\client_secret.json`). **레포에 커밋 금지.**

### 3) 환경변수 설정
`.mcp.json`은 `${VAR}` 형태로 환경변수를 읽습니다. **이 서버(workspace-mcp)는
client_secret.json "파일 경로" 방식**을 쓰므로, 아래 변수명을 정확히 사용하세요.
(클라이언트 ID/시크릿을 따로 넣는 게 아니라, 다운로드한 JSON 파일 경로를 지정합니다.)

`.mcp.json`이 실제로 읽는 변수: `GOOGLE_CLIENT_SECRET_PATH` ·
`USER_GOOGLE_EMAIL` · `WORKSPACE_MCP_CREDENTIALS_DIR`

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

### 4) Claude Code에서 MCP 승인 & 인증
1. 레포 폴더에서 `claude` 실행 → `.mcp.json`의 `google-workspace` 서버 사용을 **승인(Yes)**.
2. `/mcp` 명령으로 서버가 `connected` 인지 확인.
3. 첫 Drive 작업 시 브라우저로 **Google 로그인/동의** 창이 뜹니다 → 승인.
   이후 토큰이 `WORKSPACE_MCP_CREDENTIALS_DIR`에 저장돼 재인증이 필요 없습니다.

### 5) 연결 확인
프롬프트에 입력:
```
/drive-sync 테스트 — 공유 폴더 목록을 읽어줘
```
폴더/문서가 보이면 연결 성공입니다.

---

## C. 문제해결
| 증상 | 원인 | 해결 |
|---|---|---|
| `/mcp`에 서버 안 보임 | 승인 안 함 / uvx 없음 | `claude` 재시작 후 승인, `uvx --version` 확인 |
| `connected`인데 도구 없음 | 권한 범위 부족 | Drive/Docs/Sheets/Slides/Calendar API 사용 설정, 동의 재진행 |
| 401/403 권한 오류 | 폴더 미공유 / 계정 불일치 | 공유 폴더 Editor 권한 확인, `USER_GOOGLE_EMAIL` 확인 |
| `access_denied` | 동의 화면 테스트 사용자 미등록 | OAuth 동의 화면에 본인 이메일 추가 |
| 토큰 만료 반복 | 자격 디렉터리 권한 | `WORKSPACE_MCP_CREDENTIALS_DIR` 경로/권한 확인 |
| Sheets 셀에 색·틀고정·드롭다운이 안 들어감 | **직접 서식 API 없음** | XLSX 빌드 후 import (§C-1·`DOC-FORMATTING.md §3-A`) |
| `import_to_google_sheets`가 파일을 못 읽음 | 경로가 attachments 밖 / `file://` 누락 | attachments 디렉터리 + `file://` URL 사용 (§C-1) |
| Drive 파일명이 `_v3`처럼 잘림 | **마지막 점 뒤 확장자 strip** | 점 없는 임시명으로 import → `update_drive_file(name=…)` 리네임 (§C-1) |

---

## C-1. Sheets 서식의 실제 한계 + import 운영 함정 (실측 — 꼭 읽기)

> 이 MCP의 Sheets 능력에는 **중요한 한계와 함정**이 있다. 모르면 동일 산출물을 여러 번 재생성하게 된다.
> (상세 절차·예시 코드는 `docs/DOC-FORMATTING.md §3-A`.)

1. **Sheets 셀 서식 직접 API 없음.** 이 서버는 Sheets에 대해 **`import_to_google_sheets`(값/시트 주입)와
   값 읽기/쓰기**만 제공하고, **셀 배경색·정렬·줄바꿈·열너비·틀고정·다중 탭(시트)·데이터 유효성(드롭다운)·
   조건부 서식을 직접 거는 API(batchUpdate/format)는 없다.** → **서식이 필요하면 `openpyxl`로 `.xlsx`를
   빌드해 `import_to_google_sheets`로 올린다**(변환 시 위 서식·탭·드롭다운·조건부서식이 그대로 보존됨 — 실측).
   "새 스프레드시트 생성 API가 없다"는 **틀린 단정**이다 — `import_to_google_sheets`가 곧 생성이다.

2. **import 경로 제약.** `import_to_google_sheets`의 `file_path`는 **`C:\Users\info\.workspace-mcp\attachments\`
   내부 파일만** 허용하고 **`file://` URL** 이어야 한다. **절대 Windows 경로(`C:\...`)는 거부된다.**
   → 빌드한 `.xlsx`를 그 attachments 디렉터리에 저장하고 `file://C:/Users/info/.workspace-mcp/attachments/파일.xlsx`로 넘긴다.

3. **파일명 확장자 strip.** `file_name`(Drive 표시명)은 **마지막 점(.) 뒤가 확장자로 잘린다**
   (`동행AI_…_v3.1` → `v3`로 깨짐). → **점 없는 임시명**으로 import한 뒤 `update_drive_file(name="정식이름")`로 리네임한다.

## D. 대안 (선택)
- **읽기 위주**면 Anthropic 레퍼런스 `@modelcontextprotocol/server-gdrive`(Node)도 가능하나
  Docs 쓰기 기능이 제한적입니다. 우리 팀은 **읽기+쓰기**가 필요하므로 위 Workspace MCP를 권장합니다.
- MCP를 아직 못 쓰는 팀원은 **폴백 모드**: 에이전트가 마크다운 초안을 주면 직접 Google Docs에
  붙여넣고, 검수도 텍스트를 붙여넣어 받을 수 있습니다.

> ⚠️ 클라이언트 시크릿·토큰은 절대 레포에 커밋하지 마세요. `.gitignore`로 이미 차단돼 있습니다.
