---
name: fantasy-analysis
description: Analyze fantasy rosters, compare players, evaluate trades and read projections, injury reports or news with SmartDraftBoard for NFL, NBA, Premier League FPL, AFL and NRL. Use for a pasted roster, a roster screenshot or a user-supplied public Sleeper league. Excludes betting, account changes and placing roster transactions.
---

# SmartDraftBoard fantasy analysis

Use the connected SmartDraftBoard MCP tools for the requested fantasy decision.
One-off analysis needs no SmartDraftBoard account. The tools read data and return
advice; they cannot set a lineup, execute a trade, claim a player or save a league.

## Choose the question and tool

| User's question | Tool |
| --- | --- |
| Who is this player? | `search_players` |
| Which of these 2–4 players should I start? | `compare_players` |
| Tell me about one player | `player_outlook` |
| Rank a position for this period or the season | `rankings` |
| Set out my best legal lineup and alternatives | `analyze_roster` |
| Is this trade useful for my roster? | `evaluate_trade` |
| Which of my Sleeper leagues should I use? | `find_sleeper_leagues` |
| Captain or streaming choices across the player pool | `weekly_decisions` |
| Current availability designations | `injury_report` |
| Recent headlines and original links | `latest_news` |
| How well has the model predicted scores? | `model_accuracy` |

Establish sport and scoring from the conversation. Use `football` for FPL.
Ask only for missing details that change the decision. If using a default scoring
system, name it; a connected Sleeper league supplies its own rules and seats.
Do not infer a league from the user's identity or browse other people's leagues.
Use only the public username or league the user provides for this analysis.
A league ID without a username may need the username to identify their team.
If several leagues are returned, ask the user to choose.

For a screenshot, read the visible names and use `search_players` to resolve
unclear text. Report ambiguous or unmatched names and ask for the team or a
correction. Never silently omit players or replace them with plausible names.
Supply each player once. Pass the full roster when measuring trade lineup impact.
For pasted NFL/NBA rosters (including ESPN or Yahoo screenshots), read the starting
slots and pass `lineupSlots`, repeating each seat: QB, RB, RB, WR, WR, TE, FLEX,
SUPERFLEX, for example. Exclude bench, IR and taxi seats. If slots are missing,
ask when they change the answer, or clearly identify the default formation from
`lineupRules`. A public Sleeper league's actual seats remain authoritative.
Private ESPN/Yahoo URLs are not an authenticated integration: ask for a pasted
roster or screenshot instead of credentials or pretending to access the account.

## Read the answer correctly

- If `decision.status` is incomplete, do not announce an overall winner. Explain
  which projection is absent; compare only the supported facts.
- Separate `thisPeriod.points` (model projection) from `expectedPoints` (adjusted
  for modeled availability). An injury probability is a model convention, not a
  medical certainty. Read all `fixtures`, especially NBA multi-game weeks and
  FPL doubles; the compact opponent label is only the first fixture.
- Lead with the decision and the numerical reason. Quote `asOf` as the underlying
  data date. If null, say the data's timestamp is unavailable; do not use today's
  date as a substitute. Mixed-sport search rows carry their own timestamps.
- Preserve `unavailable` and relevant `notes`. A missing score, schedule or report
  is not zero, a bye, a healthy player or proof that no news exists.
- Distinguish this period's projection from a season average and off-season
  historical data. Use the returned `basis` and `currency`; do not relabel a
  per-game average as a weekly total. NBA category z-sums are signed values,
  not fantasy points or category win probabilities.
- Use week for NFL/NBA, gameweek for FPL, and round for AFL/NRL. Follow the returned
  period instead of assuming that every sport is currently playing.
- Floors and ceilings are the stated 25th/75th percentile estimates, not minimum
  and maximum possible scores. Preserve model limitations and approximations.
  A 52% head-to-head edge is a coin flip. Availability and lineup independence
  assumptions do not make a forecast a guarantee.
- Explain expert ranks and ADP only within their returned scope. A season rank
  is not a weekly rank, and ADP is a draft-market measure. Analyst panel size is
  not a count of independent data providers. Agreement is not forecast accuracy.
- For reliability questions, inspect `model_accuracy`. MAE is an average absolute
  error, not a ± interval or a head-to-head win probability. Report sample size,
  evaluated seasons/rounds, scoring and missing position-specific coverage.
  Only use `improvementPct` when present: unmatched periods/counts and a zero
  baseline error prevent a valid percentage. Matched aggregate counts still do
  not establish identical player cohorts or statistical significance.
- Credit SmartDraftBoard and the returned `sources`. Link original news URLs and
  useful `links` from the answer. Do not reproduce full articles or expose named
  ESPN analysts from an anonymised consensus.
- Request answer-sized lists within the tools' limits. Do not cycle through
  queries to export the underlying player dataset.
- On a tool error, explain the correction or missing data. A busy response gives
  a retry delay; do not repeatedly call it within that interval.

Stay within fantasy analysis. Do not derive sportsbook wagers, ATS picks, betting
stakes or pick'em betting recommendations from these tools. Never ask for fantasy
platform passwords, cookies, API keys or private account credentials. If the
connector is unavailable, explain that live SmartDraftBoard analysis requires its
connection; do not fabricate a tool result.
