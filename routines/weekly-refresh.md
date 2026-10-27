# Weekly Dividend Refresh

## Trigger
Every Monday at 7:00 AM in the user’s timezone. If the timezone is unknown, ask the user during setup and do not schedule until they answer.

## Instruction
Run the dividend-calendar skill.
- Re-pull holdings and recent dividend transactions, then recompute
- Update the "Dividend Calendar" Sheet: rewrite Payments rows (keep paid history) and refresh Summary, Monthly, Daily, and Holdings
- If a calendar is in use, roll it 3 months forward and update events in place (including when a projected date moves, or a projected event becomes declared). Touch only the chosen calendar
- Ask before any delete, including events for a sold position
- Post a short summary: income this week and month, changes since last week (new or sold tickers, payout changes, newly declared amounts), flags, and unresolved items
- If a connector fails, report the error and stop

## Safety
Ask before any delete. Do not contact anyone. Read-only on brokerage accounts.
