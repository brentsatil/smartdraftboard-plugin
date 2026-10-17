# Tool workflows for Claude and ChatGPT

Connect `https://smartdraftboard.com/api/mcp`, enable SmartDraftBoard in the
conversation, and inspect an actual tool result. These examples are reusable
inputs, not recommendations about current players. Use only advertised tools.

## A first useful result

For known names and scoring, call `compare_players` directly:

```json
{"sport":"nba","scoring":"espn_points","players":["Jalen Brunson","Stephen Curry"]}
```

Show the returned decision only when its evidence is complete. Include the
projection period, scoring, original source dates and material missing data.
A public comparison is Free; it does not verify the user's whole lineup.

## Find an uncertain player

Call `search_players` with the known sport to avoid unrelated results:

```json
{"query":"J. Allen BUF","sport":"nfl","limit":3}
```

Search supports source aliases and small spelling errors. `matchType` is `exact`,
`prefix`, `substring` or `suggestion`. Confirm suggestions and ambiguous names
with the user before analysis. Names are ranked by identity relevance, never
fantasy value. Search projections use default scoring, so follow with an outlook
or comparison in the requested scoring. Do not iterate searches to export data.

## Scoring that matches the league

| Sport argument | Supported public scoring values |
| --- | --- |
| `nfl` | `ppr`, `half_ppr`, `standard` |
| `nba` | `points`, `espn_points`, `9_cat`, `8_cat` |
| `football` | `fpl` |
| `afl` | `supercoach`, `fantasy` |
| `nrl` | `supercoach` |

For Fantrax NBA/EPL, omit `scoring`, set `platform` to `fantrax`, and provide the
complete non-secret `fantrax` scoring settings. `scoring: "fantrax"` is invalid.
Do not omit unmodelled categories to make a projection appear complete. A public
Sleeper league supplies its own supported scoring and starter slots.

## A failed call should lead to one useful next step

Service 1.9 returns the same JSON in text and structured error content:

```json
{"status":"error","error_code":"UNSUPPORTED_SCORING","data":null,"retryable":false,"recovery":{"action":"correct_input","message":"Correct the stated input before calling again."},"details":{"sport":"nfl","supportedScoring":["ppr","half_ppr","standard"]}}
```

The example omits the response summary and help links. SDK input-schema and
transport errors may retain the client's standard MCP error shape.

| Error code | Next action |
| --- | --- |
| `AMBIGUOUS_PLAYER`, `PLAYER_NOT_FOUND` | Ask the user to clarify using `details.unresolved` and its candidates. |
| `UNSUPPORTED_SCORING`, `INVALID_ARGUMENTS` | Correct the stated input; ask if the intended settings are unclear. |
| `AUTH_REQUIRED`, `INSUFFICIENT_SCOPE` | Complete the client's browser OAuth challenge. |
| `QUOTA_EXCEEDED`, `TOOL_FAILED` | Wait `recovery.retryAfterSeconds`, retry once, then stop if it fails. |
| `DISABLED` | Use the website; reconnecting does not enable the tool. |
| `PRO_REQUIRED` | Explain the separate membership denial and its upgrade link; do not retry. |

Free linked accounts can list their own saved leagues and read saved facts. Pro
is required for roster optimisation, roster-aware trade analysis, supported saved
recommendations and NBA streaming. Read-only tools cannot refresh providers,
change a lineup or submit a trade. Never paste passwords, tokens or cookies.

## Client evaluation checklist

Run the cases in `review-cases.json` in each client after service deployment.
Record the tool selected, arguments, result, source dates and recovery behavior.
Use the admin MCP Traffic view to confirm client attribution and request/response
previews. A local fixture pass does not certify either client's model behavior,
OAuth flow, installed tool cache or marketplace acceptance.

Guidance: [OpenAI tool requirements](https://developers.openai.com/plugins/plugin-guidelines)
and [connect and test a ChatGPT plugin](https://developers.openai.com/plugins/deploy/connect-chatgpt).
