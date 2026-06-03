# Drive 문서 서식 표준 (Docs / Sheets)

> Drive에 올라가는 모든 문서는 이 표준대로 **가독성·가시성**을 갖춰야 합니다.
> `doc-formatter` 에이전트가 이 문서를 절대 기준으로 삼고, `/drive-sync`가 이를 호출합니다.
> **plain text를 그대로 밀어넣지 말 것.** 반드시 Google Docs/Sheets의 네이티브 서식(제목 스타일·표·색)을 적용합니다.

---

## 0. 브랜드 토큰 (뉴트럴 네이비/그레이)

색상은 **HEX 문자열**(`#RRGGBB`)로 지정한다. (이 MCP 도구는 0~1 rgbColor가 아니라 HEX를 받음)

| 토큰 | HEX | 용도 |
|---|---|---|
| 포인트(네이비) | `#1F3A5F` | 제목, 표 헤더 배경, 섹션 제목, 강조 라벨 |
| 본문 | `#202124` | 일반 본문(기본값이라 보통 미지정) |
| 보조 텍스트 | `#5F6368` | 부제·메타정보·캡션 |
| 표 헤더 글자 | `#FFFFFF` | 네이비 배경 위 헤더 글자(굵게) |
| 옅은 강조 배경 | `#EEF2F7` | 핵심 결론/요약 박스(연한 네이비틴트) |
| 위험 빨강 | `#C0392B` | 🔴 위험 텍스트 강조 |
| 구분선 회색 | `#D0D5DD` | 표 테두리·구분선 |

---

## 1. Google Docs 서식 규칙

### 1-1. 커버 헤더(모든 문서 최상단)
1. **제목** → `named_style_type: "TITLE"`, 글자색 포인트(`#1F3A5F`), 굵게.
2. **부제(한 줄 요약/문서 종류)** → `SUBTITLE`, 글자색 보조(`#5F6368`).
3. **메타정보 표**(2열): 문서 종류 / 버전 / 작성자 / 작성일 / 상태 / 관련 문서.
4. (선택) 본문 시작 전 빈 줄 1개.

### 1-2. 제목 계층 (named style 필수)
- 제목 `TITLE` · 부제 `SUBTITLE` · 대단원 `HEADING_1` · 소단원 `HEADING_2` · 세부 `HEADING_3` · 본문 `NORMAL_TEXT`.
- ⚠️ `1.`, `##`, `───` 같은 마크다운/ASCII 기호를 본문 텍스트로 남기지 말 것. **named style로 변환**한다.
- 대단원 제목엔 포인트색(`#1F3A5F`)을 입힌다.

### 1-3. 표 (가장 중요한 가독성 수단)
- 비교·목록·매핑·체크리스트는 **반드시 네이티브 표**로(ASCII 표 금지).
- 생성: `create_table_with_data(document_id, table_data=[[...],...], index=<inspect의 total_length>, bold_headers=true)`.
  - **index는 반드시 `inspect_doc_structure`의 `total_length`에서 가져온다.**
- 헤더 행 배경: 생성 후 `batch_update_doc`의 `update_table_cell_style`(`background_color:"#1F3A5F"`, `row_index:0`)로 칠하고,
  헤더 글자는 `format_text`(`text_color:"#FFFFFF"`, `bold:true`)로 흰 굵은 글씨.
- **셀 가독성**: 셀 내용은 잘리지 않게 자동 줄바꿈으로 흐르게 하고, 열이 너무 좁아 본문이 끼이지 않도록 긴 텍스트 열엔 충분한 폭을 준다(Docs 표는 보통 셀 높이가 내용에 맞춰 늘어남).

### 1-4. 강조·박스
- **핵심 결론/한 줄 요약**: 해당 단락에 `update_paragraph_style`(`shading_color:"#EEF2F7"`) + `format_text`(`bold:true`).
- 심각도: 🔴 필수 / 🟡 권장 / 🟢 제안 (이모지 + 필요 시 빨강 텍스트 `#C0392B`).

### 1-5. 목록
- 글머리표/번호: `create_bullet_list`(`list_type:"UNORDERED"|"ORDERED"|"CHECKBOX"`) 또는 `update_paragraph_style(list_type=...)`.
- "- ", "* ", "1) " 같은 텍스트 기호를 그대로 두지 말 것.

### 1-6. 여백·간격
- 단락 뒤 간격 `space_below: 6~8`(pt)로 숨 쉴 공간 확보.
- 표 앞뒤로 빈 단락 1개.
- Mermaid 등 렌더 불가 코드블록은 고정폭 글꼴 + 옅은 회색 배경으로 보존하고 아래에 설명 한 줄.

