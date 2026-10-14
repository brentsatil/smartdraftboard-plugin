# Connect SmartDraftBoard to Claude

Server URL: **https://smartdraftboard.com/api/mcp**

## Claude web and desktop

1. Open Customize → Connectors → Yours and check for SmartDraftBoard first.
   If it is already connected, use it instead of creating a duplicate.
2. If absent, choose Add connector → Add custom connector, name it
   SmartDraftBoard and enter the server URL above.
3. Choose sign-in when needed. OAuth supports automatic client registration and
   Claude's published client identity. Leave fixed headers, client secrets and
   API keys empty. Public research can run without linking an account.
4. For saved-league facts or Pro decisions, use the browser authorization flow
   on smartdraftboard.com. Review the Claude client and the two read-only scopes:
   `assistant:profile:read` and `assistant:leagues:read`. Approve only your account.
5. Enable SmartDraftBoard for the conversation. Ask “Find Erling Haaland in
   SmartDraftBoard and show his FPL outlook with the source dates.” Verify an
   actual SmartDraftBoard tool call; a plausible written answer is not a test.
6. For linked access, ask to list your saved leagues, then explicitly select one.
   Free supports saved facts; current Pro is required for advanced decisions.

## Claude Code

Install the published plugin, or load a local copy with:

```sh
claude --plugin-dir /absolute/path/to/smartdraftboard-plugin
```

For the connector alone, without the plugin's skills:

```sh
claude mcp add --transport http smartdraftboard https://smartdraftboard.com/api/mcp
```

Use `/mcp` to inspect connection status and follow its browser authentication
flow when needed. Do not add a duplicate standalone server if the plugin already
provides it. Never place tokens or provider credentials in the manifest.

## If something does not work

- Missing tools: check the connected server's current inventory and refresh the
  connector in Claude. A package version does not establish hosted capabilities.
- Sign-in required: use OAuth in the browser, not secrets pasted into chat.
- `PRO_REQUIRED`: linking worked but the advanced operation needs current Pro.
  Reconnecting or changing player names cannot change membership.
- Stale saved league: refresh/import in the SmartDraftBoard website, then read it
  again. The connector cannot refresh ESPN, Sleeper, Fantrax or Yahoo itself.
- Incomplete or unsupported result: retain its stated reasons and scoring format;
  do not substitute a different platform or count missing data as zero.
- Temporary busy/rate limit: wait for the returned retry delay; do not loop.
- Disconnecting: revoke the grant in Profile → Settings → Assistant connections
  on SmartDraftBoard, and disconnect in Claude. Website logout alone does not revoke
  the assistant grant.

All tools are read-only. An installed plugin, a working personal connector and a
published connector-directory listing are separate states. Directory acceptance
is controlled by Anthropic; custom connections can work while review is pending.

Official setup: https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp
