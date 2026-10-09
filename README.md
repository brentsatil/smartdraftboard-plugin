# SmartDraftBoard

Fantasy roster analysis, player comparisons and projections for NFL, NBA, FPL,
AFL and NRL. This package provides one shared skill and eleven read-only tools
for compatible MCP clients. The eleven public tools need no SmartDraftBoard
account or API key. Version 1.5 prepares optional account access; the protected
rollout remains disabled pending actual account/client verification. The v1.5
service and clean setup URL are deployed; directory approval is separate.

## Connect

Use **https://smartdraftboard.com/api/mcp** as a remote MCP server. Public tools
work anonymously. The server uses stateless Streamable HTTP and JSON responses.
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
SuperCoach; football supports FPL and supplied Fantrax EPL points rules. A public Sleeper league can supply its own supported
scoring and lineup settings. Ambiguous names require clarification.

## Version 1.4

Fantrax EPL and NBA settings are supported by comparisons, outlooks, rankings,
roster/trade analysis, weekly decisions and accuracy queries. Supply `platform:
"fantrax"` and complete `fantrax` scoring settings. NBA supports custom points
and category sets; EPL re-prices the GAMM stat line, with position-specific
weights and predicted minutes. Missing ghost stats withhold the full total.

EPL lineup analysis requires exact starter slots and player eligibility. It
uses no FPL captain multiplier. EPL season/trade projections and custom-scoring
uncertainty are not available; reference backtests do not validate league scoring.
The app's football Fantrax GAMM column and drawer use the same scorer. No
private Fantrax account access or MLB/NHL/NFL Fantrax integration is claimed.

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

The [setup guide](https://smartdraftboard.com/assistant-plugin) includes a
local question builder and an optional live player comparison.

## Limitations and data

Projections are estimates, not guarantees. Answers label data dates, source
credits, assumptions and unavailable information. Seasonal and weekly estimates
are distinct; an unavailable injury return date can prevent a confident trade
verdict. Incomplete matchup inputs do not produce a win probability. These tools
cannot alter lineups, submit trades, export bulk datasets or place bets.
Interactive MCP cards are not provided. Optional linked access is described below;
its availability follows the server rollout, not the package version.

Only send player names, scoring settings and public league references needed for
the task. Your assistant provider receives tool results under its own policies.
[Privacy](https://smartdraftboard.com/privacy-policy) ·
[Terms](https://smartdraftboard.com/terms) ·
[Support](https://smartdraftboard.com/contact) ·
[Setup guide](https://smartdraftboard.com/assistant-plugin)

## Review and packaging

Version 1.5.2 describes available functionality in the directory listing without
subscription wording. Version 1.5.1 limits the OpenAI manifest to the required five positive and three
negative review cases. `review-cases.json` retains the broader public regression
cases, including Fantrax coverage, and staged linked-access cases. The linked
walkthrough demonstrates the public integration in Claude, not OpenAI client
certification.
Public cases need no demo account. Protected cases require owner-controlled
synthetic Free/Pro accounts after authorized deployment. Use a public sample
league for Sleeper testing, or pasted names without any league connection.

Zip the contents of this directory, including hidden files, with `plugin.json`
at the archive root. Do not include application source, credentials, logs or
customer data. Each public directory has a separate review process; this package
alone does not publish a listing.

The MIT license applies to plugin files, not third-party data or the hosted service.

## Staged optional account access (1.5)

The server keeps `ASSISTANT_PRO_ENABLED` off by default. When disabled it lists
only the eleven public tools. After authorized deployment and client checks,
`get_assistant_profile` works for active linked Free accounts;
`list_my_leagues` requires current Pro membership and lists owned saved leagues
so the user can select one. `my_weekly_briefing` and `evaluate_my_trade` require
current Pro membership and an explicitly selected owned saved league. Upgrading after
linking does not require a new grant; each call checks current membership.
Expired or inactive membership, revoked/expired credentials, wrong ownership
and incomplete evidence do not unlock recommendations.

Link through the assistant client's browser authorization flow on SmartDraftBoard,
review the client origin and read-only scopes, then approve explicitly. Cancel
issues no grant. Revoke from Profile → Settings → Assistant connections; logging
out of the website clears website private state but does not revoke an assistant
grant. Revoke explicitly when disconnecting an assistant. Never paste provider
passwords, cookies, API keys, OAuth codes or tokens into chat.

Scopes are only `assistant:profile:read` and `assistant:leagues:read`. Public
clients use PKCE S256 and token endpoint authentication `none`. Resource audience
is `https://smartdraftboard.com/api/mcp`; operational routes are under
`/api/assistant/oauth/`. Codes last five minutes and are single use; access lasts
fifteen minutes. Refresh rotates, with thirty days inactivity and ninety days
absolute expiry. Refresh downscoping is unsupported: omit scope or repeat the
exact grant set. A smaller set is rejected without consuming the refresh token.

Saved recommendation support currently depends on verified NBA ESPN/Sleeper
roster, eligibility, rules, model and feed captures. Older saved leagues may need
refreshing on the website before recommendations are available. A DB write date
is not original source freshness. Other sport/provider combinations may provide
selection or partial saved facts; this package does not promise universal saved
recommendation support. Saved trade results can compare verified named package
values in the returned period/currency. Incoming platform eligibility remains
unverified: legal lineup impact, overall trade verdict and rest-of-season totals
are unavailable. A value comparison is not a full trade recommendation.

## Privacy and client verification

The assistant provider receives the selected tool inputs and returned account or
saved-league facts under its own policies. SmartDraftBoard records coarse events
and authorized grant use; it does not put tokens, provider credentials or roster
payloads into analytics. Only meaningful protected successes count as Pro use;
login, upgrade clicks, denials and all-unavailable answers do not.

Claude, ChatGPT and Codex are intended clients, subject to each host's plan,
workspace policy and MCP/OAuth support. Local HTTP/browser fixtures are verified;
this release package does **not** certify actual client sign-in, refresh, reconnect
or directory acceptance. Owner-controlled deployed client checks remain pending.
Package installation, draft PRs and local builds are not deployment or publication.
See [privacy](https://smartdraftboard.com/privacy-policy),
[support](https://smartdraftboard.com/contact) and the setup guide for help.
