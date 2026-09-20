---
name: lbo-model
description: Build, populate, extend, repair, or audit formula-driven leveraged buyout models in Excel. Use for public-to-private or private-company LBOs, Sources & Uses, purchase accounting and pro forma closing balance sheets, stub-period three-statement forecasts, average-balance working-capital and interest schedules, debt cash sweeps, sponsor and management rollover returns, value-creation bridges, IRR/MOIC sensitivities, or LBO template completion.
---

# LBO Model

Build an auditable transaction model in which valuation, Sources & Uses, the closing balance sheet, forecast statements, debt paydown, and returns remain dynamically linked.

## Start from the evidence

1. Inspect every supplied workbook and note before building.
2. Preserve the reference model's analytical depth, period convention, sign convention, formulas, and visual language. Do not merely imitate colors.
3. Map historical statements and company KPIs to model drivers. Cite source files and primary-source URLs in comments or a Sources sheet.
4. If several templates are supplied, select the best mechanics from each and state the resulting architecture in the workbook.
5. Read [references/lbo-methodology.md](references/lbo-methodology.md) before building any full LBO or any model with a stub period.
6. When the user supplies a validated reference model, reuse its applicable mechanics and row structure, not its company-specific assumptions. Otherwise use the self-contained methodology and default layout below; no private reference workbook is required.

## Build in this order

1. **Timeline and conventions** — set closing date, fiscal year-end, day-count basis, stub fractions, exit date, currency, units, and scenario.
2. **Historical financials** — import at least three years when available, preserve reported subtotals, and derive normalized EBITDA/net income in visible rows.
3. **Assumptions** — separate operating, transaction, debt, tax, working-capital, and exit inputs from calculations.
4. **Sources & Uses** — distinguish public-company and private-company structures; balance sources and uses exactly.
5. **Pro forma closing balance sheet** — show pre-transaction, adjustments, and PF close. Reset historical equity, refinance debt, retain minimum cash, capitalize financing fees, expense transaction fees, and calculate goodwill visibly.
6. **Three statements and supporting schedules** — forecast the pre-deal stub, PF close, post-deal stub, full years, and exit stub; drive revenue, margins, fixed assets, working capital, taxes, cash, debt, leases, and retained earnings with formulas.
7. **Debt schedule** — model beginning balance, draws, mandatory amortization, revolver, cash sweep, optional paydown, PIK, fees, average-balance cash interest, and ending balance by tranche.
8. **Returns and attribution** — use actual dates and XIRR; calculate sponsor proceeds after rollover, dilution, and exit fees; reconcile investor value creation to net exit proceeds less total entry equity invested.
9. **Sensitivities and checks** — use odd-dimension grids centered on the base case and add visible balance, cash, debt, S&U, sign, and return checks.

## Canonical Debt Schedule format

Unless the user or supplied template explicitly requires a different presentation, use the Debt Schedule format below. Preserve its section order, cash-waterfall logic, tranche roll-forwards, and visible checks while adapting the assumptions to the target company.

Use the following sections in order:

1. **Period metadata** — show period start, period end, period fraction, and stub/full-year status.
2. **Cash Available for Debt Paydown** — use the fixed display sequence `Beginning Cash → FCF before Debt Repayment → Mandatory Senior Amortization → Minimum Cash Requirement → Revolver Draw → Revolver Repayment → Senior Optional Repayment / Cash Sweep → Ending Cash`. Show `Pre-Debt Excess / (Deficit) Cash = Beginning Cash + FCF before Debt Repayment - Minimum Cash Requirement` and, separately, the post-mandatory excess/(deficit) that determines the required revolver draw.
3. **Senior Term Loan** — show beginning balance, mandatory amortization, cash sweep, ending balance, interest rate, and cash interest.
4. **HoldCo PIK Mezzanine** — show beginning balance, capitalized PIK interest, ending balance, and PIK rate. Treat PIK as a fully capitalized noncash item and never include it in cash available for debt paydown.
5. **Revolver** — show beginning balance, commitment/capacity, draws, paydown, ending balance, interest rate, cash interest, and undrawn fee.
6. **Total Debt & Credit Metrics** — show beginning debt, draws, PIK/noncash accretion, mandatory paydown, sweep/optional paydown, ending debt, cash interest, PIK interest, total interest including lease interest where applicable, and relevant leverage/coverage ratios.
7. **Deferred Financing Fees** — show beginning balance, amortization, ending balance, and a visible roll-forward check.

