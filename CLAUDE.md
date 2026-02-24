# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**My-PM-Database** is a project management repository for **土地開發 AI 補助決策系統** (Land Development AI Decision Support System). The system helps Taiwan construction companies rapidly evaluate land parcels for acquisition by automating cadastral identification, regulatory lookup, incentive calculation, and ROI estimation.

The repository currently holds project specifications, reference materials, and test case data — no application source code has been written yet.

## Domain Context

The system addresses a core industry problem: land acquisition in Taiwan is a "first come, first served" race where speed and accuracy of evaluation determine competitive advantage. The motto is "買對地就贏一半" (buying the right land wins half the battle).

### Four-Stage Evaluation Pipeline

1. **篩選 Screening** — Filter land parcels by location, area, zoning; auto-reject irregular shapes (畸零地) or parcels without building line access
2. **調查 Investigation** — Due diligence: title search, encumbrance scan (他項權利), hidden restrictions (套繪管制, cultural assets, underground obstacles)
3. **評估 Evaluation** — Calculate floor area ratio (容積率), building coverage (建蔽率), incentive bonuses (危老/都更/容移), massing simulation, and Land Residual Value Method ROI
4. **簽約 Signing** — Price negotiation support with computed maximum bid price

### Key Formulas

- **Total buildable floor area** = Site area × Base FAR × (1 + incentive coefficient + transfer coefficient) + exempt floor area
- **Land Residual Value**: L = S − (C + M + P) where S=total sales, C=construction cost, M=overhead, P=target profit

### MVP Scope (per 2026/02/13 meeting)

- Priority region: **Tainan (台南市)**
- Input: land address → auto-lookup parcel ID, zoning, FAR/coverage from 《都市計畫法臺南市施行細則》
- Four-step flow: Cadastral positioning → Base FAR automation → Land potential assessment (incentives) → ROI estimation

## Repository Structure

```
土地開發AI/
├── ref/           # Domain reference materials (PDFs): spec document, meeting notes, evaluation guides, lecture slides
├── specs/         # System specifications (to be populated)
├── assets/        # Project assets
├── testcase/      # Real land parcel test cases with site plans, area calculations, and FAR data
│   ├── 三地高雄三塊厝/
│   ├── 台南仁德崁腳北段（沒有獎勵）/
│   ├── 高雄三民區大港段七小段/
│   └── 高雄德北四段/
└── user-feedback/ # User feedback collection
```

## Key External Data Sources

| Dimension | Source | Purpose |
|-----------|--------|---------|
| Cadastral/Maps | 國土測繪圖資服務雲 (NLSC), 地籍圖資便民系統 | Parcel boundaries, lot ID, shape analysis |
| Zoning | 全國土地使用分區系統, city urban planning portals | Regulatory jurisdiction, setback rules |
| Transactions | 內政部實價登錄, 公開資訊觀測站 | Historical land prices, listed company activity |
| Environment/Risk | 樂居 (Leju), 國家文化資產網, 山坡地資訊網 | Nuisance facilities, cultural heritage, slope restrictions |
| Regulations | 全國法規資料庫, city development bureau ordinances | FAR caps, height limits |

## Key Reference URLs (from kickoff)

- Land parcel map: https://maps.nlsc.gov.tw/
- Zoning query: https://ngis.tcd.gov.tw/
- National land use regulation (母法): https://law.moj.gov.tw/LawClass/LawAll.aspx?pcode=D0070012
- Building technical regulations: https://www.nlma.gov.tw/ch/legislation/regsearch/6260

## Stakeholder Roles

- **仲介 (Broker)**: Provides land data, initial price negotiation
- **代書 (Scrivener)**: Title/ownership investigation, land administration
- **代銷 (Sales Agent)**: Market research, product positioning, projected sale price
- **建築師 (Architect)**: Regulatory investigation, massing layout (排厝間), floor area calculation
