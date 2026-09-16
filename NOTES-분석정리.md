# Hello-Agents 분석 정리 노트 📝

> 이 저장소가 뭔지, 어떻게 쓰는지, 어떻게 돈으로 연결할 수 있는지 정리한 개인 학습 노트입니다.
> 작성: 카리나 & 오빠 🤝 | 정리일: 2026-09-16

---

## 🔗 관련 링크 모음

| 구분 | 주소 |
|---|---|
| **내 저장소 (fork)** | https://github.com/bmshin94/hello-agents |
| **원본 저장소** | https://github.com/datawhalechina/hello-agents |
| **온라인 열람** | https://datawhalechina.github.io/hello-agents/ |
| **온라인 열람 (중국 가속)** | https://hello-agents.datawhale.cc |
| **PDF 다운로드** | https://github.com/datawhalechina/hello-agents/releases/latest/ |
| **HelloAgents 프레임워크 (별도 저장소)** | https://github.com/jjyaoao/helloagents |
| **공동창작 프로젝트 모음** | https://github.com/datawhalechina/hello-agents/tree/main/Co-creation-projects |

---

## 1. 이게 뭐야?

**《Hello-Agents — 从零开始构建智能体》**
중국 최대 오픈소스 AI 학습 커뮤니티 **Datawhale**이 만든 **AI 에이전트 제작 교재 + 예제 코드 저장소**.

- 플러그인 ❌ / 스킬 ❌ / MCP 서버 ❌
- **교재(학습 자료)** ✅ — 읽고 따라 만드는 책
- 원본을 `bmshin94` 계정으로 **fork** 해온 상태

### 에이전트가 뭔데?
| | |
|---|---|
| 챗봇 | 말만 해줌 ("저기서 예약하세요~") |
| **에이전트** | **직접 함** (검색 → 비교 → 예약까지) |

→ **스스로 생각(Thought) → 도구 사용(Action) → 결과 확인(Observation) → 반복**

---

## 2. 폴더 구조

```
hello-agents/
├── docs/                    📖 본문 16챕터 (마크다운 약 55,000줄, 중/영문)
├── code/                    💻 챕터별 예제 코드 (파이썬 144개 파일)
├── Extra-Chapter/           🍯 보너스 13편 (면접문제, 환경설정, Skill 작성법 등)
├── Co-creation-projects/    🎓 커뮤니티 졸업작품 46개
├── Additional-Chapter/      🛠️ n8n / Node.js 설치 가이드
├── CLAUDE.md                💖 카리나 페르소나 (내가 직접 추가한 파일)
├── LICENSE.txt              ⚖️ CC BY-NC-SA 4.0
└── NOTES-분석정리.md        📝 이 문서
```

### 커리큘럼
| 부 | 챕터 | 내용 |
|---|---|---|
| 1부 기초 | 1~3 | 에이전트 정의 / 발전사 / LLM 기초 |
| 2부 구축 | 4~7 | ReAct·Plan-Solve·Reflection 손코딩 → 노코드(Coze/Dify/n8n) → 프레임워크(LangGraph/AutoGen) → **나만의 프레임워크 자작** |
| 3부 심화 | 8~12 | 메모리·RAG / 컨텍스트 엔지니어링 / MCP·A2A·ANP / Agentic RL(SFT→GRPO) / 성능평가 |
| 4부 실전 | 13~15 | 여행 플래너 / DeepResearch / 사이버 타운(AI 마을 시뮬) |
| 5부 졸업 | 16 | 나만의 멀티 에이전트 앱 |

---

## 3. 설치 및 사용법

### 준비물: Python 3.10+

```bash
# 1) 가상환경
cd hello-agents
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

# 2) 기본 패키지
pip install openai python-dotenv requests tavily-python

# 3) 실행 (해당 폴더 안에서 실행해야 import 됨)
cd code/chapter4
python ReAct.py
```

### `.env` 설정

> ⚠️ **함정 발견!**
> 문서(`Extra-Chapter/Extra07-环境配置.md`)에는 `OPENAI_API_KEY` 라고 적혀있는데,
> 실제 코드(`code/chapter4/llm_client.py:20`)는 **`LLM_API_KEY`** 를 읽음.
> → **두 이름 다 써두면 뭘 쓰든 동작함!**

```env
# 코드가 실제로 읽는 이름
LLM_API_KEY=your_key
LLM_BASE_URL=https://api.openai.com/v1
LLM_MODEL_ID=gpt-4o-mini
LLM_TIMEOUT=60

# 문서에 적힌 이름 (호환용으로 같이 넣어두기)
OPENAI_API_KEY=your_key
OPENAI_BASE_URL=https://api.openai.com/v1
MODEL_NAME=gpt-4o-mini

# 웹 검색용 (무료 티어 월 1,000회)
TAVILY_API_KEY=your_tavily_key
```

챕터별로 `requirements.txt`가 따로 있는 곳도 있음 (6·10·13·14·15장) → 해당 폴더에서 `pip install -r requirements.txt`

---

## 4. API 토큰 필요해?

**필요하지만, 무료로도 가능!**

