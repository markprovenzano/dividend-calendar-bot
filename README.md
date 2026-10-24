# Dividend Calendar Bot

A Grok Bot template that keeps an up-to-date Google Calendar of upcoming dividend payments for your holdings.

## What it does
- Pulls current holdings from Finance or Robinhood
- Looks up or estimates next ex-dividend / pay dates
- Creates or updates Calendar events
- Can run on a schedule (weekly refresh recommended)

## Connectors required
- Google Calendar
- Finance **or** Robinhood Agentic (or both)

## How to use
1. Create a fresh Grok Bot
2. Paste the contents of `SETUP_PROMPT.md` as its first message
3. Connect the requested apps when prompted
4. Test with “Run a dry-run of the dividend calendar”

## Files
- `DESCRIPTION.md` — Bot identity and standing rules
- `skills/dividend-calendar/SKILL.md` — Main workflow
- `routines/weekly-refresh.md` — Suggested schedule
- `SETUP_PROMPT.md` — One-shot setup message for a fresh Bot
