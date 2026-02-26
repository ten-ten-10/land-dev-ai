# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working in this repository.

---

## Platform Overview — TomoAid

**My-PM-Database** is the product management repository for **TomoAid** — a multi-domain AI solutions platform. TomoAid's mission is to empower professionals (PMs, HR managers, land development analysts) with AI tools that reduce decision cycles and surface actionable insights from complex data.

The repository holds PRDs, reference materials, meeting notes, and test case data across two active product verticals. No application source code lives here.

### Brand Architecture

| Sub-product | Vertical | Status |
|---|---|---|
| **TomoHire** | HR Recruiting | Active |
| **Land Scanner（土地開發 AI）** | Land Development | Active — MVP scoped to 南台灣（高雄市優先）|

---

## PM Work Specifications (Claude 協作規範)

> These rules apply to ALL interactions in this repository.

1. **Context identification first**: Before responding, identify whether the current task belongs to the **HR / TomoHire** or **Land Development** domain. Do not mix terminology across domains.
2. **Document standard**: Every PRD must include User Story, Acceptance Criteria (AC), and UI/UX logic.
3. **Communication style**: Goal-oriented, concise. Prefer tables and bullet points over prose paragraphs.
4. **Terminology**: Preserve English terms for professional jargon — MVP, PRD, AC, ROI, FAR, Land Analysis, Technical Feasibility, User Story. Body text in Traditional Chinese.
5. **Before updating any document**: Confirm the current version and scope. If information is insufficient, list the missing items explicitly before proceeding.

---

## Project A — TomoHire（HR 招募 AI）

### Background

TomoHire addresses inefficiencies in Taiwan's talent acquisition process: job-to-candidate matching is slow, screening is manual, and hiring quality is inconsistent across teams. TomoHire automates candidate screening, match scoring, and interview workflow to help HR professionals focus on high-value judgment calls.

### Core Problem

HR teams spend the majority of evaluation time on low-signal tasks (resume parsing, initial screening calls) that can be automated — leaving less bandwidth for culture fit assessment and strategic hiring decisions.

### Target Users

| Role | Pain Point | Use Case |
|---|---|---|
| HR Manager | Volume of unqualified applicants | Automated pre-screening + ranking |
| Recruiter | Time spent on scheduling and follow-ups | Workflow automation |
| Hiring Manager | Inconsistent evaluation criteria across interviewers | Structured scoring rubrics |

### Key Concepts

- **Candidate Matching**: AI-scored relevance between JD requirements and candidate profiles
- **Screening Automation**: Rule-based + LLM hybrid filtering at top-of-funnel
- **Interview Workflow**: Structured question generation, async video review, evaluation scoring

### Repository Structure (TomoHire)

```
prds/
└── TomoHire/         # PRDs for HR recruiting features (to be created)
```

---

## Project B — Land Scanner（土地開發 AI）

### Background

Land acquisition in Taiwan is a "first come, first served" race — 買對地就贏一半 (buying the right land wins half the battle). Speed and accuracy of pre-acquisition evaluation determine competitive advantage. The current process relies heavily on experienced personnel for regulatory lookup, FAR calculation, and ROI estimation: slow, error-prone, and difficult to scale.

### Core Problem

A single land parcel evaluation requires cross-referencing cadastral data, urban planning regulations, incentive schemes, and financial models — a process that takes days and requires expertise across four stakeholder roles.

### Four-Stage Evaluation Pipeline

| Stage | Key Tasks | Automation Target |
|---|---|---|
| **篩選 Screening** | Filter by location, area, zoning; reject 畸零地 or parcels without building line access | AI auto-filter + red-flag tagging |
| **調查 Investigation** | Title search, encumbrance scan (他項權利), hidden restrictions (套繪管制, cultural assets, underground obstacles) | Query automation + risk summary |
| **評估 Evaluation** | FAR, building coverage, incentive bonuses (危老/都更/容移), massing simulation, Land Residual Value ROI | Full auto-calculation + plan comparison |
| **簽約 Signing** | Maximum bid price calculation, negotiation support | Bid ceiling output |

### Key Formulas

- **Total buildable floor area** = Site area × Base FAR × (1 + incentive coefficient + transfer coefficient) + exempt floor area
- **Land Residual Value**: `L = S − (C + M + P)`
  - S = Total sales revenue
  - C = Construction cost
  - M = Overhead / marketing
  - P = Target profit