---

## 2. 도구 사용법 핵심 (이 MCP 기준 — 실수 방지)

- **`batch_update_doc`는 `operations=[{type, ...}]` 형식.** Google 원시 `requests`/`rgbColor`를 쓰지 말 것.
  주요 type: `insert_text`(`end_of_segment:true` 또는 `index`), `format_text`(`bold`,`text_color`,`font_size`…),
  `update_paragraph_style`(`named_style_type`,`heading_level`,`shading_color`,`space_below`,`alignment`),
  `update_table_cell_style`(`table_start_index`,`row_index`,`column_index`,`background_color`),
  `create_bullet_list`, `insert_table`, `merge_table_cells`.
- 색은 전부 **HEX 문자열**.
- **⚠️ 탭(tab) 문서 주의**: 새 Doc은 보통 단일 탭(`t.0`)을 가진다. 이 경우 `inspect_doc_structure`·`create_table_with_data`·
  `batch_update_doc`의 모든 operation에 **`tab_id`(예: `"t.0"`)를 넘겨야** 내용이 보이고 서식이 적용된다.
  `inspect_doc_structure`가 `total_length:0`인데 글은 들어가 있다면 → tab_id를 빠뜨린 것이다. 먼저 tab_id 없이 호출해 `tabs[].tab_id`를 확인하라.
- **표 자리에 빈 줄(placeholder)을 먼저 만든다**: 본문을 한 번에 넣을 때, 표가 들어갈 위치마다 공백 문자(`　`) 한 줄을 미리 넣어둔다.
  그 빈 줄의 시작 인덱스에 표를 삽입하면 표가 **문단 중간을 가르지 않고** 정확히 그 자리에 들어간다.
  (표를 본문 문단 중간 인덱스에 직접 꽂으면 본문이 토막나서 깨진다 — 실제 발생했던 사고.)
- **표는 문서 뒤→앞 순서로 삽입한다**: 여러 표를 넣을 때 뒤쪽(인덱스 큰 곳)부터 만들면 앞쪽 인덱스가 밀리지 않아 위치가 안 어긋난다.
- **표 삽입 후 서식 적용 전 반드시 inspect를 다시 호출**한다: 표가 추가되면 모든 인덱스가 바뀌므로, 서식 batch 직전에 최신 구조를 받아 그 값으로 적용한다.
- **인덱스 안전 절차**: ① 텍스트/표를 위→아래 순서로 다 넣는다(텍스트는 `end_of_segment:true`, 표는 직전에 `inspect_doc_structure`로 `total_length`를 얻어 그 index에).
  ② 모두 넣은 뒤 `inspect_doc_structure(detailed=true)`로 각 단락의 start/end index를 받는다.
  ③ 그 인덱스로 한 번에 `format_text`·`update_paragraph_style`·`update_table_cell_style`를 적용한다.
- 표 셀 색 적용 전 `debug_table_structure`로 표 위치·셀 인덱스를 확인하면 안전하다.

---

## 3. Google Sheets 서식 규칙 (형식 — "무엇처럼 보여야 하나")

> ⭐ **먼저 읽어라 — Sheets 서식의 진실(실측 확인):** 이 팀의 google-workspace MCP는 Sheets에
> 대해 **`import_to_google_sheets`(값/시트 주입)만** 제공하고, **셀 배경색·정렬·줄바꿈·열너비·
> 틀고정·다중 탭(시트)·데이터 유효성(드롭다운)·조건부 서식을 직접 거는 API(batchUpdate/format)는
> 없다.** 따라서 **서식이 필요한 Sheets 산출물은 §3-A 절차(openpyxl로 .xlsx 빌드 → import)로 만든다.**
> §3·§3-0의 "무엇처럼 보여야 하나"(형식 목표)는 그대로 유효하며, **그것을 실현하는 수단이 XLSX 빌드**다.

- 헤더 행: 포인트색(`#1F3A5F`) 배경 + 흰색 굵은 글씨 + 가운데 정렬.
- 헤더 1행(필요 시 1열) **틀 고정(freeze)**.
- 목록형 시트는 헤더에 **자동필터**. 표 전체 옅은 테두리(`#D0D5DD`).
- 상태/우선순위 열은 색 구분(🔴/🟡/🟢, P0=빨강 텍스트). **폭은 좁게**(헤더 텍스트 너비 정도).
- 첫 시트 상단 또는 "문서정보" 시트에 메타정보(버전/작성자/일자).
- 색 토큰은 §0과 **동일**하게 쓴다(헤더배경 `#1F3A5F`/헤더글자 `#FFFFFF`/테두리 `#D0D5DD`/위험 `#C0392B`/옅은강조 `#EEF2F7`).

