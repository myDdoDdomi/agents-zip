# QA Agents Team 🧪

전문 QA 조직을 그대로 옮긴 **Claude Code 서브에이전트(subagent) 모음**입니다.
테스트 전략 수립부터 케이스 설계, 자동화, API·성능·보안·접근성·모바일 검증, 회귀, 버그 트리아지까지 — 실제 대형 IT 기업의 QA 풀세트를 **13명의 전문 에이전트**(작업 회고·본부 피드백 보고용 `feedback-reporter` 포함)로 구성했습니다.

각 에이전트는 자기 전문 영역의 책임·워크플로우·산출물 양식·품질 원칙을 갖고 있으며, 루트 [`CLAUDE.md`](CLAUDE.md)의 **QA 그룹장**과 [`qa-lead`](agents/qa-lead.md) 에이전트가 **오케스트레이터**로서 작업을 분배·종합합니다.

> 🧭 **2026 QA 패러다임 정렬**: QA 리더 = 오케스트레이터, 리스크 기반 테스트 선정, 자율 회귀, 셀프힐링 자동화, AI 생성 코드 검증, Human-in-the-loop 거버넌스를 설계에 반영했습니다.

> 🔬 **v1.1 개선 (실전 피드백 반영)**: 실제 프로젝트 1사이클 운영 결과 "발견은 강하나 검증이 약하다"는 교훈을 반영했습니다. 전 에이전트에 **검증 규율**(Sev1/2 실행 재현 의무·싼 단언 실행 증명·phantom-read 가드·전제 자가정정·출력 압축)을 심고, 역할별로 **자가검증 Exit(자동화)·fix-verify 루프 소유(회귀)·재검증 게이트(트리아지)·심각도 루브릭(리드)**를 추가했습니다. 이 규율은 각 `agents/*.md` 안에 직접 박혀 있어 복사해도 따라갑니다.

---

## 👥 팀 구성

| 에이전트 | 역할 | 주 사용 시점 |
|----------|------|--------------|
| [`qa-lead`](agents/qa-lead.md) | QA 총괄 / 테스트 전략 리드 (오케스트레이터) | 기능·릴리스·스프린트 시작, 전략·우선순위·작업 분배 |
| [`test-case-designer`](agents/test-case-designer.md) | 테스트 케이스 설계 | 요구사항→케이스 설계, 설계 기법 적용 |
| [`manual-functional-tester`](agents/manual-functional-tester.md) | 기능 / 수동 검증 | 케이스 실행, 스모크/새너티/수용 테스트 |
| [`automation-engineer`](agents/automation-engineer.md) | 테스트 자동화 (SDET) | E2E/통합/단위 자동화, 플레이키 안정화 |
| [`api-test-engineer`](agents/api-test-engineer.md) | API 테스트 | REST/GraphQL 계약·동작·오류·인증 검증 |
| [`performance-test-engineer`](agents/performance-test-engineer.md) | 성능 / 부하 테스트 | 부하·스트레스·내구, 병목 분석 |
| [`security-test-engineer`](agents/security-test-engineer.md) | 보안 테스트 (방어적) | OWASP 점검, 취약점·시크릿·의존성 검사 |
| [`accessibility-tester`](agents/accessibility-tester.md) | 접근성 (a11y) | WCAG, 키보드/스크린리더/색 대비 |
| [`mobile-qa-engineer`](agents/mobile-qa-engineer.md) | 모바일 QA | 기기 파편화, 생명주기, 권한, 네트워크 |
| [`exploratory-tester`](agents/exploratory-tester.md) | 탐색적 테스트 | 엣지케이스 사냥, 자유 탐색 |
| [`regression-test-engineer`](agents/regression-test-engineer.md) | 회귀 테스트 | 변경 영향 분석, 회귀 스위트, 사이드이펙트 |
| [`bug-triager`](agents/bug-triager.md) | 버그 리포팅 / 트리아지 | 결함 정리, 심각도·우선순위, 중복 판정 |
| [`feedback-reporter`](agents/feedback-reporter.md) | 팀 작업 회고 → 본부 피드백 보고 | 작업 종료 후 회고, 팀 개선점 정리(read 중심) |

> 지휘: 루트 [`CLAUDE.md`](CLAUDE.md) = **QA 그룹장(오케스트레이터)** — 팀 라우팅·품질 게이트·거버넌스를 총괄.

---

## 🚀 설치 / 사용 방법

