# 📊 TechGadget Inc. — Marketing Performance Analytics

> **A full end-to-end marketing analytics project** solving a real-world budget crisis where a 40% marketing spend increase yielded only 12% revenue growth.

![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi)
![Excel](https://img.shields.io/badge/Excel-Analysis-217346?style=for-the-badge&logo=microsoftexcel)
![Python](https://img.shields.io/badge/Python-Data%20Prep-3776AB?style=for-the-badge&logo=python)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

---

## 🚨 The Business Problem

**TechGadget Inc.** increased their marketing budget by **40%** but total revenue grew by only **12%**. The CFO is threatening to freeze the Q2 budget. The CMO needs answers — fast.

Three critical questions need to be answered:

| # | Problem | Question |
|---|---------|----------|
| 1 | **The Creative Mystery** | Why do expensive video ads drive clicks but convert terribly? |
| 2 | **The Cost Crisis** | What is the exact impact of Google Search CPCs spiking 50%? |
| 3 | **The Profitability Floor** | Which campaigns are bleeding below the 3.0× ROAS minimum? |

---

## 📁 Repository Structure

```
techgadget-marketing-analytics/
│
├── 📂 data/                          # Raw source data (CSV files)
│   ├── dim_campaigns.csv             # 5 campaigns with objectives & audiences
│   ├── dim_channels.csv              # 6 marketing channels
│   ├── dim_creatives.csv             # 7 creative assets across 4 formats
│   ├── dim_date.csv                  # Date dimension (Jan 2025 – Mar 2026)
│   ├── dim_targets.csv               # ROAS & CPA targets per campaign
│   ├── fact_marketing_performance.csv # 31,297 rows — daily spend, impressions, clicks
│   └── fact_conversions.csv          # 218,793 transactions with revenue
│
├── 📂 powerbi/
│   └── TechGadget_Marketing.pbix     # Power BI dashboard file
│
├── 📂 excel/
│   └── TechGadget_Marketing_Analysis.xlsx  # 7-sheet Excel workbook
│
├── 📂 docs/
│   └── Project_Documentation.docx   # Full project write-up
│
└── README.md
```

---

## 🗃️ Data Model (Star Schema)

This project uses a **dimensional model (star schema)** — the industry standard for analytics.

```
                    ┌─────────────────┐
                    │  dim_campaigns  │
                    │  campaign_id PK │
                    └────────┬────────┘
                             │
┌──────────────┐    ┌────────▼────────────────────────┐    ┌──────────────┐
│  dim_date    │    │   fact_marketing_performance     │    │ dim_channels │
│  date PK     ├────│   date FK                        ├────│ channel_id PK│
└──────────────┘    │   campaign_id FK                 │    └──────────────┘
                    │   channel_id FK                  │
┌──────────────┐    │   creative_id FK                 │    ┌──────────────┐
│dim_creatives ├────│   spend | impressions | clicks   │    │ dim_targets  │
│creative_id PK│    └─────────────────────────────────┘    │ campaign_id  │
└──────────────┘                                            └──────────────┘
                    ┌─────────────────────────────────┐
                    │      fact_conversions            │
                    │   date FK | campaign_id FK       │
                    │   channel_id FK                  │
                    │   revenue_generated              │
                    └─────────────────────────────────┘
```

**Key rule:** Both fact tables must be joined to all shared dimension tables for correct KPI calculation.

---

## 📐 KPI Formulas Applied

| KPI | Formula | TechGadget Result | Benchmark |
|-----|---------|-------------------|-----------|
| **CTR** (Click-Through Rate) | Clicks ÷ Impressions | 5.44% | 2–5% ✅ |
| **CVR** (Conversion Rate) | Conversions ÷ Clicks | 0.62% | 2–4% ❌ |
| **CPC** (Cost Per Click) | Spend ÷ Clicks | $0.78 blended / $3.36 NonBrand | $1–4 ⚠️ |
| **CPA** (Cost Per Acquisition) | Spend ÷ Conversions | $126.42 | $30–80 ❌ |
| **ROAS** (Return on Ad Spend) | Revenue ÷ Spend | **0.95×** | >3.0× ❌ |

---

## 🔍 Key Findings

### Finding 1 — The Creative Mystery 🎥
| Format | Spend | CTR | CVR | ROAS |
|--------|-------|-----|-----|------|
| **Video** | $7.0M | 5.5% | **0.28%** | 0.63× |
| **Image** | $4.6M | 5.6% | 0.28% | 0.63× |
| **Text** | $13.8M | 5.2% | **0.85%** | 0.69× |
| **Carousel** | $2.3M | 5.5% | 0.27% | 0.63× |

> 💡 **Video ads generate the most clicks (13M+) but convert 3× WORSE than text ads.** They attract curiosity, not purchase intent. Budget is misdirected toward high-visibility, low-efficiency creatives.

### Finding 2 — The CPC Cost Crisis 💸
- Google Search NonBrand avg CPC: **$3.36** (50% above baseline of ~$2.24)
- Result: Same budget buys **33% fewer clicks**
- Estimated additional spend wasted due to spike: **$2.28M+**
- Root cause: Competitor bidding wars driving up auction prices

### Finding 3 — The ROAS Floor Breach 🚦
| Campaign | Spend | Revenue | Actual ROAS | Target ROAS | Status |
|----------|-------|---------|-------------|-------------|--------|
| Spring Forward 2026 | $5.63M | $5.27M | 0.94× | 3.0× | 🚨 FAIL |
| Brand Awareness Q1 | $5.46M | $5.21M | 0.95× | 0.5× | ✅ PASS |
| Cart Abandonment | $5.40M | $5.19M | 0.96× | 5.0× | 🚨 FAIL |
| Summer Blowout Promo | $5.71M | $5.34M | 0.94× | 3.0× | 🚨 FAIL |
| Newsletter Reactivation | $5.46M | $5.25M | 0.96× | 3.0× | 🚨 FAIL |

> 💡 **4 out of 5 campaigns fail the 3.0× ROAS floor.** Overall ROAS is 0.95× — meaning the business loses $0.05 for every $1 spent on ads.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Python (pandas)** | Data wrangling, aggregation, KPI calculation |
| **Microsoft Excel** | 7-sheet analytical workbook with KPI engine |
| **Power BI** | Interactive executive dashboard |
| **Dimensional Modeling** | Star schema data architecture |
| **DAX** | Power BI measures and calculated columns |

---

## ⚙️ How to Run / Reproduce

### Option A — View the Power BI Dashboard
1. Download `powerbi/TechGadget_Marketing.pbix`
2. Open in **Power BI Desktop** (free download from Microsoft)
3. All data is embedded — no connection needed

### Option B — Explore the Excel Workbook
1. Download `excel/TechGadget_Marketing_Analysis.xlsx`
2. Open in Microsoft Excel or Google Sheets
3. Navigate the 7 tabs for each analysis

### Option C — Run the Python Analysis
```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/techgadget-marketing-analytics.git
cd techgadget-marketing-analytics

# Install dependencies
pip install pandas numpy openpyxl

# Run analysis
python analysis.py
```

---

## 💡 Recommendations

Based on the analysis, here are the top 3 actions TechGadget Inc. should take:

1. **Reallocate video budget to text/search ads** — Text ads convert 3× better. Moving 30% of video budget to Google Search could recover significant ROAS.
2. **Set CPC bid caps on Google NonBrand** — Cap bids to protect against competitor-driven price inflation. Use Smart Bidding with Target CPA to automate this.
3. **Pause campaigns below 1.0× ROAS** — No campaign should run below break-even. Restructure targeting and creatives before reactivating.

---

## 👤 Author

**[Your Name]**
- 💼 [LinkedIn Profile URL]
- 📧 [Your Email]

*Project completed as part of a marketing analytics case study — feedback by [Nesh Kalat](https://www.linkedin.com/in/neshkalat) on LinkedIn.*

---

## 📄 License

This project is for educational and portfolio purposes.
