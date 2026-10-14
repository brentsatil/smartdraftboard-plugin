---
name: nba-streaming
description: Compare one NBA fantasy pickup or add/drop with holding by usable daily starts, bench congestion and projected points. Use for an NBA waiver or streaming decision in a supported daily points league. Requires Pro and supplied league rules; excludes category optimization, custom points, weekly locks and Sleeper Lock-In or Game Pick optimization.
---

# NBA streaming decision

Use the advertised `plan_nba_streams` tool. Follow evidence and scoring rules in
`../fantasy-analysis/SKILL.md`. It compares one supplied add/drop with an optimized
no-move baseline; it cannot claim a player or set a lineup.

Before calling, establish only the missing decision-changing inputs:
- Exact scoring: `points` or default `espn_points`, not custom weights/categories.
- Daily lineups without games caps; do not infer this from ESPN, Fantrax or Sleeper
  branding. Sleeper counted-game formats cannot be modeled as every scheduled
  game contributing to a weekly total.
- Full active/bench roster and confirmed league eligibility for every player.
- Exact starter slots, repeated for each seat, excluding bench and IR.
- Available candidates and named acceptable drops, or a confirmed open spot.
- Any acquisition timing that affects eligibility to play. The planner covers
  future Eastern dates in its loaded NBA week, not same-day lock changes.

Use the current tool schema for argument names. Do not convert a Fantrax custom
points league into default ESPN points just to run the tool. For unsupported
formats, explain the boundary and offer a supported comparison with the actual
settings; do not claim it optimizes the requested format. Never use ownership
percentage to establish that a candidate is free in the user's league.

For a complete result, show the conditional add/drop or hold and a small table:
option, usable games, blocked games, lost drop starts and net projected points
versus holding. Include daily lineups when useful. Preserve the returned units,
source dates, waiver-clearance assumptions and start date. The baseline is an
optimized hypothetical lineup, not the user's already submitted lineup.

For missing schedules, projections, slots or eligibility, explain the gap and
withhold a winner. No schedule is not a zero-game week. Do not annualize the gain:
rest-of-season drop cost and multi-move sequences are outside this tool.
`PRO_REQUIRED` is a membership denial, not invalid input; use the returned safe
continuation and do not bypass it with public-tool calls. No betting or trades.
