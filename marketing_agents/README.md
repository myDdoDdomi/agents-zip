# marketing-agents 📣

> 제품·서비스의 **대외 마케팅 콘텐츠와 캠페인**(블로그·랜딩카피·소셜·SEO·이메일·포지셔닝)을
> 시장 리서치에 근거해 **기획·작성·검수**하는 7명의 전문 에이전트 팀입니다.

이 팀은 **모델 A(자기완결형)** — 이 폴더를 Claude Code로 **직접 열어** 슬래시 스킬과 템플릿을
자동 로드해 씁니다. 외부 MCP 없이 빌트인 웹 검색만으로 동작합니다.

---

## 🚀 빠른 시작

1. **이 폴더를 Claude Code로 엽니다.**
2. (1회) `docs/BRAND-VOICE.md`에 브랜드 보이스·톤·금칙어를 채웁니다.
3. 한국어로 시킵니다. 빗금(`/`)으로 스킬을 고를 수 있습니다.
   ```
   신제품 런칭 캠페인 브리프 만들어줘
   ```
   → 브리프 → 리서치 → 콘텐츠/카피/SEO/소셜 작성 → 검수 순으로 진행합니다.

---

## 👥 팀 구성 (8 에이전트)

| 에이전트 | 역할 | model |
|---|---|---|
| [`marketing-lead`](.claude/agents/marketing-lead.md) | 전략·캠페인·포지셔닝·라우팅·게이트(오케스트레이터) | opus |
| [`market-researcher`](.claude/agents/market-researcher.md) | 시장·경쟁·오디언스 리서치(근거), read-only | opus |
| [`content-writer`](.claude/agents/content-writer.md) | 블로그·롱폼 콘텐츠 | sonnet |
| [`copywriter`](.claude/agents/copywriter.md) | 랜딩·광고·이메일 카피 (+EN 옵션) | sonnet |
| [`seo-specialist`](.claude/agents/seo-specialist.md) | 키워드·SEO 브리프·온페이지 | sonnet |
| [`social-media-strategist`](.claude/agents/social-media-strategist.md) | 소셜·숏폼 캘린더·스크립트 | sonnet |
| [`brand-voice-reviewer`](.claude/agents/brand-voice-reviewer.md) | 브랜드 보이스·사실·규정 검수, read-only | opus |
| [`feedback-reporter`](.claude/agents/feedback-reporter.md) | 팀 작업 회고→본부 피드백 보고서(read 중심) | sonnet |

## 🧩 슬래시 스킬 (10)

| 스킬 | 담당 | 산출물 |
|---|---|---|
| `/campaign-brief` | marketing-lead | 캠페인 브리프 |
| `/positioning` | marketing-lead | 포지셔닝·메시징 |
| `/market-research` | market-researcher | 시장 리서치 |
| `/blog-post` | content-writer | 블로그·롱폼 |
| `/landing-copy` | copywriter | 랜딩·광고 카피 |
| `/email-sequence` | copywriter | 이메일 시퀀스 |
| `/seo-brief` | seo-specialist | SEO 브리프 |
| `/social-calendar` | social-media-strategist | 소셜 캘린더 |
| `/copy-review` | brand-voice-reviewer | 브랜드·사실 검수 |
| `/feedback-agents` | feedback-reporter | 팀 작업 회고·피드백 보고서(→ 본부) |

---

## 🚧 다른 팀과의 경계 (핸드오프)

| 필요 | 핸드오프 |
|---|---|
| 배너·키비주얼·디자인 시스템 등 **비주얼 제작** | `design_agents` |
| 메타태그·스키마·속도 등 **기술 SEO 구현** | `dev_agents` |
| 기획서·기능명세 등 **제품/개발 문서** | `docs_agents` |

> 마케팅은 *말(컨셉·카피·메시지)*을 만들고, *비주얼·코드·제품문서*는 해당 팀으로 넘깁니다.

---

## 🧭 원칙 (요약)
- **한국어 기본**(랜딩·이메일·소셜은 요청 시 **영문 병행**).
- **브랜드 보이스 절대 기준**(`docs/BRAND-VOICE.md`), **과장·허위·미검증 효능 금지**.
- 모든 산출물은 `brand-voice-reviewer` **검수 게이트**.
- **외부 발행·광고 집행은 사람 승인**(이 팀은 콘텐츠를 만들고, 게시는 사람이).

---

## 📂 폴더 구조

```
marketing-agents/
├── CLAUDE.md            # 그룹장(오케스트레이터)
├── .claude/
│   ├── agents/          # 마케팅 에이전트 8종
│   └── skills/          # 슬래시 스킬 10종
├── templates/           # 산출물 템플릿 8종
├── feedback/            # 팀 작업 회고·피드백 보고서 (→ 본부) /feedback-agents
└── docs/                # 브랜드 보이스 · 워크플로우 · 채널 가이드
```

---
🤖 Built with [Claude Code](https://claude.com/claude-code)

## 🧠 팀 메모리 (MEMORY.md) 사용법

이 팀은 두 층의 메모리를 쓴다.
- **CLAUDE.md** — 사람이 쓰는 안정적 규칙·라우팅(매 세션 전체 로드).
- **MEMORY.md** — 작업하며 쌓이는 학습(검증된 함정·우회법·환경 매핑·반복 교정). CLAUDE.md가 `@./MEMORY.md`로 자동 로드한다.

| 항목 | 내용 |
|---|---|
| **언제 적나** | 같은 실수 2회 · 환경/도구 특이사항 · 매 세션 다시 설명하게 되는 것 · 검수가 잡아준 것 |
| **어떻게** | 해당 섹션에 **한 줄**(가능하면 날짜·근거). 규칙으로 굳으면 CLAUDE.md로 승격 |
| **확인** | 세션 중 `/memory`로 로드된 메모리 파일 확인 |
| **용량** | 200줄 이하 유지. 초과 시 `memory/<주제>.md`로 분리(필요할 때만 읽힘) |
| **개인 메모리** | 개인·머신 한정 학습은 Claude Code 자동 메모리(`~/.claude/projects/.../memory/`)가 별도 관리(레포 비커밋). 팀 공유 학습은 MEMORY.md |
