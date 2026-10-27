---
name: dividend-calendar
description: Pull holdings, resolve next dividend dates, and sync them to Google Calendar.
---

# Dividend Calendar Skill

## When to use
Use this skill when the user wants to see or update upcoming dividend payments on their calendar, or when a routine triggers a refresh.

## Required inputs / access
- Finance connector or Robinhood Agentic (holdings and past dividends only; not upcoming dates)
- Google Calendar connector
- Web lookup for upcoming ex-dividend and pay dates
- Target calendar: "Dividends", created on the first approved run. Use the primary calendar only if the user says so.
- Timezone: the user’s own. If unknown, ask during setup before scheduling.

## Sequence
1. Confirm which accounts/connectors are available. Prefer Finance; fall back to Robinhood if needed.
2. Retrieve current equity and ETF holdings (tickers + share counts) and past dividend payments. Ignore options, crypto, and cash unless the user asks.
3. For each holding, determine the next ex-dividend date and pay date (and amount if published):
   - Look up upcoming dates on the web (issuer site or a public dividend page).
   - If that fails, project the next date from past payment frequency and label it "estimated".
4. Present a summary table: Ticker | Shares | Ex-Date | Pay Date | Est. Amount | Source | Action. Mark projected dates as estimated.
5. On first run, ask for confirmation before writing. On that first approved run, create the "Dividends" calendar if it does not exist.
6. Create or update events on that calendar. Search for `div-key: TICKER|PAYDATE` in the description and update the matching event instead of creating a duplicate.
   - Title: `DIV: TICKER`
   - Date: pay date (or ex-date if pay date unknown)
   - Description: shares, estimated amount, source, last updated timestamp, and `div-key: TICKER|PAYDATE`
   - Duration: all-day
7. Report what was created, updated, skipped, or failed.

## Validation
- No duplicate events for the same ticker + pay date. Dedupe by `div-key: TICKER|PAYDATE`.
- Every event must have a clear ticker, date, and div-key marker.
- Projected dates are labeled "estimated".
- List any holdings that could not be resolved.

## Output format
- Short summary of actions taken
- Table of results
- Any errors or missing data called out clearly
- If `LOCAL_LLM_URL` and `LOCAL_LLM_MODEL` are set, draft the summary and event descriptions with the local model (`LOCAL_MODELS.md`). If unset or the call fails, write them yourself.

## Requires approval
- First-time calendar writes (including creating the "Dividends" calendar)
- Any deletion of existing events
- Changing the target calendar