### 3-0. ⭐ 가독성 마무리 (필수 — "셀 내용이 잘리면 안 된다")

> **가장 흔한 불만: "셀이 글보다 작아서 글이 잘려/안 보인다."** 값을 다 채운 뒤
> **반드시 아래 마무리를 적용**해 셀 내용이 잘리지 않고 한눈에 보이게 한다. 이건 도구 유무와
> 무관한 **필수 보장 규칙**이다(도구가 없으면 §3-1-E 폴백으로 사용자에게 수동 적용을 안내).

1. **열 너비 — 내용에 맞춘다.**
   - 각 열을 내용 길이에 맞게 **자동맞춤(auto-resize)** 한다. 자동맞춤 도구가 없으면 **열별 권장 너비**를 명시 지정.
   - **헤더 텍스트보다 좁은 열 금지** (헤더 글자가 잘리면 안 됨).
   - 너무 긴 텍스트 열(설명·비고·기대결과 등)은 **상한 너비(약 300~400px 상당)**를 두고, 그 이상은 아래 줄바꿈으로 처리(가로로 무한히 늘리지 않음).

2. **줄바꿈(wrap) + 행 높이 — 반드시 세트로.**
   - 긴 텍스트 열은 **자동 줄바꿈(wrap) ON**.
   - 그리고 **행 높이가 줄바꿈된 내용을 전부 담도록 자동 확장**되어야 한다(클립/오버플로로 잘리지 않게).
   - ⚠️ **wrap만 켜고 행 높이가 안 늘면 여전히 잘린다.** wrap과 행 높이 자동확장은 **반드시 둘을 함께** 적용한다.

3. **정렬 — 위에서부터 읽히게.**
   - 세로 정렬 **상단(top)** (줄바꿈된 셀이 위에서부터 보이게).
   - 가로 정렬: 텍스트 = **왼쪽**, 숫자·상태·우선순위 = **가운데**.

4. **헤더 가독성.**
   - 헤더 1행 **틀 고정** + 네이비 배경(`#1F3A5F`) + 흰 굵은 글씨 + 가운데 정렬(§0 토큰 재사용).
   - 헤더는 줄바꿈보다 **충분한 열 너비**를 우선해 한 줄로 보이게 한다.

5. **병합 셀 주의.**
   - 무분별한 셀 병합은 **줄바꿈·필터·정렬을 깨뜨린다.** 데이터 영역에는 병합을 쓰지 말고,
     메타/제목 영역(문서정보 시트 등) 정도로만 제한한다.

> 정확한 도구명/파라미터(열 너비·행 높이·wrap·정렬 설정 도구)에 확신이 없으면 §3-1-E처럼
> `(확인 필요: 정확한 도구명/시그니처)`로 표기한다. 다만 **위 1~5의 보장 목표(잘림 금지·줄바꿈+행높이 세트)는
> 도구와 무관한 필수 규칙**이므로 단정해서 지킨다.

---

## 3-A. ⭐ Sheets 서식 표준 절차 — openpyxl XLSX 빌드 → import (검증된 정석)

> **핵심 사실(실측):** google-workspace MCP는 Sheets 셀 서식 API가 **없다.** 하지만 로컬에서
> **openpyxl로 `.xlsx`를 빌드**해 그 안에 헤더색·정렬·줄바꿈·열너비·**틀고정·다중 탭·드롭다운·
> 조건부서식**을 모두 넣은 뒤 **`import_to_google_sheets`로 업로드하면, 변환 시 이 서식·탭·드롭다운·
> 조건부서식이 그대로 보존된다.** 즉 **서식 있는 Sheets = "XLSX로 만들어서 import"** 가 정석이다.
> (Sheets에 직접 셀서식을 거는 우회를 시도하다 중단·낭비하지 말 것 — `doc-formatter` 정의 참조.)

