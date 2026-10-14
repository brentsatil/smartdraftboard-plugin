---
name: league-briefing
description: Read a user's linked SmartDraftBoard saved league, explain its scoring and roster facts, or request its supported Pro weekly briefing or named trade values. Use when the user asks about my saved league, my team or a league already selected in this conversation. Requires advertised account tools; never guesses the league or changes a roster.
---

# Saved-league briefing

Use SmartDraftBoard's connected account tools only when they are advertised.
Follow the evidence and scoring rules in `../fantasy-analysis/SKILL.md`.

1. If the user has already explicitly selected a saved league in this conversation,
   reuse that selection. Otherwise call `list_my_leagues`; show short names and
   sport/platform and ask the user to select when several match. Do not infer
   their team from account identity, a public username or the first result.
2. Call `read_my_league` with the selected `leagueId` for roster, scoring and
   eligibility facts. These reads are available to active linked Free accounts.
   Keep the league's returned rules; do not add scoring or ownership overrides.
3. For requested recommendations, call `my_weekly_briefing` with the same ID.
   For a named saved-league trade, call `evaluate_my_trade` with that ID and the
   exact give/get names. Both need current Pro. Reading saved facts does not
   prove that a supported recommendation is available.
4. Honor `context`, original source dates, capability reasons and completeness.
   NBA ESPN/Sleeper captures are the current verified saved calculation boundary;
   Fantrax or another sport may return facts with unavailable recommendations.
   If captures are stale or rules/eligibility are missing, explain the exact gap
   and direct the user to refresh the league in the website before trying again.
   These MCP tools read saved evidence; they cannot refresh the provider.
5. Answer with the selected league and period, the supported facts or decision,
   and the data date plus material gaps. Trade package value differences are not
   legal lineup impact, rest-of-season value or an overall trade verdict.

On an authentication challenge, use the client's browser sign-in. Never request
passwords, tokens, codes, cookies or private provider IDs in chat. On
`PRO_REQUIRED` or `status: "blocked"`, explain the returned continuation and stop
that advanced request. Do not reconstruct a denied decision with repeated public
queries. Public research remains available if the user wants it separately.

Do not export all saved leagues, fetch unrelated league details, change a lineup,
submit a transaction or turn a fantasy recommendation into betting advice.
