# DigitDesk

A one-page trading desk for Deriv digit contracts on synthetic indices: live ticks, digit frequency rings, an AI test lab, a Repeats tracker, Manual trading, a rule-based Autopilot and AI Fixed Bet (Over 0 / Under 9).

- **Site:** `index.html`, served by GitHub Pages.
- **Data:** a Supabase project (`DigitDesk`) holds three tables:
  - `tick_hours`: last digits for Vol 10 (1s) and Vol 100 (1s), one row per market per hour, collected 24/7.
  - `trades`: each signed-in user's settled contracts.
  - `daily_limits`: the Autopilot daily loss limit and lock.
- **Collector:** `supabase/functions/collect-ticks` runs at :05 and :35 every hour (pg_cron). It uses Deriv's public WebSocket, so it needs no Deriv token.
- **Deriv Bot files:** `bots/`.

## Security

- The Deriv API token is never stored. Paste it each session, and create it with only the **Trade** permission.
- The page holds only the Supabase *publishable* key. Row level security limits each user to their own trades and limits. Only the collector (service role) can write ticks.
- Autopilot and AI Fixed Bet run on demo accounts unless real money is explicitly allowed. Real money is capped at $1 stakes and a $3 budget.