### 3-A-1. 절차 (5단계)
1. **빌드**: 로컬에서 `openpyxl`로 `.xlsx`를 만든다. 시트(탭)·값·서식·드롭다운·조건부서식을 전부 코드로 지정.
2. **임시 위치로 둔다**: 파일을 **`C:\Users\info\.workspace-mcp\attachments\`** 안에 저장한다(§3-A-3 경로 함정).
   파일명은 **점(.)이 없는 임시명**으로 한다(예: `techspec_tmp` — §3-A-3 확장자 strip 함정).
3. **import**: `import_to_google_sheets(file_path="file://C:/Users/info/.workspace-mcp/attachments/techspec_tmp.xlsx", source_format="xlsx", ...)`
   — **`file://` URL** 로 넘긴다(절대 Windows 경로 `C:\...`는 거부됨). 이 호출로 **새 스프레드시트가 생성**된다.
4. **리네임**: import 직후 `update_drive_file(file_id=<생성된 ID>, name="동행AI_기술명세서_v3.1")`로 최종 이름을 준다
   (점 없는 임시명으로 import했으므로 여기서 정식 이름 부여).
5. **폴더 이동**: 필요 시 `update_drive_file(add_parents=<대상폴더ID>, remove_parents="root")`로 지정 폴더에 넣는다.

### 3-A-2. openpyxl 빌드 예시 (헤더 브랜드색·틀고정·다중 탭·드롭다운·조건부서식)

```python
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side
from openpyxl.worksheet.datavalidation import DataValidation
from openpyxl.formatting.rule import FormulaRule
from openpyxl.utils import get_column_letter

NAVY   = "1F3A5F"   # 포인트(헤더 배경) — openpyxl은 '#' 없는 6자리 ARGB/RGB
WHITE  = "FFFFFF"
BORDER = "D0D5DD"
RED    = "C0392B"   # 지연/위험
ORANGE = "E67E22"   # 차단

wb = Workbook()

# ── 본문 시트(탭 1) ───────────────────────────────────────────
ws = wb.active
ws.title = "기술명세서"
headers = ["ID","모듈","기능명","사용자가치","기술구현방식","기술비기능","처리흐름","예외","Phase","우선순위"]
ws.append(headers)

thin = Side(style="thin", color=BORDER)
border = Border(left=thin, right=thin, top=thin, bottom=thin)

# 헤더 서식: 네이비 배경 + 흰 굵은 글씨 + 가운데
for col, _ in enumerate(headers, start=1):
    c = ws.cell(row=1, column=col)
    c.fill = PatternFill("solid", fgColor=NAVY)
    c.font = Font(color=WHITE, bold=True)
    c.alignment = Alignment(horizontal="center", vertical="center", wrap_text=True)
    c.border = border

# 데이터 행(예시) — 실제는 명세 행들을 append
# ws.append([ "REQ-001","인증","로그인", ... ])

# 본문 셀: 줄바꿈 + 상단 정렬 + 테두리
for row in ws.iter_rows(min_row=2, max_row=ws.max_row, max_col=len(headers)):
    for c in row:
        c.alignment = Alignment(wrap_text=True, vertical="top",
                                horizontal="center" if c.column_letter in ("I","J") else "left")
        c.border = border

# 열 너비(긴 텍스트 열은 넓게, 상태/우선순위는 좁게)
widths = {"A":12,"B":14,"C":18,"D":28,"E":40,"F":28,"G":34,"H":28,"I":10,"J":12}
for col, w in widths.items():
    ws.column_dimensions[col].width = w

# 틀고정: 헤더 1행(+1열까지면 "B2")
ws.freeze_panes = "A2"

# 자동필터(헤더 범위)
ws.auto_filter.ref = f"A1:{get_column_letter(len(headers))}1"

# 드롭다운(데이터 유효성): 우선순위(J), Phase(I)
dv_pri = DataValidation(type="list", formula1='"P0,P1,P2,P3"', allow_blank=True)
dv_pha = DataValidation(type="list", formula1='"MVP,Phase2,Phase3"', allow_blank=True)
ws.add_data_validation(dv_pri); ws.add_data_validation(dv_pha)
dv_pri.add(f"J2:J{ws.max_row}"); dv_pha.add(f"I2:I{ws.max_row}")

# 조건부 서식: 우선순위=P0이면 빨강 글씨(FormulaRule)
ws.conditional_formatting.add(
    f"J2:J{ws.max_row}",
    FormulaRule(formula=['$J2="P0"'], font=Font(color=RED, bold=True))
)

# ── 개요·부록 시트(탭 2) ──────────────────────────────────────
ws2 = wb.create_sheet("개요·부록")
ws2.append(["항목","값"])
for c in ws2[1]:
    c.fill = PatternFill("solid", fgColor=NAVY); c.font = Font(color=WHITE, bold=True)
ws2.append(["문서종류","기술명세서"]); ws2.append(["버전","v3.1"])
ws2.column_dimensions["A"].width = 16; ws2.column_dimensions["B"].width = 40

wb.save(r"C:\Users\info\.workspace-mcp\attachments\techspec_tmp.xlsx")
```

