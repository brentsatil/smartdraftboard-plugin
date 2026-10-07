# SmartDraftBoard

Fantasy roster analysis, player comparisons and projections for NFL, NBA, FPL,
AFL and NRL. This package provides one shared skill and eleven read-only tools
for Claude and OpenAI-compatible plugin clients. No SmartDraftBoard account,
API key or OAuth is required.

## Connect

Use **https://smartdraftboard.com/api/mcp** as a remote MCP server, with no
authentication. The server uses stateless Streamable HTTP and JSON responses.
POST is supported; GET and DELETE intentionally return 405.

In Claude, add a custom connector with this URL under Customize → Connectors.
In ChatGPT, add a custom MCP server from your workspace's available plugin or
connector settings. Availability depends on the host's plan and administrator
settings. A custom connection does not imply approval in either public directory.

For Claude Code, clone this package and run:

```sh
claude --plugin-dir /absolute/path/to/smartdraftboard-plugin
```

Claude reads `.claude-plugin/plugin.json` and `.mcp.json`. Portable plugin clients
read `plugin.json` and `mcp.json`. Both use `skills/fantasy-analysis/SKILL.md`.

## Try it

- Compare Josh Allen and Patrick Mahomes in NFL PPR, with dates and uncertainty.
- Analyze my pasted roster and explain empty lineup seats.
- Show the top 10 NBA point guards in ESPN points scoring.
- Find Erling Haaland and show his FPL outlook.
- Show available FPL model accuracy and explain its limitations.

The tools are `analyze_roster`, `compare_players`, `player_outlook`, `rankings`,
`evaluate_trade`, `weekly_decisions`, `injury_report`, `latest_news`,
`search_players`, `find_sleeper_leagues`, and `model_accuracy`.

NFL scoring supports PPR, half PPR and standard. NBA supports points, ESPN points,
9-category and 8-category values. AFL supports SuperCoach and Fantasy; NRL uses
SuperCoach; football uses FPL. A public Sleeper league can supply its own supported
scoring and lineup settings. Ambiguous names require clarification.

## Version 1.3

`model_accuracy` accepts scoring and platform. AFL Fantasy has its own benchmark;
FPL Classic, Draft and default Fantrax remain scoring aliases. NFL and NBA host
aliases share PPR and classic-points reference benchmarks, explicitly labelled
when a requested format lacks a separate evaluation. Custom/category scoring
cannot be validated by converting aggregate point error.

New backtest runs record per-stat MAE, RMSE, signed bias and paired sample counts,
including position subsets. Missing forecasts/actuals are excluded, real zeros
are counted, and RMSE pools squared errors. Old rows without counted diagnostics
are labelled unavailable. Published seasonal fits are not untouched holdouts.

FPL attacking signals now account for expected minutes and double gameweeks.
A frozen-default, four-season test measured 0.45% lower MAE over 40,383 forecasts;
reserved confirmation seasons improved 0.37%. This is a modest FPL-specific
historical gain, not proof of future or other-sport improvement.

## Version 1.1

Pasted NFL/NBA rosters can include exact `lineupSlots`, including SUPERFLEX and
NBA G/F/UTIL. The result identifies whether seats came from a connected league,
your input, or a default. Screenshots from ESPN/Yahoo can be transcribed by the
assistant; private platform URLs are not authenticated integrations.

Comparisons include availability-adjusted expected scores, all fixtures, expert
rank scope and draft ADP where available. Missing projections prevent an overall
start/sit winner. Head-to-head probabilities are explicitly uncalibrated model
approximations, not guarantees.

Backtests expose evaluation periods, sample sizes and computation dates. Baseline
improvement requires matching periods and counts, and is withheld for zero
baseline error. MAE, interval coverage and outcome percentiles are defined
separately. Aggregate matching is not proof of identical players or significance.

The [setup guide](https://smartdraftboard.com/assistant-plugin.html) includes a
local question builder and an optional live player comparison.

## Limitations and data

Projections are estimates, not guarantees. Answers label data dates, source
credits, assumptions and unavailable information. Seasonal and weekly estimates
are distinct; an unavailable injury return date can prevent a confident trade
verdict. Incomplete matchup inputs do not produce a win probability. These tools
cannot alter lineups, submit trades, access private leagues, export bulk datasets,
or place bets. OAuth and interactive MCP cards are not part of this release.

Only send player names, scoring settings and public league references needed for
the task. Your assistant provider receives tool results under its own policies.
[Privacy](https://smartdraftboard.com/privacy-policy) ·
[Terms](https://smartdraftboard.com/terms) ·
[Support](https://smartdraftboard.com/contact) ·
[Setup guide](https://smartdraftboard.com/assistant-plugin.html)

## Review and packaging

`review-cases.json` contains five positive and three negative review scenarios.
No demo account or password is required. Use an owner-controlled public sample
league for Sleeper testing, or pasted names without any league connection.

Zip the contents of this directory, including hidden files, with `plugin.json`
at the archive root. Do not include application source, credentials, logs or
customer data. Each public directory has a separate review process; this package
alone does not publish a listing.

The MIT license applies to plugin files, not third-party data or the hosted service.
