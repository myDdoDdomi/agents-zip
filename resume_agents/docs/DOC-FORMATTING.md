# Drive 문서 서식 표준 (이력서 · 포트폴리오 — Google Docs)

> Drive에 올라가는 이력서·포트폴리오는 이 표준대로 **가독성·가시성**을 갖춰야 합니다.
> `docs-formatter` 에이전트가 이 문서를 절대 기준으로 삼고, `/drive-sync`가 이를 호출합니다.
> **plain text를 그대로 밀어넣지 말 것.** Google Docs의 네이티브 서식(제목 스타일·표·색)을 적용합니다.
> 이 팀은 **Docs만** 사용합니다(Sheets/Slides/Calendar 미사용 — 권한 표면 최소화).

---

## 0. 브랜드 토큰 (뉴트럴 네이비/그레이)

색상은 **HEX 문자열**(`#RRGGBB`)로 지정한다. (이 MCP 도구는 0~1 rgbColor가 아니라 HEX를 받음)

| 토큰 | HEX | 용도 |
|---|---|---|
| 포인트(네이비) | `#1F3A5F` | 제목, 섹션 제목, 표 헤더 배경, 강조 라벨 |
| 본문 | `#202124` | 일반 본문(기본값이라 보통 미지정) |
| 보조 텍스트 | `#5F6368` | 부제·메타정보·캡션 |
| 표 헤더 글자 | `#FFFFFF` | 네이비 배경 위 헤더 글자(굵게) |
| 옅은 강조 배경 | `#EEF2F7` | 요약(헤드라인)·핵심 성과 박스 |
| 구분선 회색 | `#D0D5DD` | 표 테두리·구분선 |

---

## 1. Google Docs 서식 규칙

### 1-1. 커버 헤더(문서 최상단)
1. **이름/제목** → `named_style_type: "TITLE"`, 글자색 포인트(`#1F3A5F`), 굵게.
2. **부제(지원 회사/직무·한 줄 요약)** → `SUBTITLE`, 글자색 보조(`#5F6368`).
3. (이력서) 연락처·링크 한 줄(사용자 명시 범위만, 민감정보 제외).

### 1-2. 제목 계층 (named style 필수)
- 제목 `TITLE` · 부제 `SUBTITLE` · 대단원 `HEADING_1`(요약/경력/프로젝트/스킬) · 소단원 `HEADING_2`(개별 경력·프로젝트) · 본문 `NORMAL_TEXT`.
- ⚠️ `1.`, `##`, `───`, `- ` 같은 마크다운/ASCII 기호를 본문 텍스트로 남기지 말 것 → **named style/네이티브 목록으로 변환**.
- 대단원 제목엔 포인트색(`#1F3A5F`)을 입힌다.

### 1-3. 표 (스킬·기타 프로젝트 목록 등)
- 스킬 매트릭스·기타 프로젝트 목록은 **네이티브 표**로(ASCII 표 금지).
- 생성: `create_table_with_data(document_id, table_data=[[...],...], index=<inspect의 total_length>, bold_headers=true)`.
  - **index는 반드시 `inspect_doc_structure`의 `total_length`에서 가져온다.**
- 헤더 행 배경: 생성 후 `update_table_cell_style`(`background_color:"#1F3A5F"`, `row_index:0`), 헤더 글자는 `format_text`(`text_color:"#FFFFFF"`, `bold:true`).

### 1-4. 강조·박스
- **요약(헤드라인)·핵심 성과**: 해당 단락에 `update_paragraph_style`(`shading_color:"#EEF2F7"`) + `format_text`(`bold:true`).
- 경력·프로젝트의 결과(임팩트)는 굵게/포인트색으로 살짝 강조(과한 색칠 금지 — 이력서는 차분하게).

### 1-5. 목록
- 경력·프로젝트의 행동/성과는 `create_bullet_list`(`list_type:"UNORDERED"`)로. "- ", "* " 같은 텍스트 기호를 그대로 두지 말 것.

### 1-6. 여백·간격·분량
- 단락 뒤 간격 `space_below: 6~8`(pt)로 숨 쉴 공간 확보. 표 앞뒤로 빈 단락 1개.
- **분량**: 이력서는 1~2p, 포트폴리오는 프로젝트당 간결하게(가독성 우선). 빼곡함보다 여백.

---

## 2. 도구 사용법 핵심 (이 MCP 기준 — 실수 방지)

- **`batch_update_doc`는 `operations=[{type, ...}]` 형식.** Google 원시 `requests`/`rgbColor`를 쓰지 말 것.
  주요 type: `insert_text`(`end_of_segment:true` 또는 `index`), `format_text`, `update_paragraph_style`(`named_style_type`,`shading_color`,`space_below`), `update_table_cell_style`, `create_bullet_list`, `insert_table`.
- 색은 전부 **HEX 문자열**.
- **⚠️ 탭(tab) 문서 주의**: 새 Doc은 보통 단일 탭(`t.0`). 이 경우 `inspect_doc_structure`·`create_table_with_data`·`batch_update_doc`의 모든 operation에 **`tab_id`(예: `"t.0"`)를 넘겨야** 내용이 보이고 서식이 적용된다. `total_length:0`인데 글이 있으면 tab_id 누락.
- **표 자리에 빈 줄(placeholder)을 먼저 만든다**: 표가 들어갈 위치마다 공백 문자 한 줄을 미리 넣고, 그 줄 시작 인덱스에 표를 삽입(본문 문단 중간을 가르지 않게).
- **표는 뒤→앞 순서로 삽입**한다(앞 인덱스가 밀리지 않음).
- **표 삽입 후 서식 적용 전 반드시 inspect를 다시 호출**한다(인덱스가 바뀌므로 최신 구조로 적용).
- **인덱스 안전 절차**: ① 텍스트/표를 위→아래로 다 넣고(텍스트 `end_of_segment:true`, 표는 직전 inspect의 `total_length` index에) → ② `inspect_doc_structure(detailed=true)`로 각 단락 start/end 받고 → ③ 그 인덱스로 한 번에 서식 적용.

---

## 3. 파일명·폴더 규칙
- 파일명: `[회사]_[직무]_이력서_v1`, `[회사]_[직무]_포트폴리오_v1` (회사·직무·버전 명시).
- 같은 사람의 여러 기업 지원본은 회사명으로 구분해 헷갈리지 않게 한다.
- 기존 문서 갱신 시 버전(v2…)을 올리고 덮어쓰기 전 사용자 확인.

> MCP 미연결이면 위 서식을 적용한 마크다운을 산출하고, 사용자가 복사해 Google Docs에 붙여넣도록 안내(폴백).