> 위 빌드가 §3·§3-0의 가독성 목표(헤더 브랜드색·틀고정·줄바꿈+상단정렬·셀 잘림 방지·상태색)를
> 실제로 달성한다. 색 토큰은 §0과 동일(단, openpyxl은 `#` 없는 6자리 HEX).

### 3-A-3. import 운영 함정 2가지 (반드시 회피)
1. **경로 제약**: `import_to_google_sheets`의 `file_path`는 **`C:\Users\info\.workspace-mcp\attachments\`
   내부 파일만** 허용하고 **`file://` URL** 형식이어야 한다. 절대 Windows 경로(`C:\...`)는 **거부**된다.
   → 빌드 결과물을 그 디렉터리에 저장하고 `file://.../파일.xlsx`로 넘긴다.
2. **파일명 확장자 strip**: `file_name`(또는 Drive 표시명)은 **마지막 점(.) 뒤가 확장자로 잘린다**
   (`동행AI_…_v3.1` → `v3`로 깨짐). → **점 없는 임시명**으로 import한 뒤 `update_drive_file(name="정식이름")`로 리네임한다.

### 3-A-4. 폴백 (openpyxl/ import 불가 시)
- openpyxl이 없거나 import 도구가 막히면 **거짓 도구명을 지어내지 말고**, ① 값(데이터)은 정확히 채우고
  ② 적용 못 한 서식을 **명시**한 뒤 ③ 그룹장에 보고하고 대안(XLSX 수동 빌드 안내 등)을 제시한다. **임의 매체 우회(마크다운/Docs로 도피) 금지** — 그룹장 판단을 받는다.

---

## 3-1. Sheets 도구 사용법 핵심 (이 MCP 기준 — 실전 작성법)

> Docs의 §2에 상응하는 **Sheets 작성/수정 절차**. 사용 서버는 `workspace-mcp`
> (taylorwilsdon/google_workspace_mcp). 아래는 **능력(capability) 단위**로 절차를 기술하고,
> 정확한 도구명/파라미터가 검증되지 않은 부분은 `(확인 필요: …)`로 정직하게 표기한다.
> **거짓 도구명을 단정하지 말 것.** 실제 호출 시 `/mcp`로 노출된 도구 목록에서 이름을 확인해 맞춘다.
>
> ⚠️ **서식이 필요하면 §3-A(XLSX 빌드 → import)가 정석이다.** 아래 §3-1은 **서식 없는 값 주입/읽기**
> (예: 기존 시트의 값만 추가·갱신)와 셀범위·안전절차에 대한 보조 절차다. 이 MCP에는 **셀 서식 직접 API가
> 없으므로**(헤더색·틀고정·드롭다운·조건부서식·줄바꿈은 §3-A로만 보장), 서식 산출물은 §3-A로 만든다.

### 3-1-A. Sheets 도구 능력 맵 (이름은 호출 전 `/mcp`로 확인)

| 능력 | 도구 / 경로 | 비고 |
|---|---|---|
| **새 스프레드시트 생성 + 서식** | **XLSX 빌드 → `import_to_google_sheets`** (§3-A) | ⭐ **서식 있는 새 시트는 이 경로로 생성된다.** "생성 API 없다"는 **틀렸다** — import가 곧 생성 |
| 스프레드시트 목록/검색 | `list_spreadsheets` / Drive 검색 | |
| 셀 값 **읽기** | `read_sheet_values`(`spreadsheet_id`, `range`) | range는 A1 표기(`시트명!A1:F100`). 후속 에이전트가 콘텐츠를 받을 때 활용 |
| 셀 값 **쓰기/수정**(서식 없음) | `modify_sheet_values`(`spreadsheet_id`, `range`, `values`) | `values`=2D 배열. **값만** 들어간다(셀 서식 못 검). 덮어쓰기 주의(아래 D) |
| 서식(배경색·틀고정·필터·드롭다운·조건부서식·줄바꿈·열너비·정렬) | **직접 API 없음 → §3-A(XLSX 빌드 후 import)로만 보장** | 이 MCP에 셀 포맷 batchUpdate가 **없음**(실측). 직접 시도하다 중단·낭비 금지 |
| 파일 리네임/폴더 이동 | `update_drive_file`(`name`, `add_parents`, `remove_parents`) | import 후 정식 이름 부여(§3-A-3)·폴더 이동 |

