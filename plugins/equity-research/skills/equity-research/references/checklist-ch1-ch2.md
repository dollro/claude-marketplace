# Analyst Checklist — Chapters 1 & 2: Fundamental Quality + Valuation

> What to pull from MCP, what to calculate, what to search, how to assess.

---

## Chapter 1: Fundamental Quality

### 1.1 — Revenue Growth & Trajectory

**MCP calls:**
- `get_financial_statement(income_stmt)` → "Total Revenue" for 3–4 years. Calculate YoY growth.
- `get_financial_statement(quarterly_income_stmt)` → Quarterly revenue for 8 quarters. QoQ and YoY quarterly.

**Calculations:**
- 1yr revenue growth, 3yr CAGR
- Sequential QoQ growth for last 4 quarters — accelerating or decelerating?
- YoY quarterly (removes seasonality)

**Web search:** "[Company] revenue guidance", "[Company] organic growth", "[Sector] industry growth rate"

**Key questions:** Organic vs. acquisition-driven? Last 2Q accelerating/decelerating? Outgrowing market?

---

### 1.2 — Margin Profile & Trend

**MCP calls:**
- `get_financial_statement(income_stmt)` → Revenue, Gross Profit, Operating Income, EBITDA, Net Income (3–4 yrs)
- `get_financial_statement(quarterly_income_stmt)` → Same metrics quarterly (8Q)

**Calculations:**
- Gross/operating/EBITDA/net margin + 8Q trend
- Margin delta: latest Q vs. same Q prior year vs. 2yr ago
- Incremental margin = Δ operating income / Δ revenue

**Web search:** "[Company] margin expansion guidance", "[Sector] average operating margin"

**Key questions:** Expanding/stable/compressing? Gross margin erosion? Operating leverage?

---

### 1.3 — ROIC

**MCP calls:**
- `get_financial_statement(income_stmt)` → Operating Income, Tax Rate
- `get_financial_statement(balance_sheet)` → Equity, Total Debt, Cash

**Calculations:**
- NOPAT = Operating Income × (1 − tax rate)
- Invested Capital = Equity + Debt − Cash
- ROIC = NOPAT / Avg Invested Capital. 3yr trend.
- ROIC spread = ROIC − WACC (est. 8–10% most large-caps)

---

### 1.4 — Cash Flow Quality

**MCP calls:**
- `get_financial_statement(cashflow)` → Operating CF, Capex, FCF (3–4 yrs)
- `get_financial_statement(quarterly_cashflow)` → Recent 4Q

**Calculations:**
- FCF = Operating CF − Capex
- FCF margin, FCF conversion (FCF/NI), FCF yield (FCF/MCap)
- Capex intensity (Capex/Revenue)
- Accrual ratio = (NI − Operating CF) / Total Assets

---

### 1.5 — Balance Sheet

**MCP calls:**
- `get_financial_statement(balance_sheet)` → Debt, Cash, Current Assets/Liabilities, Equity
- `get_financial_statement(income_stmt)` → EBITDA, Interest Expense

**Calculations:**
- Net debt = Total Debt − Cash
- Net debt/EBITDA, current ratio, interest coverage, debt/equity
- Compare to sector corridors from frameworks.md

**Web search:** "[Company] credit rating", "[Company] debt maturity schedule"

---

### 1.6 — Earnings Quality

**MCP calls:**
- `get_financial_statement(income_stmt)` → GAAP Operating Income vs. EBITDA gap
- `get_financial_statement(quarterly_income_stmt)` → Q-to-Q earnings consistency
- `get_financial_statement(cashflow)` → NI vs. Operating CF gap

**Web search:** "[Company] earnings surprise history", "[Company] stock-based compensation"

---

## Chapter 2: Valuation

### 2.1–2.3 — Absolute, Relative, Historical

**MCP calls:**
- `get_stock_info` → P/E, EPS, Market Cap, EV
- `get_financial_statement(income_stmt)` → Revenue, EBITDA, NI for multiples
- `get_financial_statement(cashflow)` → FCF for P/FCF
- `get_historical_stock_prices(5y, 1wk)` → 5yr prices for historical range

**Calculations:**
- Forward P/E, EV/EBITDA, P/FCF, EV/Revenue, PEG, FCF yield, earnings yield
- Compare to sector medians, peer group, own 5yr history

**Web search:** "[Sector] forward P/E median", "[Company] peer group valuation", "[Company] analyst price target consensus", "[Company] historical P/E range"

### 2.4 — DCF

**Quick DCF:** 5yr projection using current FCF, conservative growth (below guidance), terminal growth 2.5–3.5%, appropriate WACC. Sensitivity table: ±1% growth and WACC.

**Reference:** See `valuation-methods.md` for full DCF methodology.
