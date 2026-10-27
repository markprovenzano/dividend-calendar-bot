# Weekly Dividend Refresh

## Trigger
Every Monday at 7:00 AM in the user’s timezone. If the timezone is unknown, ask the user during setup and do not schedule until they answer.

## Instruction
Run the dividend-calendar skill.
- Pull latest holdings and past dividends
- Resolve next ex-dividend and pay dates by web lookup (issuer site or a public dividend page). If lookup fails, project from past payment frequency and label the date "estimated"
- Update events on the "Dividends" calendar (primary only if the user said so)
- Match existing events by `div-key: TICKER|PAYDATE` and update them (do not create duplicates)
- Post a short summary in this conversation
- If a connector fails, report the error and stop

## Safety
Do not delete events. Do not contact anyone. Read-only on brokerage accounts.