> ⚠️ `read_sheet_values`/`modify_sheet_values`의 **시그니처(파라미터명)가 100% 확정은 아니다.** 첫 호출 전
> 노출 도구 목록(`/mcp`)으로 대조하고, 어긋나면 노출된 실제 이름을 따른다.
> **핵심:** 셀 서식이 필요하면 위 표의 "값 주입" 도구로 헤매지 말고 **§3-A(XLSX→import)** 로 직행한다.

### 3-1-B. 작성 절차 (순서 — Docs §2의 3단계에 대응)

1. **생성/열기**: **새 서식 산출물은 §3-A(XLSX 빌드 → `import_to_google_sheets`)로 만든다**(이 경로가 새 시트를
   생성한다). 기존 시트에 **값만** 추가/갱신하는 경우에만 ID로 열고 먼저 §3-1-D(수정 안전절차)로 현재 구조를 읽는다.
2. **시트(탭) 구성**: 본문 시트 1개 + `문서정보` 시트 1개를 기본으로. 필요 시 `create_sheet`로 추가.
   첫 시트 이름은 문서 종류에 맞게(예: `요구사항`, `테스트케이스`).
3. **헤더 행 작성**: 1행에 컬럼 헤더를 쓴다. range는 `시트명!A1:<마지막열>1`, `values=[[ "ID","구분",… ]]`.
   헤더 컬럼은 §3-1-C의 문서종류별 레이아웃을 따른다(=`templates/`의 표 컬럼과 일치).
4. **데이터 행 입력**: **2행부터** 데이터를 쓴다. range는 `시트명!A2:<마지막열><N+1>` (N=항목 수).
   한 행 = 한 항목(요구사항/엔드포인트/테스트케이스 1건). 값은 2D 배열로 한 번에 보낸다(행별 분할 호출 지양).
5. **서식 적용**: 헤더 배경(`#1F3A5F`)·헤더 글자(흰색 굵게)·틀고정(1행)·자동필터·상태색·옅은 테두리(`#D0D5DD`).
   → **이 MCP엔 셀 서식 직접 API가 없으므로 §3-A(XLSX 빌드 후 import)로 적용한다.** 값만 주입하는 경우 서식은 미적용임을 명시(§3-1-E).
6. **메타정보 시트**: `문서정보` 시트에 문서종류/버전/작성자/작성일/상태/관련문서를 2열(키-값)로 기록.
7. **⑥ 가독성 마무리 (필수 — 마지막 단계, 생략 금지)**: §3-0을 그대로 적용한다.
   **열 너비 자동맞춤(헤더보다 좁지 않게)** + **긴 텍스트 열 줄바꿈(wrap) ON + 행 높이 자동확장(세트)** +
   **세로 상단 정렬 / 가로 텍스트=왼쪽·숫자상태=가운데**. → **셀 내용이 잘리지 않는지** 확인하고 끝낸다.
   전용 도구가 불확실/부재면 §3-1-E 폴백(값은 채우고, 미적용 서식을 사용자에게 명시).

### 3-1-C. A1 표기·범위(range) 규칙

- 형식: **`시트명!시작셀:끝셀`** (예: `요구사항!A1:H1` = 헤더, `요구사항!A2:H51` = 데이터 50행).
- **헤더는 1행, 데이터는 2행부터.** 헤더와 데이터를 한 range로 섞어 쓰지 말고 분리해 쓴다(헤더 보존 용이).
- 시트명에 공백/한글이 있으면 작은따옴표로 감싼다: `'문서 정보'!A1:B10`.
- **다음 빈 행 계산**: 현재 데이터가 헤더 포함 R행이면, 다음 추가 행은 `R+1`행.
  새 항목 K건을 **추가**할 때 range는 `시트명!A<R+1>:<끝열><R+K>` (기존 행을 덮지 않음).
- 전체 시트를 받을 땐 열만 지정: `시트명!A:H`. (행수 불확실할 때 읽기용)

### 3-1-C-1. 문서종류별 시트 레이아웃 (헤더 컬럼 = `templates/`의 표 컬럼과 일치)

각 행 = 한 항목. 헤더 컬럼은 해당 템플릿의 항목 표 컬럼을 그대로 1행에 옮긴다.
키-값 형태 템플릿(기능명세·API·화면)은 **항목을 행으로 펴서(flatten)** 아래 권장 컬럼을 쓴다.

