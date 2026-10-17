---
name: setup
description: Help a new SmartDraftBoard user connect the plugin, choose a sport and scoring, and get a first verified free player comparison. Use for initial setup, getting started, installation help or testing whether the plugin works. Reuse an existing connection and user-provided context.
---

# First useful SmartDraftBoard result

Keep setup short. Aim for one real public comparison before suggesting account
linking or advanced workflows. Follow `../fantasy-analysis/SKILL.md` for scoring,
source dates, missing data and error recovery.

1. Inspect the tools advertised in this conversation. If `compare_players` is
   available, reuse the connection; do not ask the user to reinstall or create a
   website account. If it is absent, explain that it must be enabled and link to
   [guided setup](https://smartdraftboard.com/assistant-plugin#start-here). Ask
   which assistant they use only if the conversation does not establish it.
   Do not claim that opening the guide or installing a package verifies tools.
2. Reuse the user's sport, scoring and two players if provided. Otherwise ask
   one compact question for those details, offering a concrete example such as
   NBA ESPN points: Jalen Brunson versus Stephen Curry. For a user who explicitly
   asks for a demo, name that example and its scoring before using it. Do not
   silently assume the user's own league uses the example's settings.
3. Call `compare_players` once with the selected sport, scoring and players.
   Public comparisons are Free and do not require a SmartDraftBoard account.
   Search only if the identities need clarification. If the client requires
   authorization, use its browser flow; never request credentials in chat.
4. After an actual response, show a short result with the returned scoring,
   source dates, projection period and material missing data. Explain that the
   tool responded; only present a comparison supported by the evidence. A partial
   result confirms communication but does not establish current data coverage.
   An error or unavailable tool is not successful setup: follow the recovery
   instructions and keep any retry bounded. Never replace missing data with a
   generic model answer while claiming that SmartDraftBoard supplied it.
5. Offer one relevant next question using the same sport and scoring. Explain
   account linking only when the user wants saved league facts or Pro decisions.
   Saved facts require their linked account; full roster optimisation and
   roster-aware trades require current Pro. Do not call account/profile tools
   merely to run a public demo, and do not imply you can change their lineup.

If installation is needed, use the [Claude guide](https://smartdraftboard.com/guides/claude-fantasy-plugin)
or [ChatGPT guide](https://smartdraftboard.com/guides/chatgpt-fantasy-plugin).
ChatGPT custom MCP setup must be created and installed, then selected with `@`
in a Work chat. A custom MCP connection alone does not install this package's
skills. Packaged clients may offer a setup action; otherwise the user can ask
“Help me get started with SmartDraftBoard.”
