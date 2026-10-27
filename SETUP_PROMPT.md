Read the design in this repository: https://github.com/markprovenzano/dividend-calendar-bot

Do the following:
1. Set your identity and standing rules from DESCRIPTION.md. Ask whether to rename this bot. Recommend "Dividend Calendar" only if I agree; otherwise keep the existing name.
2. Install the skill from skills/dividend-calendar/SKILL.md into your skill library.
3. Check which connectors are already connected. Show connect cards only for the ones still missing:
   - Holdings: Finance (preferred) or Robinhood Agentic.
   - Output: Google Drive (for the Sheet).
   - Google Calendar, only if I want calendar events.
   Never ask me to sign in to Google in a browser.
4. Ask, one question at a time, with these defaults: Sheet only (or Sheet plus Calendar); all accounts combined (default), or specific accounts; project from the latest payout; calendar horizon of 3 months if I want a calendar. The Sheet always covers 12 months. If I choose specific accounts, list the accounts from the holdings connector and let me pick.
5. If I want a calendar: use a calendar I already have named "Dividends" if one exists. Otherwise ask me to make one, or use my primary calendar. Connectors cannot create calendars. Save the chosen calendar id.
6. Ask for my timezone for scheduled routines. If I do not provide one, ask again before creating the routine; do not guess.
7. Ask whether I have a local model endpoint (optional). If I do, store the URL and model as bot secrets or env (`LOCAL_LLM_URL`, `LOCAL_LLM_MODEL`) — never in the repo — and follow LOCAL_MODELS.md. If I have none, skip this.
8. Create the weekly refresh routine described in routines/weekly-refresh.md, in my timezone.
9. Confirm what you set up and list any missing pieces.
10. Do not write to the Sheet or Calendar until I say so.

After setup, wait for my next instruction.