| 문서 | 시트 헤더(1행 컬럼) | 행 단위 |
|---|---|---|
| 요구사항정의서 | `ID` / `요구사항` / `우선순위` / `사유` / `인수기준(검증 방법)` / `출처` | 요구사항 1건 |
| 기능명세서 | `기능 ID` / `연결 요구사항` / `설명` / `사용자·권한` / `사전조건` / `입력` / `처리 규칙` / `출력·결과` / `유효성 검증` / `예외·에러` / `화면 ID` / `API ID` / `TC ID` | 기능 1건 |
| API명세서 | `API ID` / `메서드` / `경로` / `설명` / `인증` / `요청(Body/Param)` / `응답` / `에러코드` / `연결 기능` | 엔드포인트 1건 |
| DB설계서 | `테이블` / `컬럼` / `타입(길이)` / `PK` / `FK` / `NULL` / `기본값` / `UNIQUE` / `설명` | 컬럼 1건 (테이블별 반복) |
| 테스트케이스 | `TC ID` / `연결 요구사항·기능` / `유형` / `사전조건` / `입력·절차` / `기대결과` / `우선순위` | 테스트케이스 1건 |
| 화면정의서 | `화면 ID` / `화면명` / `목적` / `구성요소·UI` / `상태·동작` / `기능 ID` / `API ID` | 화면(또는 구성요소) 1건 |
| 문서관리대장 | `문서 ID` / `문서명` / `종류` / `버전` / `상태` / `작성자` / `최종수정일` / `Drive 링크` | 문서 1건 |

> 정확한 컬럼은 각 `templates/<문서>.md`의 항목 표를 우선한다(템플릿이 갱신되면 그쪽이 기준).
> 추적성 컬럼(요구사항 ID↔기능 ID↔TC ID)은 가능한 한 유지해 문서 간 일관성을 맞춘다.

### 3-1-D. 수정(갱신) 안전절차 (Docs "덮어쓰기 전 읽기"와 동일 원칙)

1. **먼저 읽는다**: `read_sheet_values`로 현재 값/구조(헤더 컬럼, 행 수, 시트 탭 목록)를 확보한다.
   `get_spreadsheet_info` **(확인 필요: 정확한 도구명)** 로 시트 탭/범위를 먼저 파악하면 더 안전.
2. **덮어쓸 범위를 먼저 정한다**: 어느 `시트명!range`를 바꾸는지 명확히 한 뒤에만 쓴다.
3. **행 삽입 vs 값 덮어쓰기 구분**:
   - 기존 행을 **수정**: 해당 행 range만 `modify_sheet_values`로 덮어쓴다(다른 행 안 건드림).
   - 새 항목 **추가**: §3-1-C의 "다음 빈 행"부터 쓴다. **기존 데이터 위에 쓰면 밀림/손실** 발생 — 주의.
   - 행 사이 **끼워넣기**(아래 행을 밀어야 함)는 값 덮어쓰기로 안 됨 → 행 삽입 능력 **(확인 필요: 행 삽입 도구/방식)** 필요. 불확실하면 사용자에게 알리고 끝에 추가하는 방식을 제안.
4. **헤더 행(1행)은 보존**한다. 데이터 갱신이 헤더를 덮지 않도록 range를 2행부터로 잡는다.
5. 큰 변경 전에는 그룹장 원칙대로 **현재 내용을 1회 읽어 요약**하고, 손실 위험이 있으면 사용자에게 먼저 알린다(§CLAUDE.md 5).

### 3-1-E. 서식 적용 폴백 (XLSX 빌드/import가 불가할 때)

- **1순위는 §3-A(openpyxl XLSX 빌드 → `import_to_google_sheets`)다.** 이 MCP엔 Sheets 셀 서식
  직접 API가 없으므로, 헤더색·틀고정·드롭다운·조건부서식·줄바꿈은 §3-A로만 보장된다.
- §3-A가 막힐 때(openpyxl 부재·import 도구 미노출 등)에만 아래 폴백: ① 값(데이터)은 정확히 채우고 ② 서식·가독성 마무리는 적용 못 함을 **명시**한 뒤
  ③ 사용자에게 "헤더 고정·색·필터 **및 열 너비 자동맞춤·긴 열 줄바꿈+행 높이 확장**을 시트에서 1회 수동 적용"을
  안내(또는 색 구분을 텍스트/이모지로 대체)한다. 거짓 포맷 도구명을 지어내 호출하지 않는다.
  > ⚠️ 특히 **"셀이 글보다 작아 잘려 보이는"** 문제를 막는 열너비·줄바꿈+행높이는 사용자에게 꼭 짚어 안내한다.

---