Treat the three liquidity actions according to their economic nature, not as a single repayment-priority list. Mandatory Senior Amortization is a contractual obligation and is calculated first regardless of excess cash. After deducting mandatory amortization and the minimum-cash requirement, draw the revolver only for the resulting cash deficit; the revolver is a liquidity plug. If cash is in excess, repay outstanding revolver first, then apply the remaining excess cash to Senior Optional Repayment / Cash Sweep. Cash Sweep is an allocation of excess cash, not a contractual amortization item.

Keep the cash-available calculation separate from the tranche roll-forwards. For every debt tranche, show a visible check that `Beginning Debt + Drawdown + PIK/Noncash Accretion - Mandatory Amortization - Optional Repayment = Ending Debt`. For HoldCo PIK, use the standalone roll-forward `Beginning PIK + PIK Interest = Ending PIK`; do not include PIK interest in the operating cash waterfall, and deduct the ending PIK balance in the exit EV-to-equity bridge. Feed Core FCF from the indirect-method CFS into the Debt Schedule, calculate ending cash in the Debt Schedule, and link BS cash directly to that ending cash; never use cash as a balance-sheet plug. Retain the transaction's actual period columns, including stub periods when applicable.

## Non-negotiable modeling rules

- Put assumptions in visible input cells; never hide business assumptions inside formulas.
- When the user designates a historical-statement workbook, preserve every reported statement line and historical amount in the model; link the displayed historical periods to source tabs by formula. Put analytical aggregation only in supporting schedules, not in the historical IS, BS, or CFS.
- Integrate LBO-specific IS and BS lines into the relevant statement sections and reported subtotals. Do not append a separate supplemental block below the statements unless the user or supplied template explicitly requires it. Preserve the original historical reported lines and use zero/blank historical values for deal-only lines.
- Unless the user or supplied template explicitly requires a direct-method presentation, present the model CFS using the indirect method: start with net income, add back D&A, PIK interest and financing-fee amortization as applicable, reflect working-capital and other operating adjustments, then show capex, lease cash payments, debt draws/paydowns, cash interest, dividends, and other financing cash flows. Preserve the original historical CFS line-by-line on a separate source/history tab and reconcile the indirect-method historical CFO, CFI, CFF, and ending cash to it.
- Put each operating driver's historical evidence in the same table block as its Bull / Base / Bear forecast assumptions. Show at least three historical years and a historical average when available; historical values and ratios must be cross-sheet formulas linked to statements or schedules, not copied hardcodes. A separate historical-driver table is not a substitute.
- Use formulas for every derived value. Keep copy-across patterns consistent.
- Quote every cross-sheet reference, such as `='Debt Schedule'!H22`.
- Use actual dates for closing and exit. Do not approximate a five-year hold with five full fiscal years when the deal closes mid-year.
- Maintain an explicit mapping from every income-statement period to its assumptions period. Annualize a prior period using that prior period's own fraction, never the current column's fraction.
- Calculate historical working-capital turnover on average balances and solve forecast ending balances from the same definition: `Ending = MAX(0, 2 × Annualized Flow / Turnover - Beginning)`.
- Roll PP&E, intangible assets, and right-of-use assets in separate schedules. Never forecast D&A only as a percentage of revenue.
- Calculate cash interest on average beginning/ending debt and revolver undrawn fees on average beginning/ending undrawn capacity, multiplied by the period fraction.
- If average-balance interest creates a cash-sweep circularity, use controlled Excel iteration and a documented zero initializer at the circular entry point. Do not leave `#VALUE!` on first calculation.
- Prevent negative debt balances with `MIN`/`MAX`; apply paydown in contractual priority.
- Do not subtract net debt from a P/E-derived exit equity value. Do subtract net debt from an EV-derived exit value.
- When the selected EV / EBITDA convention treats lease liabilities as debt, deduct current and non-current lease liabilities consistently in both entry and exit EV-to-equity bridges. State whether leases are assumed or refinanced in Sources & Uses so they are not funded or deducted twice.
- Model financing-fee amortization as noncash; add it back in cash flow and reduce the deferred financing-fee asset.
- Model transaction fees as a use and expense/equity reduction, not as a deferred financing asset.
- If ROU depreciation is added back in cash flow, model the corresponding lease principal cash payment or an explicit lease-liability roll-forward.
- When the user requires historical goodwill to be eliminated at PF close, reset historical goodwill and show the specified purchase-accounting bridge explicitly. If the requested convention is `LBO goodwill = equity purchase price - original net tangible asset value`, calculate original net tangible asset value as parent-attributable equity less historical goodwill and retain only the resulting LBO goodwill after close.
- Define entry value consistently: P/E produces purchase equity value; transaction enterprise value equals purchase equity plus pre-deal net debt. Do not use PF-close net debt to reconstruct entry transaction EV.
- For investor value creation, compare net exit equity proceeds with total entry equity invested. Entry transaction and financing fees are funded in Sources & Uses and must be deducted exactly once.
- Forecast cash must never be a balance-sheet balancing item. Calculate CFO in the cash flow statement, feed Core FCF into the debt waterfall, calculate ending cash in the debt schedule, and link BS cash directly to that ending cash. If the BS does not balance, trace the operating, asset, lease, debt, fee, and equity roll-forwards; do not force balance through cash or a residual cash-flow line. Label and justify any other unavoidable plug.

