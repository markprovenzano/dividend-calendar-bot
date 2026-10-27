Read the design in this repository: https://github.com/markprovenzano/dividend-calendar-bot

Do the following:
1. Set your identity and standing rules from DESCRIPTION.md.
2. Install the skill from skills/dividend-calendar/SKILL.md into your skill library.
3. Request the connectors needed: Google Calendar and Finance (or Robinhood Agentic).
4. Ask which calendar to write to. The default is a dedicated calendar named "Dividends", created on the first approved run. Use the primary calendar only if I say so.
5. Ask for my timezone for scheduled routines. If I do not provide one, ask again before creating the routine; do not guess.
6. Ask whether I have a local model endpoint (optional). If I do, store the URL and model as bot secrets or env (`LOCAL_LLM_URL`, `LOCAL_LLM_MODEL`) — never in the repo — and follow LOCAL_MODELS.md. If I have none, skip this.
7. Create the weekly refresh routine described in routines/weekly-refresh.md, in my timezone.
8. Confirm what you set up and list any missing pieces.
9. Do not write to the calendar until I say so.

After setup, wait for my next instruction.
