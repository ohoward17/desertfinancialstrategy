# DFCU ALM Command Center

A concept asset/liability management (ALM) dashboard for Desert Financial Credit Union, paired with a creative brief. The brief sets the strategic goal; the dashboard shows what that goal is worth on the balance sheet and whether the credit union can afford it.

**Live demo:** https://YOUR-USERNAME.github.io/dfcu-alm-command-center/

> Hypothetical concept built from public data. Not affiliated with or endorsed by Desert Financial Credit Union.

![Executive dashboard](docs/screenshot-dashboard.png)

## What's inside

| Section | What it shows |
|---|---|
| **Executive Dashboard** | KPIs as of June 30, 2026 (assets, NIM, net worth, efficiency, ROA, liquidity), trends since 2021, balance sheet mix, loan and share composition |
| **ALM & Risk** | Repricing gap, Year 1 and Year 2 NII sensitivity (-300 to +300bp) against board limits, NEV and the NCUA NEV Supervisory Test, full model assumption register |
| **Financial Documents** | June 30, 2026 balance sheet, FY2025 vs. FY2024 audited income statement, key ratios vs. peers, liquidity and contingent funding |
| **Strategy** | The creative brief connected to the balance sheet: what a "main bank" member is worth, an interactive Give & Grow capacity test against the ROA floor and capital target, the three creative thought starters sized as funded initiatives, and the 2026–2030 plan |
| **AI Analyst** | Scripted Q&A grounded in the model's numbers |

![Strategy tab](docs/screenshot-strategy.png)

## The idea

The [creative brief](docs/creative-brief.md) asks how Desert Financial can get Arizonans who bank with big national banks to make it their main bank: *"Make Arizona better in ways members can see and feel."*

The Strategy tab answers the CFO's side of that question:

- **What primacy is worth.** Checking costs 0.06% with a rate beta of 0.02, worth about $120 a year in cheaper funding and about $781 of franchise value per account.
- **What the balance sheet can afford.** Plan ROA has about $24M of room above the 0.95% floor in its tightest year.
- **What it takes to pay back.** The three thought starters (neighborhood courts, a ZIP-code member vote, a Cardinals member tailgate) total a hypothetical $12.5M a year and pay for themselves at roughly 19,700 new primary members a year.

## How it was built

1. **Data.** [`data/DFCU_ALM_Workbook.xlsx`](data/DFCU_ALM_Workbook.xlsx) holds the financials and a monthly cash-flow ALM model: audited annual reports (FY2020–FY2025), NCUA 5300 call reports, Treasury curves, 20 rate scenarios, NII simulation, NEV, liquidity stress, policy limits and a hypothetical strategic plan. Every figure is tagged public, audited, modeled or hypothetical.
2. **Data layer.** [`data/build_dashboard_data.py`](data/build_dashboard_data.py) and [`data/build_brief_data.py`](data/build_brief_data.py) read the workbook, reconcile totals (balance sheet and income statement tie to reported figures) and produce the JSON embedded in the app.
3. **App.** React + Recharts in [`src/App.jsx`](src/App.jsx), bundled with esbuild into one self-contained `index.html`.

## Run it

Open `index.html` in any browser. No install needed.

To rebuild after editing `src/App.jsx`:

```bash
npm install
npm run build   # writes index.html
```

To regenerate the data JSON from the workbook:

```bash
pip install openpyxl
python data/build_dashboard_data.py
python data/build_brief_data.py
```

## Caveats

- Balance sheet and income totals are public or audited. Product splits, yields, behavior assumptions, fair values and all forward-looking figures are modeled or hypothetical.
- Policy limits, the strategic plan, initiatives, program budgets and peer medians are illustrative.
- The NCUA NEV Test uses an approximation of standardized share values.

## Author

Oliver · Finance and Marketing, W. P. Carey School of Business, Arizona State University
