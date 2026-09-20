# LBO Methodology

## Contents

1. Timeline and stub periods
2. Driver assumptions and working capital
3. Fixed assets, leases, and three-statement integration
4. Entry valuation, Sources & Uses, and PF close
5. Debt schedule and cash waterfall
6. Exit, sponsor returns, and value creation
7. Sensitivities, formatting, and QA

## 1. Timeline and stub periods

Anchor the model on real dates.

- `Close date`: legal/economic transaction date.
- `Fiscal year-end`: target's reporting year-end.
- `Stub fraction`: `YEARFRAC(close date, next fiscal year-end, basis)` or an explicit day-count fraction.
- `Exit date`: close date plus the stated holding period.

For a 30 June close and 31 December year-end, model:

- Balance sheet: historical year-end, close-date pre-deal, close-date PF close, next year-end, later year-ends, and exit-date balance sheet.
- Income statement and cash flow: close-date pre-deal stub, post-close stub, full fiscal years, and a final exit stub.
- LTM at exit: combine the unowned half of the prior fiscal year with the final exit stub. Do not use a full fiscal-year value if the exit occurs mid-year.

Apply period fractions to revenue, operating expenses, D&A, interest, taxes, mandatory amortization, fee amortization, and cash sweep inputs as appropriate. Balance-sheet amounts remain point-in-time values.

Maintain an explicit `IS period -> Assumptions period` map. When a formula annualizes the previous income-statement period, divide by that previous period's own fraction. A common failure is dividing a full-year forecast by the preceding stub's `0.5` fraction, which doubles the following year's revenue.

For a 30 June 2026 close and a 30 June 2031 exit:

- Entry LTM earnings = 2025H2 + 2026H1 pre-deal.
- Exit LTM earnings = 2030H2 + 2031H1 exit stub.
- PF close adjustments start from the forecast 30 June 2026 pre-deal balance sheet, not from 31 December 2025.

## 2. Driver assumptions and working capital

Show the latest three available historical years of driver ratios and a visible historical average in the same operating-scenario table block as Bull / Base / Bear forecast assumptions. Each historical value or ratio must be a cross-sheet formula linked to the historical statements or supporting schedules; do not copy a hardcoded ratio into the assumptions table, and do not replace the integrated evidence with a standalone historical-driver table. Forecast inputs remain editable blue hardcodes. Use the historical average as evidence, not as an automatic answer: normalize step changes, outliers, acquisition effects, and clearly unsustainable growth or margin trends.

### Average-balance turnover

If historical turnover is calculated on average balance, the forecast balance must use the same definition.

`Turnover = Annualized Flow / Average(Beginning Balance, Ending Balance)`

Solving for forecast ending balance:

`Ending Balance = MAX(0, 2 × Annualized Flow / Turnover - Beginning Balance)`

Use the relevant flow:

- accounts receivable and contract assets: revenue;
- inventory and accounts payable: cost of sales;
- prepayments: the operating expense or purchase flow that best matches the account;
- other payables and contract liabilities: the disclosed operating flow or revenue, consistently applied.

For stub periods, annualize the flow with that period's own fraction before applying turnover. Do not use `Flow / Turnover` when assumptions were derived from average balances; that formula estimates average balance, not ending balance.

## 3. Fixed assets, leases, and three-statement integration

Build separate roll-forwards:

- `Ending PP&E = Beginning PP&E + PP&E Capex - Depreciation - Disposals`;
- `Ending Intangibles = Beginning Intangibles + Intangible Capex - Amortization - Disposals`;
- `Ending ROU Asset = Beginning ROU Asset + Lease Additions - ROU Depreciation`.

Keep capex rates and depreciation/amortization rates as separate assumptions. If depreciation is based on average assets, solve the algebra consistently rather than applying a revenue ratio.

If ROU depreciation is added back as a noncash item in cash flow while lease additions keep the ROU asset stable, include the corresponding lease-principal cash payment and lease-liability movement. Otherwise the balance sheet will be out by exactly the ROU depreciation/addition amount.