### MVP Scope

- Priority region: **南台灣（高雄市優先，宜蘭縣、苗栗縣次之）**
- Input: land address / parcel ID → auto-lookup zoning, FAR/coverage from local urban planning ordinances
- Core flow: Cadastral positioning → Base FAR automation → Land potential assessment (incentives + 套繪風險預警) → ROI estimation

### Target Users

| Role | Chinese | Responsibility |
|---|---|---|
| Land Development Analyst | 土地開發評估人員 | Primary user — parcel screening and evaluation |
| Project Manager | 建設公司專案經理 | Evaluation meetings, report review |
| Real Estate Analyst | 不動產投資分析師 | ROI modeling and investment decisions |

### Stakeholder Ecosystem

| Role | Chinese | Function |
|---|---|---|
| Broker | 仲介 | Provides land data, initial price negotiation |
| Scrivener | 代書 | Title/ownership investigation, land administration |
| Sales Agent | 代銷 | Market research, product positioning, sale price projection |
| Architect | 建築師 | Regulatory investigation, massing layout (排厝間), floor area calculation |

### Key External Data Sources

| Dimension | Source | Purpose |
|---|---|---|
| Cadastral / Maps | 國土測繪圖資服務雲 (NLSC), 地籍圖資便民系統 | Parcel boundaries, lot ID, shape analysis |
| Zoning | 全國土地使用分區系統, city urban planning portals | Regulatory jurisdiction, setback rules |
| Transactions | 內政部實價登錄, 公開資訊觀測站 | Historical land prices, listed company activity |
| Environment / Risk | 樂居 (Leju), 國家文化資產網, 山坡地資訊網 | Nuisance facilities, cultural heritage, slope restrictions |
| Regulations | 全國法規資料庫, city development bureau ordinances | FAR caps, height limits |

### Key Reference URLs

- Land parcel map: https://maps.nlsc.gov.tw/
- Zoning query: https://ngis.tcd.gov.tw/
- National land use regulation (母法): https://law.moj.gov.tw/LawClass/LawAll.aspx?pcode=D0070012
- Building technical regulations: https://www.nlma.gov.tw/ch/legislation/regsearch/6260

### Repository Structure (Land Development AI)

```
土地開發AI/
├── ref/           # Domain reference materials (PDFs): spec, meeting notes, evaluation guides, lecture slides
├── specs/         # System specifications (to be populated)
├── assets/        # Project assets
├── testcase/      # Real land parcel test cases with site plans, area calculations, and FAR data
│   ├── 三地高雄三塊厝/
│   ├── 台南仁德崁腳北段（沒有獎勵）/
│   ├── 高雄三民區大港段七小段/
│   └── 高雄德北四段/
└── user-feedback/ # User feedback collection
```

### Engineering Repo — tomoland

工程主倉庫：`https://github.com/tomoaid/tomoland.git`

**技術棧**：Next.js (TypeScript) + FastAPI (Python) + PostgreSQL + pgvector + Celery + Redis + Docker/K8s

**文件架構**：

```
docs/
├── product/       # PM（@ten-ten-10）：prd.md, epics.md, product-brief.md, lean-canvas.md
├── architecture/  # PM（@ten-ten-10）：architecture.md, land-assessment-flow.mmd
│   └── adr/       # Tech Lead（@eric-tomoaid）：架構決策記錄
├── design/        # Designer（@cave-tomo）：ux-design-specification.md
├── specs/         # 工程師：API & 資料規格
└── research/      # PM（@ten-ten-10）：domain-research.md, PDF 參考資料
```

**團隊分工（CODEOWNERS）**：

| 路徑 | 負責人 |
|---|---|
| `docs/product/`, `docs/research/`, `docs/architecture/`, `docs/specs/` | @ten-ten-10（PM）|
| `docs/architecture/adr/` | @eric-tomoaid（Tech Lead）|
| `docs/design/`, `apps/web/` | @cave-tomo（Designer）|
| `apps/api/` | @balafish @TomoAid-Victor @jordan-tomoaid |
| `infra/` | @eric-tomoaid |

---

*Last updated: 2026-02-26*
