---
name: prototype
description: 프로토타입 시나리오·화면 흐름·인터랙션을 정리합니다. "프로토타입", "화면 흐름", "유저플로우", "인터랙션" 요청 시 사용. 사용법 /prototype [과업/기능명]
---

# /prototype — 프로토타입 시나리오

`ideation-prototyper` 에이전트를 사용해 **프로토타입 시나리오**를 만듭니다.

## 절차
1. `templates/프로토타입시나리오.md` 구조를 확인한다.
2. (있으면) 컨셉(`/ideate` 결과)·IA를 참조한다.
3. `ideation-prototyper`에게 위임해 화면 흐름(Mermaid flowchart), 주요 인터랙션,
   상태(빈/로딩/에러/성공)를 서술한다.
4. 완료 후 안내: `/a11y-review`(접근성·UX 검토), Figma 생성이 필요하면 `/figma-sync`.

인자(`$ARGUMENTS`)가 있으면 과업으로 사용하고, 없으면 먼저 묻는다.
