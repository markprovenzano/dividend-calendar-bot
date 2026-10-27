# Dividend Calendar Bot

A Grok Bot template that keeps an up-to-date Google Calendar of upcoming dividend payments for your holdings.

## What it does
- Pulls current holdings and past dividends from Finance or Robinhood
- Looks up upcoming ex-dividend / pay dates on the web, or estimates them from past payment frequency
- Creates or updates events on a "Dividends" calendar, matched by a stable `div-key`
- Can run on a schedule (weekly refresh, in your timezone)

## Connectors required
- Google Calendar
- Finance **or** Robinhood Agentic (or both)

## How to use
1. Create a fresh Grok Bot
2. Paste the contents of `SETUP_PROMPT.md` as its first message
3. Connect the requested apps when prompted
4. Test with “Run a dry-run of the dividend calendar”

## Optional local model
If you run a local model server, the bot can send text-only work (digest summarizing, drafting event descriptions) there. See `LOCAL_MODELS.md`. Data lookups, decisions, and calendar writes stay in the bot. If no endpoint is set, the bot does that work itself.

## Files
- `DESCRIPTION.md` — Bot identity and standing rules
- `skills/dividend-calendar/SKILL.md` — Main workflow
- `routines/weekly-refresh.md` — Suggested schedule
- `SETUP_PROMPT.md` — One-shot setup message for a fresh Bot
- `LOCAL_MODELS.md` — Optional local-model offload