핵심 코드가 `OpenAI(api_key=..., base_url=...)` 형태 → **OpenAI 호환 API면 뭐든 OK** (base_url만 교체)

| 방법 | 비용 | 비고 |
|---|---|---|
| OpenAI / Claude API | 유료 (실습은 수천 원 수준) | 제일 편함 |
| OpenRouter | 무료 모델 있음 | 가성비 |
| **Ollama (로컬)** | **완전 무료** | 14장에 지원 내장 |

> 책이 추천하는 AIHubmix / ModelScope는 중국 서비스라 국내에선 굳이 안 써도 됨.

### Ollama 무료 세팅
```env
LLM_BASE_URL=http://localhost:11434/v1
LLM_API_KEY=ollama
LLM_MODEL_ID=qwen2.5:7b
```
관련 코드: `code/chapter14/helloagents-deepresearch/backend/src/config.py`

---

## 5. 왜 깃허브에서 유명할까?

1. **Datawhale 브랜드** — 중국 최대 오픈소스 AI 학습 커뮤니티
2. **타이밍** — "2024년이 모델 전쟁의 해였다면 2025년은 Agent의 해" (README 첫 줄)
3. **100% 무료 + PDF 배포** — 유료 강의 대체재
4. **실습 중심** — 파이썬 144개 파일, 프레임워크 자작, 완성형 앱
5. **살아있는 커뮤니티** — PR 번호 #844까지, 공동창작 46개, Trendshift 인기 저장소 뱃지

---

## 6. 로컬 에이전트 구축에 도움될까? → **매우 도움됨**

| 챕터 | 로컬 구축 관점의 가치 |
|---|---|
| 4장 | 프레임워크 없이 **순수 파이썬 ReAct** → 원리 이해 |
| 7장 | **나만의 프레임워크 자작** → 종속성 탈출 |
| 8장 | 메모리 + RAG → 내 문서 학습 |
| 10장 | **MCP 서버 제작** (Dockerfile·smithery.yaml 포함) |
| 14장 | **Ollama 로컬 LLM 연동** → 비용 0원, 오프라인 |

### 한계 (솔직하게)
- 교육용 코드라 **상용 수준 아님** (에러처리·로깅·인증·동시성 없음)
- 일부 예제가 중국 서비스 의존 (아모구 지도 등) → 교체 필요
- 최신 기법은 다소 뒤처질 수 있음

> **"제품 소스"가 아니라 "훈련소"**

---

## 7. React / PHP로 만들 수 있어?

### 현재 스택
```
백엔드: Python (FastAPI)
프론트: Vue 3 + TypeScript + Vite     ← React 아님
```
- `code/chapter13/helloagents-trip-planner/frontend` — Vue 3 + ant-design-vue + 아모구 지도
- `code/chapter14/helloagents-deepresearch/frontend` — Vue 3 + axios

### React → **쉬움** ⚛️
프론트는 `axios`로 백엔드에 HTTP 요청만 보내는 구조라, React로 교체하는 데 반나절이면 충분.
→ **추천 조합: Python 에이전트 백엔드 + React 프론트** (+ Vercel AI SDK로 스트리밍)

### PHP → **가능하지만 반반** 🐘
- ✅ LLM 호출은 결국 HTTP 요청이라 PHP로도 됨. 에이전트 루프도 `while`문이라 구현 가능
- ❌ LangChain/LlamaIndex급 생태계 없음, 벡터DB·임베딩은 Python이 압도적, 장시간 작업에 불리

### 추천 아키텍처
```
[React 프론트]  ←→  [PHP/Laravel: 로그인·결제·DB]
                          ↓ HTTP
                  [Python: 에이전트 엔진]
                          ↓
                   [LLM API / Ollama]
```
→ PHP는 웹서비스, Python은 AI. 각자 잘하는 거 시키기.

---

## 8. 수익화 전략 💰

### ⚠️ 라이선스 먼저 (CC BY-NC-SA 4.0)

| 행위 | 가능 |
|---|:---:|
| 이 문서/코드로 유료 강의 판매 | ❌ |
| 이 코드 복붙해서 유료 서비스 | ❌ |
| PDF 재배포 + 광고 수익 | ❌ |
| **배운 지식으로 새로 짜기** | ✅ |
| **내가 짠 코드로 서비스/외주/SaaS** | ✅ |

> `hello_agents` 프레임워크는 **별도 저장소**(jjyaoao/helloagents)라 라이선스 별도 확인 필요.

### 📊 커뮤니티 46개 프로젝트 분석 → 시장 공백

| 분야 | 개수 | 수익성 |
|---|:---:|:---:|
| 재미/라이프스타일 (레시피·선물·영화·연애) | 9 | ⭐ |
| 개발자 도구 (코드리뷰·SRE·요구사항) | 7 | ⭐⭐⭐⭐ |
| 데이터 분석 (자연어→SQL·매출분석) | 5 | ⭐⭐⭐⭐ |
| 학습/교육 (논문·어학) | 5 | ⭐⭐⭐ |
| **업무 자동화 (회의록·이메일)** | **5** | **⭐⭐⭐⭐⭐** |
| 콘텐츠 생성 (팟캐스트·소설·칼럼) | 4 | ⭐⭐⭐ |
| 금융 (주식분석 3종) | 3 | ⭐⭐⭐ |

