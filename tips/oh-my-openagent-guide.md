# oh-my-openagent (OMO) — Claude Code 호환 멀티 모델 에이전트 하네스

> 한국 개발자 code-yeongyu가 만든 오픈소스 에이전트 하네스. `ultrawork` 한 단어로 Claude Opus + GPT-5 + Kimi K2를 오케스트레이션합니다. Claude Code의 hooks/skills/MCP 그대로 호환됩니다.

## 핵심 요약

- **만든 사람**: 한국 개발자 [@code-yeongyu](https://github.com/code-yeongyu) (김연규)
- **정체**: OpenCode 위에서 동작하지만 **Claude Code 완전 호환** 에이전트 하네스
- **한 단어로 시작**: `ultrawork` (또는 `ulw`)만 입력하면 모든 에이전트가 동시에 작동
- **다중 모델 오케스트레이션**: Claude Opus(메인) + Kimi K2.6 + GPT-5.4 + GLM-5.1 자동 분배
- **5만+ GitHub 스타** — Anthropic이 OpenCode를 차단한 이유로 지목된 그 프로젝트
- **차별점**: Hash-Anchored Edit (Grok Code Fast 1 성공률 6.7% → 68.3%로 상승)

## 목차

- [무엇을 해결하는가](#무엇을-해결하는가)
- [핵심 기능 한눈에](#핵심-기능-한눈에)
- [Discipline Agents — 그리스 신화 기반 에이전트 팀](#discipline-agents--그리스-신화-기반-에이전트-팀)
- [Hash-Anchored Edit — 하네스 문제 해결](#hash-anchored-edit--하네스-문제-해결)
- [Claude Code 호환성](#claude-code-호환성)
- [설치 방법](#설치-방법)
- [Claude Code와 비교](#claude-code와-비교)
- [언제 OMO를 쓰면 좋은가](#언제-omo를-쓰면-좋은가)

---

## 무엇을 해결하는가

Claude Code, Codex, Cursor, OpenCode를 매번 갈아타며 워크플로우를 디버깅하는 피로감을 해소합니다.

> *"You're juggling Claude Code, Codex, and random OSS models. We did the work. Tested everything. Install oh-my-openagent. Type `ultrawork`. Done."*

| 기존 워크플로우 | OMO 워크플로우 |
|----------------|----------------|
| 모델별로 도구 갈아타기 | 모델 자동 분배 (작업 종류로 매핑) |
| 수동 워크플로우 설정 | 카테고리만 선택, 모델은 하네스가 결정 |
| 컨텍스트 자주 터짐 | Skill-Embedded MCP로 컨텍스트 절약 |
| 에이전트 멈추면 직접 재개 | Todo Enforcer가 자동 재개 |

---

## 핵심 기능 한눈에

| 기능 | 설명 |
|:----:|------|
| 🪄 **`ultrawork` / `ulw`** | 한 단어로 모든 에이전트 활성화 |
| 🤖 **Discipline Agents** | Sisyphus, Hephaestus, Prometheus, Oracle, Librarian, Explore — 병렬 실행 |
| 🚪 **IntentGate** | 진짜 의도를 분석 후 분류/실행 (문자 그대로 해석 방지) |
| 🔗 **Hash-Anchored Edit** | `LINE#ID` 컨텐츠 해시 검증으로 stale-line 에러 0% |
| 🛠️ **LSP + AST-Grep** | 워크스페이스 rename, AST 기반 리라이트 (25개 언어) |
| 🧠 **Background Agents** | 5+ 전문가 병렬 실행, 컨텍스트는 깨끗하게 유지 |
| 📚 **Built-in MCPs** | Exa(웹 검색) + Context7(공식 문서) + Grep.app(GitHub 검색) |
| 🔁 **Ralph Loop** | 100% 끝날 때까지 자기 참조 루프 |
| ✅ **Todo Enforcer** | 에이전트가 멈추면 강제로 재개 |
| 💬 **Comment Checker** | AI 슬롭 주석 자동 차단 |
| 🖥️ **Tmux 통합** | REPL, 디버거, TUI 앱 모두 인터랙티브 |
| 🔌 **Claude Code 호환** | hooks/commands/skills/MCPs/plugins 그대로 사용 |
| 🎯 **Skill-Embedded MCPs** | 스킬마다 자체 MCP 서버 동봉, 컨텍스트 절약 |
| 📋 **Prometheus Planner** | 인터뷰 모드로 사전 계획 수립 |
| 🔍 **`/init-deep`** | 폴더별 `AGENTS.md` 자동 생성 |

---

## Discipline Agents — 그리스 신화 기반 에이전트 팀

OMO의 시그니처. 그리스 신화 캐릭터로 역할을 명확히 분리한 에이전트 팀입니다.

| 에이전트 | 역할 | 사용 모델 |
|---------|------|-----------|
| **Sisyphus** (시지프스) | **메인 오케스트레이터** — 계획 + 위임 + 끝까지 추진 | Claude Opus 4.7 / Kimi K2.6 / GLM-5.1 |
| **Hephaestus** (헤파이스토스) | **자율 심층 작업자** — 목표만 주면 끝까지 실행 | GPT-5.4 |
| **Prometheus** (프로메테우스) | **전략 계획가** — 인터뷰 모드로 사전 계획 | Claude Opus 4.7 / Kimi K2.6 / GLM-5.1 |
| **Oracle** | 의사결정 어드바이저 | (모델 자동 선택) |
| **Librarian** | 문서/지식 관리 | (모델 자동 선택) |
| **Explore** | 코드베이스 탐색 | (모델 자동 선택) |

### 모델 선택은 카테고리로 (수동 X)

Sisyphus가 서브에이전트에게 위임할 때 **모델을 직접 고르지 않고 카테고리만 선택**합니다. 하네스가 자동으로 매핑합니다.

| 카테고리 | 용도 | 자동 매핑 |
|---------|------|-----------|
| `visual-engineering` | 프론트엔드, UI/UX, 디자인 | (시각 작업에 강한 모델) |
| `deep` | 자율적 리서치 + 실행 | GPT-5.4 등 |
| `quick` | 단일 파일 변경, 오타 | (가벼운 모델) |
| `ultrabrain` | 어려운 로직, 아키텍처 결정 | GPT-5.4 xhigh |

> 사용자는 손대지 않습니다. 에이전트가 작업 종류만 말하면 하네스가 모델을 정합니다.

---

## Hash-Anchored Edit — 하네스 문제 해결

OMO의 **가장 기술적으로 인상적인 기능**입니다. Can Bölük의 [The Harness Problem](https://blog.can.ac/2026/02/12/the-harness-problem/) 글에서 출발했습니다.

### 문제: 에이전트 실패는 모델 잘못이 아니다

> *"None of these tools give the model a stable, verifiable identifier for the lines it wants to change. They rely on the model reproducing content it already saw. When it can't, the user blames the model."*

대부분의 편집 도구는 모델이 **이전에 본 내용을 재현**하기를 요구합니다. 공백 한 칸 틀리면 실패합니다.

### 해결: Hashline — 모든 줄에 컨텐츠 해시 부착

```
11#VK| function hello() {
22#XJ|   return "world";
33#MB| }
```

에이전트는 `LINE#ID` 태그를 참조해서 편집합니다. 파일이 변경됐으면 해시가 안 맞아서 **편집 자체가 거부**됩니다 (corruption 발생 전 차단).

### 효과

| 지표 | Before | After |
|------|--------|-------|
| Grok Code Fast 1 성공률 | 6.7% | **68.3%** |
| stale-line 에러 | 자주 발생 | **0%** |
| 공백 재현 실패 | 빈번 | 없음 |

> 영감 출처: [oh-my-pi](https://github.com/can1357/oh-my-pi)

---

## Claude Code 호환성

OMO는 OpenCode 플러그인이지만 **Claude Code의 자산을 그대로 재사용**할 수 있습니다.

| Claude Code 자산 | OMO에서 동작? |
|------------------|---------------|
| Hooks | ✅ 그대로 |
| Commands (슬래시 명령어) | ✅ 그대로 |
| Skills | ✅ 그대로 |
| MCP 서버 | ✅ 그대로 |
| Plugins | ✅ 그대로 |
| `CLAUDE.md` / `AGENTS.md` | ✅ 그대로 (`/init-deep`이 자동 생성) |

이미 Claude Code 환경을 다듬어 놓았다면, OMO 설치만 하면 그대로 적용됩니다.

---

## 설치 방법

### 사람을 위한 설치 (게으른 방법)

OMO 공식 README가 권장하는 방식입니다.

```
Install and configure oh-my-openagent by following the instructions here:
https://raw.githubusercontent.com/code-yeongyu/oh-my-openagent/refs/heads/dev/docs/guide/installation.md
```

위 프롬프트를 Claude Code, AmpCode, Cursor 등에 그대로 붙여넣으면 에이전트가 알아서 설치합니다.

> *"Let an agent do it. Humans fat-finger configs."* (사람은 설정 파일 오타 냅니다)

### LLM 에이전트를 위한 설치

```bash
curl -s https://raw.githubusercontent.com/code-yeongyu/oh-my-openagent/refs/heads/dev/docs/guide/installation.md
```

위 명령으로 가이드를 받아 따라가면 됩니다.

### 추천 구독 (저렴하게 풀파워)

OMO 공식 추천 (제휴 아님, 개인 추천):

| 구독 | 월 비용 |
|------|---------|
| ChatGPT Plus | $20 |
| Kimi Code | $19 |
| GLM Coding Plan | $10 |
| **합계** | **약 $49** |

> Claude Code Max ($200)의 1/4 가격으로 풀파워가 가능하다는 주장입니다. Kimi K2.6 + GPT-5.4 조합만으로도 바닐라 Claude Code 이상의 결과를 낸다고 명시합니다.

---

## Claude Code와 비교

| 항목 | Claude Code | oh-my-openagent (OMO) |
|------|-------------|------------------------|
| 모델 | Claude 단일 | Claude + GPT-5 + Kimi + GLM |
| 가격 | $200/월(Max) | $49/월 (위 구독 합계) |
| 에이전트 자동 분배 | 수동 | 카테고리 자동 매핑 |
| 편집 도구 | string match | **Hash-Anchored** |
| 워크플로우 시작 | 사용자가 지시 | `ultrawork` 한 단어 |
| 자기 수정 루프 | 부분 지원 | **Ralph Loop** (100% 까지) |
| Hooks/Skills/MCP | 자체 생태계 | **Claude Code 호환** + 자체 추가 |
| 락인 | Anthropic | 멀티 프로바이더 |

> 주의: OMO는 **Claude Code 대체재가 아니라 보완재**로 봐도 됩니다. Claude Code 자산을 그대로 쓸 수 있어 마이그레이션 비용이 거의 없습니다.

---

## 언제 OMO를 쓰면 좋은가

### ✅ 적합한 상황

- **여러 모델을 섞어 쓰고 싶은 경우** — Claude만으로 부족하거나 비용이 부담될 때
- **장기 자율 작업** — Ralph Loop로 끝까지 완료시키고 싶은 야간 빌드 작업
- **대규모 리팩터링** — Hash-Anchored Edit으로 stale-line 에러 없이 안전하게
- **Claude Code 자산이 이미 많은 경우** — 설정 마이그레이션 비용 거의 없음
- **에이전트 비용 최적화** — `quick`은 가벼운 모델, `ultrabrain`은 강한 모델로 자동 분배

### ⚠️ 부적합한 상황

- **단순한 1회성 작업** — `ultrawork` 같은 풀 오케스트레이션이 오버킬
- **회사 보안상 Anthropic만 허용** — OMO의 핵심은 멀티 프로바이더
- **OpenCode 자체에 익숙하지 않은 경우** — 학습 곡선이 살짝 있음 (`ultrawork`만 쓰면 작긴 함)

---

## 인용된 사용자 후기 일부

> "It made me cancel my Cursor subscription. Unbelievable things are happening in the open source community." — Arthur Guiot
> "If Claude Code does in 7 days what a human does in 3 months, Sisyphus does it in 1 hour." — B, Quant Researcher
> "Knocked out 8000 eslint warnings with Oh My Opencode, just in a day" — Jacob Ferrari
> "I converted a 45k line Tauri app into a SaaS web app overnight using Ohmyopencode and ralph loop." — James Hargis

---

## 한국 개발자가 만든 도구라는 점

OMO는 한국 개발자 [@code-yeongyu](https://github.com/code-yeongyu)(김연규)가 만들었습니다. README는 영어/한국어/일본어/중국어 4개 언어를 지원하고, 디스코드에서 "Building in Public" 형식으로 실시간 개발이 진행됩니다.

또한 본인이 운영하는 [Sisyphus Labs](https://sisyphuslabs.ai)에서 Jobdori라는 AI 어시스턴트(OmO 기반)로 OMO 자체를 유지보수한다고 명시되어 있습니다. **AI가 AI 도구를 만드는** 메타적 구조입니다.

---

## 우리 프로젝트(claude-toolkit)와의 관계

| claude-toolkit | OMO |
|----------------|-----|
| Claude Code 단일 환경의 베스트 프랙티스 | 멀티 모델 오케스트레이션 |
| 가이드/스킬/에이전트 모음 | 즉시 설치되는 하네스 |
| 학습/축적용 | 즉시 사용용 |

OMO를 설치한 뒤에도 claude-toolkit의 **CLAUDE.md 템플릿, hooks-guide, skills 등을 그대로 사용 가능**합니다. OMO가 Claude Code 자산을 그대로 받기 때문입니다.

---

**출처**:
- [GitHub: code-yeongyu/oh-my-openagent](https://github.com/code-yeongyu/oh-my-openagent)
- [공식 설치 가이드](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/guide/installation.md)
- [The Harness Problem (Can Bölük)](https://blog.can.ac/2026/02/12/the-harness-problem/) — Hashline 영감 출처
- [oh-my-pi](https://github.com/can1357/oh-my-pi) — Hashline 원전
- [Sisyphus Labs](https://sisyphuslabs.ai) — 메인테이너의 회사

**작성일**: 2026-05-07
**태그**: ClaudeCode, OpenCode, OMO, oh-my-openagent, 에이전트하네스, 멀티모델, 오픈소스, KimiK2, GPT5, ClaudeOpus