The income statement should show revenue, gross profit, operating expenses, EBITDA, D&A, EBIT, cash interest, undrawn fees or related financing operating costs, financing-fee amortization, interest income, taxes, and net income. Keep adjusted and reported historical metrics distinct.

The cash flow statement starts from net income, adds back noncash D&A and financing-fee amortization, deducts working-capital investment and capex, includes lease principal where relevant, and links financing flows to the debt schedule. Ending cash must equal balance-sheet cash.

## 4. Entry valuation, Sources & Uses, and PF close

### Valuation basis

- P/E gives equity value: `Equity Value = LTM Adjusted Net Income × P/E`.
- EV/EBITDA gives enterprise value: `Equity Value = EV - debt - preferred - NCI + cash`.
- If the transaction's EV / EBITDA definition treats leases as debt, include current and non-current lease liabilities in the entry and exit bridges. Distinguish leases assumed at close from lease debt that is refinanced: an assumed lease reduces equity value but is not also a cash use.
- Use diluted shares times offer price when the transaction is expressed per share.
- Normalize material noncash or one-time gains/losses in a visible bridge.

Never subtract debt again from a P/E-derived equity value. P/E already capitalizes earnings after interest.

Purchase equity under a P/E entry must use LTM adjusted net income, never EBITDA. For a fixed-leverage structure, size senior and junior debt from entry LTM EBITDA (for example, 3.0x senior and 2.0x junior) and plug sponsor equity after management rollover and other sources.

### Public-company LBO

Typical uses:

- purchase of common equity;
- refinancing existing interest-bearing debt and lease debt when required;
- transaction fees;
- financing fees and OID;
- optional minimum-cash funding.

Typical sources:

- new debt;
- sponsor equity;
- management rollover;
- available cash/restricted cash released at closing;
- seller note or other negotiated sources.

### Private-company LBO

If the model begins from enterprise value, avoid separately adding refinanced debt and cash unless the purchase-price definition requires it. Make the bridge explicit.

### Rollover and capital mix

Treat management rollover as an equity source, not a reduction of uses. If debt is a fixed percentage of total uses, calculate debt from total uses and plug sponsor equity after rollover. Separate ownership percentage from funding percentage when fees make them differ.

Show `Pre-transaction`, `Adjustments`, and `PF Close`.

First roll the operating balance sheet from the last historical year-end to the close-date pre-deal node using the same forecast mechanics as later periods. Only then apply the transaction adjustments to create PF close.

- Retain minimum cash and use excess cash as a source.
- Release restricted cash only when the linked restriction/debt is settled.
- Refinance identified debt; do not include normal trade liabilities in debt.
- Capitalize OID and lender/financing fees as deferred financing fees.
- Expense transaction fees at close.
- Eliminate historical equity and establish sponsor/rollover equity.
- Calculate goodwill visibly. A simplified bridge is `Purchase Equity Price - Historical Net Tangible Assets`, subject to purchase-accounting adjustments.
- Include a zero balance-sheet check.

## 5. Debt schedule and cash waterfall

Build each tranche separately. Recommended order:

1. beginning balance;
2. scheduled/mandatory amortization;
3. revolver draw or paydown to protect minimum cash;
4. cash sweep on residual cash;
5. optional paydown and prepayment penalty;
6. PIK accrual, if any;
7. ending balance.

Calculate cash interest on the average of beginning and ending debt balances, multiplied by the rate and period fraction. Calculate revolver undrawn fees on average beginning/ending undrawn capacity, also multiplied by the period fraction, and include the fee in SG&A or the relevant operating-expense line.

Average-balance interest and a cash sweep create an intentional circularity. Use controlled native-Excel iteration, with a formula such as `IFERROR(core FCF link,0)` at the circular entry point so the workbook initializes to numbers rather than `#VALUE!`. Enable iteration explicitly before final recalculation; a practical default is 100 iterations and 0.001 maximum change. Document the circularity and verify convergence.

Do not sweep cash while the revolver is drawn unless the model first repays the revolver. No tranche may become negative.

Useful checks:

- debt never goes below zero;
- ending balance equals beginning plus draws/PIK less repayments;
- cash interest coverage exceeds the selected threshold;
- DSCR = `(EBITDA - capex - change in NWC - cash taxes) / (cash interest + mandatory principal)`.

