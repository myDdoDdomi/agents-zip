---
name: firmware-spec
description: 임베디드 펌웨어를 설계하고 참조 구현·프로토타입까지 만듭니다. "펌웨어 설계", "RTOS/베어메탈", "드라이버/HAL", "전력 관리", "부트로더/OTA", "펌웨어 참조 구현" 요청 시 사용. 사용법 /firmware-spec [SoC/기능]
---

# /firmware-spec — 펌웨어 설계 · 참조 구현

`firmware-embedded-engineer` 에이전트를 사용해 **펌웨어 설계서와 참조 구현**을 만듭니다.

## 절차
1. `templates/펌웨어설계서.md` 구조를 확인한다.
2. 요구·제약(전력·메모리·실시간성)과 타깃 SoC/MCU를 확인한다(없으면 질문).
3. SoC/RTOS/SDK 사양은 **Context7 MCP로 최신 문서 조회**(없으면 `(확인 필요)`).
4. `firmware-embedded-engineer`에게 위임해 펌웨어 구조(RTOS/베어메탈)·메모리맵·전력 관리·드라이버/HAL·
   부트/OTA를 설계하고, 필요하면 **참조 구현 골격 코드**를 작성한다.
5. OTA·부트 무결성·서명은 `device-security-engineer`와 협의한다.

## 주의
- 코드는 **참조/프로토타입 범위**. 양산 구현·정식 코드리뷰는 `dev_agents` 핸드오프/조립(CLAUDE.md §2·§7).
- **파일 쓰기는 사람 승인** 게이트(Human-in-the-loop).

인자(`$ARGUMENTS`)가 있으면 대상으로 사용하고, 없으면 먼저 묻는다.