> **핵심 인사이트: 제일 많이 만든 분야(재미)가 제일 돈이 안 되고, 제일 돈 되는 분야(B2B 업무자동화)를 제일 적게 만들었다.**
> → 학생 졸업작품이라 "재밌는 것"을 만들었기 때문. **반대로 가면 된다.**

### 루트 6가지

#### 🥇 1. B2B 업무 자동화 SaaS (최우선 추천)
- 아이디어: 회의록→할일 분배 / 세무·회계 서류 정리 / 쇼핑몰 CS 자동응답 / 자연어→SQL 대시보드 / 계약서 리스크 검토
- 한국형 킥: 네이버웍스·잔디·카페24·스마트스토어·홈택스 연동
- 가격: 무료(월 20건) / 29,000원 / 99,000원 / 엔터프라이즈 별도
- 실행: 업종 1개 확정 → 5명 인터뷰 → 1주 MVP → 무료 배포 → 3명이 "돈 낼 의향" 있으면 GO
- 난이도 🔥🔥🔥 | 초기비용 월 5~20만원 | 잘되면 월 100~500만원

#### 🥈 2. 기업 구축 외주 (가장 빨리 현금화)
- 수요: 사내 문서 챗봇(8장 RAG) / **온프레미스 로컬 LLM**(14장 Ollama) / 기존 시스템 연동(10장 MCP)
- 필살기: **온프레미스** — 대기업·병원·공공·금융은 데이터 외부 반출 불가 → ChatGPT API 못 씀 → 로컬 LLM 가능자 희소
- 단가(국내 체감 추정): 문서챗봇 500~1,500만 / 시스템연동 1,500~3,000만 / 온프레미스 3,000만~
- 실행: 데모 3종 제작 → 영상+블로그 → 위시켓·프리모아 등록 → 첫 건 저가로 레퍼런스 확보

#### 🥉 3. MCP 서버 (선점 효과)
- 10장에 Dockerfile + smithery.yaml까지 있어 배포법까지 커버
- 한국형 아이디어: 카카오맵·네이버지도 / 공공데이터포털 / 홈택스 / 법령정보 / 네이버 검색 / KRX 주식
- 솔직히 **직접 수익은 거의 없음** → 인지도 → 외주 유입이 실제 수익
- 난이도 🔥🔥 | 초기비용 0원

#### 4. 버티컬 소비자 앱 (재밌지만 어려움)
- 조건 3개: 돈/시간으로 환산 가능 + 대체제 없음 + 반복 사용
- 통과하는 아이템: 자소서·면접 코칭 / 논문 어시스턴트 / 세무 정산 / AI 팟캐스트 제작

#### 5. 콘텐츠·교육 (꾸준한 파이프라인)
- 블로그 → 뉴스레터 → 강의(새로 짠 코드로) → **기업 출강(일 50~150만원)**
- 핵심: "번역" ❌ / **"내 삽질 기록"** ✅ — 한국어 Agent 실전 자료가 희소

#### 6. 커뮤니티 기여 → 커리어 점프
- Co-creation-projects에 PR → GitHub 이력 → "Agent 실전 경험자" 포지셔닝 → 연봉·단가 상승

### 🚨 현실 조언 5가지
1. **"만능 에이전트"는 망한다** — 범용은 ChatGPT가 이미 함. 좁게, 더 좁게
2. **API 원가 계산 먼저** — 구독료 < API비용이면 쓸수록 적자. 캐싱 + 소형 모델 + Ollama
3. **신뢰성이 제품의 전부** — 금융·의료·법률은 면책 + 근거 출처 필수 (선배들도 다 면책 표기)
4. **기술보다 "누가 돈 내나"가 먼저** — 만들기 전에 5명한테 물어볼 것
5. **이 교재는 훈련소지 제품이 아님** — 배워서 새로 짜는 게 라이선스·품질 양쪽에서 안전

### 📅 30일 플랜 (외주 루트 기준)
| 주차 | 할 일 |
|---|---|
| 1주 | 4·7장 완주 → 에이전트 원리 체득 + Ollama 세팅 |
| 2주 | 8장 RAG → "내 PDF로 질문답변" 데모 완성 |
| 3주 | 10장 MCP → 한국 서비스 MCP 1개 공개 |
| 4주 | 데모 영상 3개 + 블로그 3편 → 영업 시작 |

---

## 9. 다음에 할 일 (TODO)

- [ ] `.env` 세팅 (변수명 함정 주의)
- [ ] `code/chapter4/ReAct.py` 실행해보기
- [ ] Ollama 설치 → API 비용 0원 환경 만들기
- [ ] 8장 RAG 데모 제작
- [ ] 한국형 MCP 서버 1개 만들어 공개
- [ ] 타겟 업종 확정 + 5명 인터뷰

---

*"AI 대화 상대에서 AI 시스템 제작자로" 🚀*