## 6. Exit, sponsor returns, and value creation

### EV-based exit

`Exit Equity Value = Exit EBITDA × Exit EV/EBITDA - Exit Net Debt - Exit Fees`.

### P/E-based exit

`Exit Equity Value = Exit LTM Adjusted Net Income × Exit P/E - Exit Fees`.

Do not subtract debt again in a P/E exit. Show implied enterprise value and net debt only as a reconciliation.

Allocate exit equity among sponsor, management, and option holders using the agreed ownership/dilution rules. Use negative entry cash flow and positive proceeds. Use XIRR with actual dates for stub or irregular periods, and calculate MOIC as proceeds divided by invested capital.

### Value-creation bridge

Distinguish market equity-value change from investor value creation:

- Pure equity-value change = gross exit equity value - purchase equity value.
- Total entry equity invested = sponsor initial equity + management rollover from Sources & Uses.
- Investor actual value creation = net exit equity proceeds - total entry equity invested.

Total entry equity already funds transaction and financing fees. Deduct those fees exactly once in attribution. Do not validate the bridge against purchase equity value with fees removed from both sides.

When the requested attribution is `change in earnings`, `change in multiple`, and `change in net debt`, use a reconciled EV/EBITDA bridge even if entry and exit valuation are stated as P/E:

1. `Entry Operating EV = Purchase Equity Value + Pre-Deal Net Debt` from the transaction/Sources & Uses bridge. Do not use PF-close net debt here.
2. `Exit Operating EV = Gross Exit Equity Value + Exit Net Debt`.
3. `Entry Implied Multiple = Entry Operating EV / Entry LTM EBITDA`.
4. `Exit Implied Multiple = Exit Operating EV / Exit LTM EBITDA`.
5. `Earnings Contribution = (Exit LTM EBITDA - Entry LTM EBITDA) × Entry Implied Multiple`.
6. `Multiple Contribution = (Exit Implied Multiple - Entry Implied Multiple) × Exit LTM EBITDA`.
7. `Net Debt / Fee Contribution = PF-Close Net Debt - Exit Net Debt - Upfront Financing Fees - Transaction Fees`.
8. Show exit fee as a separate negative item.
9. Reconcile total contribution after exit fee to `Net Exit Equity Proceeds - Total Entry Equity Invested` with zero difference.

Cash interest and undrawn fees are not separately deducted in this bridge because they already reduce earnings, cash generation, and debt paydown.

## 7. Sensitivities, formatting, and QA

Use 5×5 or 7×7 grids. Put the base assumptions in the center and highlight the center cell. Each sensitivity formula must vary both row and column drivers and recalculate the relevant operating/valuation logic.

Minimum checks:

- Sources minus Uses = 0;
- every balance sheet balances;
- ending cash ties between cash flow and balance sheet;
- debt ending balances tie between debt schedule and balance sheet;
- deferred financing fees roll forward;
- sponsor entry cash flow is negative and exit cash flow positive;
- sensitivity center equals base-case IRR/MOIC;
- no formula errors, external links, or negative debt balances.
- close-date pre-deal, PF-close, every forecast year-end, and exit balance sheets all balance;
- historical turnover formulas use average balances and forecast formulas algebraically solve the same definition;
- PP&E, intangibles, ROU assets, leases, debt, deferred financing fees, and retained earnings roll forward;
- entry debt sizing matches the stated EBITDA multiples;
- value creation reconciles to net exit equity proceeds less total entry equity invested;
- helper tables point to numeric value columns, not adjacent text-label columns.

Run both a static workbook audit and a native Excel full recalculation/error scan. Static inspection cannot detect every formula that resolves to text or every cached `#VALUE!`. Save the recalculated workbook and reopen/read back the final target.

Use consistent formatting: Arial 9 pt body and 14 pt titles unless the template requires otherwise; blue font for hardcoded inputs, black for formulas, green for cross-sheet links, restrained section fills, and uniform number formats. Set each freeze pane once after layout is final, because repeated freeze/unfreeze changes can create sheet-view repair warnings.
