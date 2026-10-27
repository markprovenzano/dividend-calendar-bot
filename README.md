# Dividend Calendar Bot

A Grok Bot template that projects upcoming dividend income from your holdings into a Google Sheet, with an optional Google Calendar.

## What it does
- Pulls holdings and recent dividend history from Finance or Robinhood, then public sources
- Projects future payments from the latest regular per-share payout and the inferred cadence
- Writes a private Google Sheet named "Dividend Calendar" (tabs: Summary, Payments, Monthly, Daily, Holdings) through Google Drive, so no browser sign-in is needed
- Optionally mirrors pay dates on a calendar you already have, rolled 3 months forward and updated in place
- Can run on a schedule (weekly refresh, in your timezone)

## Connectors
- Google Drive (required, for the Sheet)
- Finance **or** Robinhood Agentic (holdings)
- Google Calendar (optional)

Setup shows only the connect cards that are still missing.

## How to use
1. Create a fresh Grok Bot
2. Paste the contents of `SETUP_PROMPT.md` as its first message
3. Connect the requested apps when prompted
4. Test with “Run a dry-run of the dividend calendar”

## Optional local model
If you run a local model server, the bot can send text-only work (summaries and drafts) there. See `LOCAL_MODELS.md`. Data lookups, math, decisions, and writes stay in the bot. If no endpoint is set, the bot does that work itself.

## Files
- `DESCRIPTION.md` — Bot identity and standing rules
- `skills/dividend-calendar/SKILL.md` — Main workflow
- `routines/weekly-refresh.md` — Suggested schedule
- `SETUP_PROMPT.md` — One-shot setup message for a fresh Bot
- `LOCAL_MODELS.md` — Optional local-model offload