이 폴더의 `agents/*.md` 파일은 [Claude Code 서브에이전트](https://code.claude.com/docs/en/sub-agents) 형식입니다.
루트 [`CLAUDE.md`](CLAUDE.md)는 QA 그룹장(오케스트레이터) 지침으로, 이 리포에서 작업할 때 자동 로드됩니다.

### 1) 프로젝트에 추가 (팀 공유 권장)
리포지토리를 클론한 뒤, `agents/` 안의 파일을 사용하려는 프로젝트의 `.claude/agents/`로 복사합니다.

```bash
# 예시
git clone <this-repo-url>
mkdir -p your-project/.claude/agents
cp QA_agents/agents/*.md your-project/.claude/agents/
```

`/feedback-agents` 스킬도 쓰려면 `skills/feedback-agents/`를 대상 프로젝트의 `.claude/skills/`로 복사합니다.

```bash
mkdir -p your-project/.claude/skills
cp -r QA_agents/skills/feedback-agents your-project/.claude/skills/
```

### 2) 개인 전역(global)으로 추가
모든 프로젝트에서 쓰려면 사용자 홈의 `~/.claude/agents/`(Windows: `%USERPROFILE%\.claude\agents\`)로 복사합니다.

### 3) 호출
Claude Code에서 자동으로 적절한 에이전트가 위임되며, 직접 지정도 가능합니다.

```
> qa-lead 에이전트로 이번 결제 기능 변경에 대한 테스트 전략을 세워줘
> api-test-engineer 로 /orders 엔드포인트를 검증해줘
```

> 💡 보통은 **`qa-lead`에게 먼저 맡기면** 리스크를 분석해 적절한 전문 에이전트들에게 작업을 분배합니다.

> 🔁 **작업이 끝나면 회고:** `/feedback-agents`(또는 "feedback-reporter로 이번 QA 작업 회고해줘")로
> 이 팀의 작업을 돌아보고, 본부(team-architect)에 전달할 `feedback.md`를 **대상 프로젝트의 `feedback/`**에
> 산출합니다. best-effort 회고(git·세션·산출물 흔적 기반)이며, 본부가 QA 팀 정의·문서를 바로 개선하도록 업데이트 대상을 적습니다.

---

## 🔄 추천 협업 흐름

```
요구사항/변경
   │
   ▼
[qa-lead] ── 전략 수립 · 리스크 평가 · 작업 분배
   │
   ├──▶ [test-case-designer] ── 케이스 설계
   │           │
   │           ▼
   ├──▶ [manual-functional-tester] / [automation-engineer] ── 실행·자동화
   ├──▶ [api-test-engineer] [performance-...] [security-...] [accessibility-...] [mobile-...]
   ├──▶ [exploratory-tester] ── 추가 위험 탐색
   ├──▶ [regression-test-engineer] ── 회귀 검증
   │           │
   │           ▼  (발견된 결함)
   └──▶ [bug-triager] ── 결함 리포트·트리아지
   │
   ▼
[qa-lead] ── 결과 종합 · Exit Criteria로 릴리스 판정
```

---

## 🛠 커스터마이징

- 각 `.md`의 **frontmatter**(`name`, `description`, `tools`, `model`)와 본문 시스템 프롬프트를 자유롭게 수정해 팀 컨벤션에 맞추세요.
- `tools`는 해당 에이전트에 허용할 도구를 제한합니다(최소 권한 원칙). 빼면 전체 상속. (예: 보안/접근성은 read-only)
- `model`은 작업 난이도에 맞춰 조정합니다. 본 팀은 **복잡 추론·분석·전략** 역할(`qa-lead`, `performance`, `security`, `exploratory`)만 `opus`, 나머지는 비용 최적화를 위해 `sonnet`으로 배분했습니다. `inherit`로 메인 세션 모델을 따르게 할 수도 있습니다.
- `description`은 자동 위임의 **라우팅 신호**입니다. 트리거 상황을 구체적으로 쓰고, 자동 호출을 원하면 "적극적으로(PROACTIVELY)" 같은 문구를 넣으세요.
- 스택 전용 에이전트(예: `playwright-specialist`, `k6-load-tester`)를 추가해 확장할 수 있습니다.
- 각 에이전트의 **`## 🔬 검증 규율 (실전 피드백 v1.1)`** 블록은 실전 운영 교훈을 압축한 핵심 가드레일입니다. 모델 B(복사-라이브러리) 특성상 `CLAUDE.md`는 대상 프로젝트에 따라가지 않으므로, 이 규율은 의도적으로 각 `agents/*.md`에 직접 심어 두었습니다 — 삭제하지 마세요.

---

## 📜 라이선스 / 기여

내부/외부 팀이 자유롭게 가져다 쓰고 개선할 수 있도록 공유합니다.
개선 사항은 PR로 기여해 주세요. (각 에이전트의 산출물 양식·체크리스트 보강 환영)
