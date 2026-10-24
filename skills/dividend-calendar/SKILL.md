---
name: dividend-calendar
description: Pull holdings, resolve next dividend dates, and sync them to Google Calendar.
---

# Dividend Calendar Skill

## When to use
Use this skill when the user wants to see or update upcoming dividend payments on their calendar, or when a routine triggers a refresh.

## Required inputs / access
- Finance connector or Robinhood Agentic (for holdings)
- Google Calendar connector
- Optional: user-specified calendar name (default: “Dividends” or primary calendar)

## Sequence
1. Confirm which accounts/connectors are available. Prefer Finance; fall back to Robinhood if needed.
2. Retrieve current equity and ETF holdings (tickers + share counts). Ignore options, crypto, and cash unless the user asks.
3. For each holding, determine the next ex-dividend date and pay date (and estimated amount if available). Use the connector data first; if missing, note it and continue.
4. Present a summary table: Ticker | Shares | Ex-Date | Pay Date | Est. Amount | Action.
5. On first run, ask for confirmation before writing to the calendar.
6. Create or update Calendar events:
   - Title: `DIV: TICKER`
   - Date: pay date (or ex-date if pay date unknown)
   - Description: shares, estimated amount, source, last updated timestamp
   - Duration: all-day
7. Report what was created, updated, skipped, or failed.

## Validation
- No duplicate events for the same ticker + pay date.
- Every event must have a clear ticker and date.
- List any holdings that could not be resolved.

## Output format
- Short summary of actions taken
- Table of results
- Any errors or missing data called out clearly

## Requires approval
- First-time calendar writes
- Any deletion of existing events
- Changing the target calendar
