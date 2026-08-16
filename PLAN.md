# Workshop plan (source of truth — survives Claude context compaction)

> This file exists so any future Claude session can rebuild the plan without
> re-litigating decisions. Update when direction changes.

## Audience & constraints

- **Attendees:** embedded C engineers with **no software / scripting** background.
  Some have used a MATLAB MCP client. All have GitHub Copilot (tier: some free,
  some Enterprise — TBD; may login via GitHub Enterprise tomorrow).
- **Time:** 30–45 minutes, walking in cold (no pre-work).
- **Presenter:** Dhan Somaiah, personal GitHub `dhansomaiah`, no company repo
  access for this workshop, no premium Copilot.
- **Delivery:** GitHub Codespaces on attendees' own accounts, opened from
  `https://github.com/dhansomaiah/mcp-workshop`.
- **Slides:** Gamma.app, presenter builds them (never generate `.pptx`).

## Session arc (fixed)

1. **APIs fail agents (5 min)** — one visual: agent trying to call REST endpoints,
   4 fail-points labeled (endpoint / auth / params / response parse).
2. **What MCP is (5 min)** — two visuals: signal-trace handshake diagram, tool
   anatomy mapped to C function + doxygen.
3. **Live demo (5 min)** — presenter drives on projector: server up, client
   queries, then edit a description, re-query, see behavior shift.
4. **Hands-on (15–20 min)** — Task 1: relabel a drawer (edit description).
   Task 2: add a new tool `find_component_by_family`, query it.
5. **Wrap (3–5 min)** — links, one caution ("tools = sudo"), Q&A.

## Visual axiom (fixed)

**Data flow + message passing.** Every visual answers "who's talking, what's on
the wire, in what order." Diagrams drawn like CAN / UART traces.

**Hero metaphor:** MCP server = **toolbox with labeled drawers**. Tool
description = label taped on the drawer. The AI (or client) reads the label.
"The description IS the interface for the AI." Reused 4× across the deck.

## Client-side path (CURRENTLY OPEN — see decision log)

The workshop needs a **queryable, interactive** client — attendees must be able
to type different questions and see different tool calls happen. A one-shot
script that prints hardcoded output is not sufficient.

Options considered (see decision log for what's been ruled out):

- **Q1. GitHub Copilot Chat in Codespaces with MCP config** — best UX (natural
  language chat, matches real work). Depends on their Copilot tier supporting
  agent mode / MCP servers. UNVERIFIED on free tier and on Enterprise.
- **Q2. MCP Inspector with harder-tried config** — queryable UI (pick tool,
  fill form, submit). We failed with `ALLOWED_ORIGINS` alone; next tries:
  `HOST=0.0.0.0`, `DANGEROUSLY_OMIT_AUTH=true`, or pin to a pre-v2 Inspector
  version (v0.10–v0.14) that predates the DNS-rebinding origin check.
- **Q3. A tiny stdio REPL** (`repl.py` prompting for tool + args) — reliable
  but ugly and unnatural.
- Q4 (rejected). Custom Flask/Streamlit UI — overengineered.
- Q0 (rejected). Hardcoded `client.py` script — not queryable.

## Setup that exists (repo state)

Path: `C:\Users\MZZ3YE\source\repos\mcp-workshop`
Remote: `https://github.com/dhansomaiah/mcp-workshop` (public, main branch)

Files:
- `.devcontainer/devcontainer.json` — Python 3.11 + Node feature, runs
  `pip install -r requirements.txt`, forwards port 6274.
- `server.py` — FastMCP server, two tools (`lookup_component`,
  `list_ntcs_for_component`). **BROKEN** as of 2026-08-16: `from mcp.server.fastmcp
  import FastMCP` fails on current PyPI `mcp` package. Need to pin version or
  switch import path. Must be re-verified inside the Codespace before pushing.
- `client.py` — hardcoded stdio client that calls `lookup_component("CM101A")`.
  Not queryable. To be replaced or supplemented.
- `data/components.csv` — 20 synthetic components (id, name, owner, family,
  subsystem). Fields include realistic-looking IDs like CM101A, ES249B, AR200A.
- `data/ntcs.json` — NTC fault codes keyed by component id.
- `README.md`, `HANDS_ON.md`, `CHEAT_SHEET.md`, `SOLUTION.md`,
  `requirements.txt`, `.gitignore`.

## Decision log

| Date       | Decision                                                       | Why                                                                         |
|------------|----------------------------------------------------------------|-----------------------------------------------------------------------------|
| 2026-08-16 | Public repo on `dhansomaiah` personal GitHub                    | No company repo access; personal is the only option                          |
| 2026-08-16 | Language: Python + FastMCP                                     | Simplest for non-scripters; Copilot autocompletes it well                    |
| 2026-08-16 | Synthetic embedded-flavored data (components + NTCs)           | Resonant for the audience without any real proprietary data                  |
| 2026-08-16 | Removed presenter's name from synthetic component owners        | Presenter preference                                                        |
| 2026-08-16 | Phase 5 = just links + Q&A                                      | Presenter does not want to show real MCP or point attendees at it            |
| 2026-08-16 | MCP Inspector attempted first as queryable client              | Familiar official tool, visual                                              |
| 2026-08-16 | Inspector abandoned mid-test — 403 origin block in Codespaces  | `ALLOWED_ORIGINS=...-6274.app.github.dev` set but proxy still rejects; may  |
|            |                                                                | be Inspector v2.2 bug (see issues #950, #1276)                              |
| 2026-08-16 | Pivoted to `client.py` stdio script                            | Zero networking issues; works under Enterprise policies too                  |
| 2026-08-16 | REJECTED `client.py` alone as workshop client                  | Not queryable — presenter's audience needs to explore, not re-run a script  |
| 2026-08-16 | `mcp.server.fastmcp` import fails on current PyPI `mcp` pkg    | Need to pin version — must verify in Codespace before pushing again          |

## Rules for future Claude turns

1. **Never push code without first running it in the target environment**
   (the Codespace, not just locally on Windows). Use `pip install` + `python`
   in the Codespace terminal to verify imports resolve.
2. **Trust the presenter's pedagogy calls** — they know the audience. When they
   push back on a design (e.g. "not queryable"), pivot, don't defend.
3. **Personal repo only** — never point attendees at any company data or the
   presenter's real MCP server.
4. **Stdio over network** — every network step is a failure mode. Prefer stdio
   client patterns unless a network client is strictly necessary.
5. **Update this file** when a decision is made or reversed. This is the
   context-loss backstop.