## 3-2. 간트차트·칸반보드 특유 서식 (1개 스프레드시트 — `project-scheduler` / `/project-plan`)

> 기본 가독성은 §3·§3-0(셀 잘림 금지·줄바꿈+행높이 세트·헤더 틀고정)을 **그대로** 따른다.
> 아래는 타임라인·보드 특유의 추가 규칙만. 컬럼·연동(수식) 설계는 `templates/간트차트-칸반보드.md`.

- **이번 주 강조(필수).** `Gantt`의 이번 주에 해당하는 날짜 열은 배경색/세로 라인으로 강조해
  "지금 무슨 일이 진행 중"이 즉시 보이게 한다(`This Week` 시트와 함께 주간 포커스 제공).
- **상태별 막대 색.** 대기=`#D0D5DD` / 진행중=`#1F3A5F` / 검수중=`#EEF2F7`배경+네이비테두리 /
  완료=연한 녹색 또는 체크 / **지연=위험 빨강 `#C0392B`**. 가능하면 **조건부서식/수식**으로 자동 색칠.
- **칸반 컬럼.** 상태(대기/진행중/검수중/완료) 컬럼에 `FILTER`/`QUERY`로 `Tasks`를 읽어 자동 분류.
  카드에 작업명·담당자·검수자·기간. 데이터 영역 **셀 병합 금지**(필터·정렬 깨짐).
- **단방향.** `Tasks`가 입력원, 간트·칸반은 읽어서 표시만. 상태는 `Tasks`에서 바꾼다.
- **드롭다운(데이터유효성)·조건부서식·틀고정은 §3-A(XLSX 빌드)로 넣고 import한다**(MCP 직접 API 없음).
  단 **수식(FILTER/QUERY)**은 XLSX에 그대로 담아 import하면 Sheets에서 재계산되므로 셀에 수식 문자열로 넣는다
  (단방향 연동 유지). 정확한 동작이 불확실하면 `(확인 필요)` 표기 + §3-1-E 폴백. 거짓 도구명 호출 금지.

---

## 4. 품질 체크 (서식 통과 조건)
- [ ] 커버 헤더(제목 TITLE + 부제 + 메타표)가 있는가
- [ ] 모든 제목이 named style인가(마크다운/ASCII 기호 없음)
- [ ] 비교·목록이 네이티브 표·목록인가(ASCII 표 없음)
- [ ] 표 헤더가 네이비 배경 + 흰 글씨인가
- [ ] 핵심 결론이 강조 박스(연한 네이비틴트)로 보이는가

**Sheets 산출물 (§3-A / §3 / §3-1)**
- [ ] **서식 산출물을 XLSX 빌드→`import_to_google_sheets`로 만들었는가**(§3-A) — 직접 셀서식 시도 아님
- [ ] import를 **attachments + `file://`** 로 했고 **점 없는 임시명→`update_drive_file` 리네임**했는가(확장자 strip 회피, §3-A-3)
- [ ] 헤더 1행이 네이비 배경(`#1F3A5F`) + 흰 굵은 글씨 + 가운데 정렬인가
- [ ] 헤더 1행 **틀 고정(freeze)** 됐는가
- [ ] **⭐ 셀 내용이 잘리지 않는가** — 열 너비 자동맞춤(헤더보다 좁지 않게) + 긴 열 **줄바꿈(wrap) + 행 높이 자동확장**을 **세트로** 적용했는가(§3-0)
- [ ] 세로 정렬 상단(top) + 가로 정렬(텍스트 왼쪽/숫자·상태 가운데)으로 위에서부터 읽히는가
- [ ] 데이터 영역에 무분별한 **셀 병합이 없는가**(필터·정렬·줄바꿈 깨짐 방지)
- [ ] 목록형 시트에 **자동필터** + 옅은 테두리(`#D0D5DD`)가 있는가
- [ ] 상태/우선순위 열이 색(🔴/🟡/🟢, P0=빨강)으로 구분되는가
- [ ] 컬럼 헤더가 **해당 템플릿의 표 컬럼과 일치**하는가(§3-1-C-1), 각 행=한 항목인가
- [ ] `문서정보` 시트(또는 상단)에 메타정보(버전/작성자/일자)가 있는가
- [ ] (수정 시) 먼저 읽고 → 범위 정하고 → **헤더 1행 보존** + 기존 행 밀림 없이 반영했는가
- [ ] (서식 도구 불확실/부재 시) 값은 채우고, 미적용 서식을 사용자에게 **명시**했는가(거짓 도구명 호출 안 함)
