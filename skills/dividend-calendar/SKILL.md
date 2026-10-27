---
name: dividend-calendar
description: >-
  Build and refresh a projected dividend calendar (Google Sheet by default,
  Google Calendar optional) from the user's brokerage holdings, Snowball-style.
  Use when someone wants upcoming or projected dividend income, a dividend
  calendar, or when a routine triggers a refresh.
---
# Dividend Calendar

Projects each holding's future dividends (latest per-share payout x current shares, on the fund's inferred cadence) and keeps them in a Google Sheet, with an optional Google Calendar. Works for any user with no browser sign-ins.

## 1. Setup (first run only; skip anything already in place)
1. Check connections with GetMcpServerStatus. You need:
   - Holdings: the Finance connector (preferred) or Robinhood Agentic. If missing, offer it and show its connect card.
   - Output: the Google Drive connector (for the Sheet). The Google Calendar connector is optional, only if the user wants calendar events.
   Show only the connect cards that are missing. Never ask the user to sign in to Google in a browser.
2. Ask the user (one question at a time, with sensible defaults):
   - Output: Sheet only (default), or Sheet plus Calendar.
   - Accounts: all accounts combined (default), or specific ones. Always keep per-account columns.
   - Projection basis: latest payout (default, matches Snowball), or the average of the last 4 payouts for weekly or monthly payers whose latest payout is an outlier.
   - Calendar horizon (if Calendar): 3 months (default). The Sheet always covers 12 months.
3. Calendar target: connectors usually can't create calendars. Use a calendar the user already has called "Dividends" if one exists; otherwise ask the user to make one (or use their primary calendar). Save the chosen calendar id to memory. Changing it later needs approval.
4. Create the Sheet through Drive (build an .xlsx with formulas and upload it with conversion to Google Sheets). Name it "Dividend Calendar", keep it private, and save its file id to memory.
5. Show a preview table and get approval before the first write to the Sheet or Calendar.

## 2. Data collection
- Pull accounts, holdings, and the last ~12 months of dividend and cash transactions. Use equities and ETFs only; leave out options, crypto, cash, and "other" asset types unless asked.
- Get dividend history and announcements for each ticker from the connector first, then from reliable public sources (Nasdaq dividend history, stockanalysis.com, the issuer or fund site). Record the source URL. Never make up dates or amounts.
- Use Finance dividend credits to mark past payments as paid, matching them by date and amount. Credits with no ticker (often from sold positions) go under "OTHER"; try to attribute them from sell history.

## 3. Projection rules and edge cases
- **Cadence:** infer weekly, monthly, quarterly, semiannual, or annual from recent history, along with the usual gap between ex-date and pay date. Prefer an issuer's published distribution calendar when one exists (pay dates are confirmed even when amounts are estimated).
- **Holidays and weekends:** move a projected pay or ex-date that lands on a weekend or a US market or bank holiday (including Veterans Day and Columbus Day, when banks close but markets are open) to the next business day.
- **Payout volatility:** for option-income or weekly ETFs, flag the holding when the latest payout is more than 30% below or above the trailing 4-payout average. Project with the user's chosen basis and show both figures.
- **Special, one-time, return-of-capital, and liquidating distributions:** don't repeat them. Project from the last regular payout.
- **Cuts, suspensions, and new payers:** a suspended dividend projects as zero. A new payer with fewer than 2 payments has an unknown cadence, so flag it and don't project it.
- **Stock splits and reverse splits:** adjust historical per-share amounts to the current share basis before projecting.
- **Mergers, ticker changes, spin-offs, delistings:** follow the new ticker, carry over its history where you can, and flag positions that no longer exist.
- **Sold positions:** stop projecting immediately. Keep their past paid rows.
- **Ex-date eligibility:** shares bought on or after the ex-date don't get that payment. Use the share count as of the ex-date when activity shows a change.
- **Fractional shares and DRIP:** support fractional shares. If dividends are reinvested, projecting at current shares is fine; note that it understates future income.
- **Foreign and ADR dividends:** convert to the account's currency, note foreign withholding (gross vs net), and expect irregular amounts.
- **Accounts:** keep each account separate (taxable vs IRA, and so on). Totals can combine them, but show the per-account split so the numbers can be compared with tools that track one account.
- **Status:** each payment is paid (cash received), declared (officially announced), or estimated (projected).
- **No data:** list the ticker as unresolved with the reason. Never fill in a guess.

## 4. Sheet layout
- **Summary:** annual (next 12 months), monthly average, daily average, portfolio yield (annual income divided by the current market value of dividend holdings), paid and yet to receive this month, per-account totals, and a last-updated time. All are formulas based on Payments.
- **Payments:** Pay Date, Ex-Date, Ticker, Account, Shares, Per-Share, Est. Total, Status, Frequency, Basis, Flags, Source. One row per account per payment, sorted by date.
- **Monthly:** 12 months with paid, declared, and estimated columns (SUMIFS), a stacked column chart, and a ticker-by-month table.
- **Daily:** total and tickers for each pay date.
- **Holdings:** shares, last payout, trailing average, frequency, annual income, price, market value, and yield.

## 5. Calendar events (optional)
- All-day events on the pay date (the ex-date if no pay date is known). Titles are "DIV: TICKER" when declared or paid, and "DIV: TICKER (proj)" when estimated.
- Description: shares, per-share amount, estimated total, status, basis, source, and last updated.
- Dedupe key: ticker plus pay date. Read the existing events in the window before writing. Create in batches of 50 or fewer.
- On a refresh, update events in place, including when a projected date moves, or a (proj) event becomes declared and gets its title changed. Deleting events, such as for a sold position, needs the user's approval.
- Only touch the chosen calendar.

## 6. Weekly refresh
Re-pull holdings and transactions, recompute, rewrite the Payments rows (keeping paid history), roll the calendar window forward, and post a short summary: income this week and month, changes since last week (new or sold tickers, payout changes, newly declared amounts), flags, and unresolved items. If a connector fails, report it and stop.

## 7. Optional local models
If the user has a local model endpoint or script configured (noted in memory), you can hand off text work such as summaries and drafts. Keep data lookups, math, decisions, and writes yourself. If it fails, do the work normally.

## Rules
- Read-only on brokerage accounts. Never trade, transfer, or contact anyone.
- The first write and any deletion need approval. After the format is approved, refreshes can update rows and events.
- Every report covers what was found, what was created, updated, or skipped, and what couldn't be resolved.
