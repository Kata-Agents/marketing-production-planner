# Marketing Production Planner

Plans video ad production from a script pack: what blocks the work, how it splits into tracks that cost different amounts and move at different speeds, what it will cost, the briefs for anything shot for real, and a release order with an approval gate on every unit.

It estimates before anything is committed, and it will not invent the numbers it does not have. Credit rates, resolution tiers and plan limits are asked for once rather than assumed, because a cost estimate built on a made-up unit price is a budget decision made on a fabrication.

Retakes get their own budget line. They are not waste — they are where the quality is, and a plan that budgets only the first attempt runs out at the point the work starts getting good.

It is a planner, not an orchestrator. It calls no generation API, submits no job and runs no render; it produces the plan, the queue, the shoot briefs and the assembly instructions, and a person or a downstream system executes them one approved unit at a time.

## What this is, precisely

A FindAgent **`mcp-tool`** agent. Each of its 6 tools is a
`prompt-template` action: the tool renders an instruction and hands it back to the
model that called it.

Two consequences worth being blunt about, because they decide whether this is useful to you:

- **It calls no model and reaches no network.** A tool call costs nothing and returns
  the same text for the same input, every time. There is no API key, no credential
  slot and no egress.
- **It observes nothing.** It has no access to your repository, your logs, your
  analytics or your devices. Every template is written so that supplying nothing
  produces an honest statement of what is missing rather than a confident-looking
  answer about data nobody provided. If you ask for a report and give it no findings,
  it will tell you the work has not been done — not invent it.

## Tools

| Tool | What it returns | Required input |
|---|---|---|
| `audit_production_readiness` | Audit what blocks production before anything is costed — missing reference assets, real-capture needs, talent and location, legal sign-off, and assets already owned. | `script_pack` |
| `break_down_work_tracks` | Split the work into the four tracks that differ in cost, owner and lead time — reference assets, generation, real capture, assembly — and order each one. | `script_pack` |
| `estimate_cost_and_time` | Produce the pre-approval estimate with its arithmetic shown, asking once for the unit prices rather than inventing them, and budgeting retakes as their own line. | `work_tracks` |
| `write_shoot_brief` | Write the brief and shot list for real capture — what must be genuine, what must be in frame, and the privacy pass on anything that will be shown to strangers. | `capture_modules` |
| `plan_release_gates` | Plan the release order with an approval gate on every unit, so a failure costs one unit rather than the batch, and name what stops the run. | `work_tracks` |
| `draft_production_plan` | Assemble the production plan — blockers, tracks, estimate, briefs, gates — refusing to produce a costed plan when no script pack or no prices were supplied. | `campaign_context` |

Optional inputs render as empty when omitted. Every template names that case and says
what it could not determine, so an empty slot degrades into a stated gap rather than a
dangling clause.

## Part of a department

This agent is one member of the **marketing video ad** department, a
hub-orchestrator team of 7. The hub is `marketing-brief-scoper`, which locks the brief every later
stage reads; the other members are
reached through it or called directly as `<alias>__<tool>`.

| Agent | Stage in the pipeline |
|---|---|
| `marketing-brief-scoper` | 1 — interviews for the brief and freezes it (department hub) |
| `marketing-ad-researcher` | 2 — competitor harvest plan, longevity ranking, customer voice, coverage |
| `marketing-angle-strategist` | 3 — scored angle map with auditable arithmetic |
| `marketing-hook-writer` | 4 — the modular creative bank, built on verbatim customer language |
| `marketing-ad-scripter` | 5 — modules, continuity kits, prompts, assembly map, QA protocol |
| `marketing-production-planner` | 6 — blockers, tracks, cost estimate, shoot briefs, release gates |
| `marketing-ad-tester` | 7 — clip QA, test design, readout, and the feedback loop back to 3, 4 and 5 |

Each member is published independently and works on its own.

## Provenance

`source/producer.md` is the markdown skill this agent was converted from. The tool templates carry its
instructions, parameterised: anything the original hard-coded to one team's repositories,
file paths or people became an input you supply, and where a template would otherwise
depend on reading something it cannot reach, it asks for that material as an argument
instead.

## Licence and use

Published by Kata Team on FindAgent. Free to connect.
