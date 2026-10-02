# DFCU ALM Command Center

I built this to answer a question I kept running into: how do you prove a brand idea is worth funding? I wrote a creative brief for Desert Financial Credit Union, then built a full asset/liability management (ALM) dashboard to test it. The brief defines the member behavior I want to change. The ALM model shows what that change could be worth on the balance sheet, and whether the credit union could afford to fund it.

**Live demo:** https://ohoward17.github.io/desertfinancialstrategy/

> Hypothetical concept built from public data. Not affiliated with or endorsed by Desert Financial Credit Union.

## The idea

My [creative brief](docs/creative-brief.md) starts from a simple problem. Desert Financial is well known in Arizona, but people who already bank with a big national bank have no real reason to switch. Their bank works. So the brief's proposition is *"Make Arizona better in ways members can see and feel."* Being from Arizona doesn't make a company part of Arizona. Changing something here does.

The marketing side of that is easy to get excited about. The harder question is the one a CFO would ask: what is it actually worth? So I traced the idea all the way through to the balance sheet:

**Creative strategy → member behavior change → balance-sheet impact → financial impact → capacity to reinvest**

- **Member behavior.** The goal is primacy: getting people to make Desert Financial their main bank, with checking and direct deposit.
- **Balance-sheet impact.** Consumer checking averages $3,899, costs 0.06% and runs off about 14% a year. It's the cheapest, stickiest funding the credit union has. I model new checking as replacing 3.88% certificate funding.
- **Financial impact.** In my base scenario (15,000 new primary members a year, 60% direct-deposit adoption), each new member is worth about $197 a year in net interest income and interchange, building to about $10M a year by 2030.
- **Capacity to reinvest.** The plan's ROA has about $24M of room above a 0.95% floor in its tightest year, and the strategy's member value adds to that. My three thought starters (neighborhood courts and fields, a ZIP-code member vote, a Cardinals member tailgate) come to a hypothetical $12.5M a year and fit inside it.

Every one of those assumptions is a slider on the Strategy page, so you can change them and watch deposits, NII, ROA, net worth and remaining capacity move.

![Strategy tab](docs/screenshot-strategy.png)

## What's inside

| Section | What it shows |
|---|---|
| **Executive Dashboard** | KPIs as of June 30, 2026 (assets, NIM, net worth, efficiency, ROA, liquidity), trends since 2021, balance sheet mix, loan and share composition |
| **ALM & Risk** | Repricing gap, Year 1 and Year 2 NII sensitivity (-300 to +300bp) against board limits, NEV and the NCUA NEV Supervisory Test, the full model assumption register |
| **Financial Documents** | June 30, 2026 balance sheet, FY2025 vs. FY2024 audited income statement, key ratios vs. peers, liquidity and contingent funding |
| **Strategy** | The brief translated into financial assumptions: a logic flow from "make local visible" to "more capacity to invest in Arizona," an interactive scenario model (new primary members, checking balance, direct-deposit adoption, runoff, community investment), Community Impact Capacity by plan year, the three thought starters sized as initiatives, and the 2026–2030 plan |
| **AI Analyst** | Scripted Q&A grounded in the model's numbers, including strategy questions answered by the same scenario model the Strategy page uses |

I kept ALM at the core on purpose. The Strategy page is a layer on top of the model, not a replacement for it. Everything on it is clearly labeled as a hypothetical scenario so it never gets mixed up with reported figures.

## How I built it

1. **Data.** [`data/DFCU_ALM_Workbook.xlsx`](data/DFCU_ALM_Workbook.xlsx) holds the financials and a monthly cash-flow ALM model: audited annual reports (FY2020–FY2025), NCUA 5300 call reports, Treasury curves, 20 rate scenarios, NII simulation, NEV, liquidity stress, policy limits and a hypothetical strategic plan. Every figure is tagged public, audited, modeled or hypothetical.
2. **Data layer.** [`data/build_dashboard_data.py`](data/build_dashboard_data.py) and [`data/build_brief_data.py`](data/build_brief_data.py) read the workbook, check that the balance sheet and income statement tie to reported totals, and produce the JSON embedded in the app.
3. **Scenario model.** `runScenario()` in [`src/App.jsx`](src/App.jsx) turns the strategy assumptions into yearly cohorts of primary members, deposits, NII, interchange, ROA and net worth. The Strategy page and the AI Analyst both run on it, so their numbers always match.
4. **App.** React + Recharts in [`src/App.jsx`](src/App.jsx), bundled with esbuild into one self-contained `index.html`.

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
- The strategy scenario is not a forecast or a Desert Financial plan. It assumes new checking replaces certificate funding, that members without direct deposit hold half the balance and do half the debit activity, and it leaves out lending cross-sell. Interchange is held at today's rate even though Durbin caps would apply above $10B in assets.
- Policy limits, the strategic plan, initiatives, program budgets and peer medians are illustrative.
- The NCUA NEV Test uses an approximation of standardized share values.

## Author

Oliver · Finance and Marketing, W. P. Carey School of Business, Arizona State University