## Default workbook architecture

Use the reference template when supplied. Otherwise use:

1. `Cover`
2. `Assumptions`
3. `Sources & Uses`
4. `Income Statement`
5. `Balance Sheet`
6. `Cash Flow`
7. `Debt Schedule`
8. `Return Analysis`
9. `Checks`
10. `Sources`

Keep a PF-close column on the balance sheet. Keep stub periods on the income statement, cash flow, and debt schedule.

For a Chinese company using Chinese financial statements, default sheet names, titles, statement labels, notes, and instructions to Chinese. Retain only useful market-standard abbreviations such as LBO, EV/EBITDA, PIK, IRR, MOIC, PP&E, ROU, or other terms whose translation would reduce clarity.

## Formatting

- Hide gridlines and use a restrained institutional layout.
- Follow user-specified typography first. If the user requests separate Chinese and Latin fonts, apply the East Asian font and Latin font consistently across every populated cell; otherwise use Arial 8–10 pt unless the template specifies another standard.
- Use dark blue section bars with white bold text and light blue period headers.
- Any cell with a dark fill must use white text, including titles, section bars, period headers, status cells, and dark-filled subtotal rows.
- Use blue font for editable inputs, black for formulas, green for cross-sheet links, and red only for external links.
- Use yellow fill sparingly for key decision inputs.
- Apply explicit financial, percentage, multiple, and date formats. Follow the user's requested precision across the full workbook; absent a request, use one decimal for financial models. Display zeros as `-`.
- Freeze panes on long sheets and keep actuals visually distinct from forecasts.
- Set each sheet's freeze pane once, after the layout is complete. Do not freeze and then clear or replace it, because stale pane selections can make Excel repair the sheet view.

## Verification

1. Run `scripts/audit_lbo_model.py <workbook.xlsx>`.
2. Recalculate and save in native Excel with iteration enabled when the model intentionally contains average-interest circularity.
3. Run a native Excel error scan after recalculation; static formula counts do not detect formulas that point to text labels. In every helper table, confirm formulas reference the value column rather than the adjacent label column.
4. Confirm all checks show `OK`, Sources equal Uses, balance sheets balance, cash ties, debt rolls, and the sensitivity center equals the base case.
5. Inspect every user-facing sheet visually for clipping, broken formulas, unreadable colors, accidental blank areas, and unusable frozen panes.
6. Trace at least one path from an operating assumption through cash flow and debt paydown to sponsor IRR.
7. Independently recompute `Net Exit Equity Proceeds - Total Entry Equity Invested` and confirm it equals total value creation after exit fees.
