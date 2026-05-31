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

## 3. Google Sheets 서식 규칙

- 헤더 행: 포인트색(`#1F3A5F`) 배경 + 흰색 굵은 글씨 + 가운데 정렬.
- 헤더 1행(필요 시 1열) **틀 고정(freeze)**.
- 긴 텍스트 열은 **줄바꿈(wrap)** + 열 너비 조정(텍스트 잘림 방지).
- 목록형 시트는 헤더에 **자동필터**. 표 전체 옅은 테두리(`#D0D5DD`).
- 상태/우선순위 열은 색 구분(🔴/🟡/🟢, P0=빨강 텍스트).
- 첫 시트 상단 또는 "문서정보" 시트에 메타정보(버전/작성자/일자).

---

## 4. 품질 체크 (서식 통과 조건)
- [ ] 커버 헤더(제목 TITLE + 부제 + 메타표)가 있는가
- [ ] 모든 제목이 named style인가(마크다운/ASCII 기호 없음)
- [ ] 비교·목록이 네이티브 표·목록인가(ASCII 표 없음)
- [ ] 표 헤더가 네이비 배경 + 흰 글씨인가
- [ ] 핵심 결론이 강조 박스(연한 네이비틴트)로 보이는가
- [ ] (Sheets) 헤더 고정·색·줄바꿈·필터가 적용됐는가
