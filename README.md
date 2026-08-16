# MCP Workshop

Build your first MCP server in 30 minutes.

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/dhansomaiah/mcp-workshop)

## What's inside

A tiny MCP server that serves synthetic embedded-flavored data — fake components (like `CM101A`) and fake NTC fault codes — plus a `.vscode/mcp.json` that hands it to GitHub Copilot Chat's Agent mode as a tool source. You'll add tools during the workshop and ask Copilot to use them.

## Quick start (in Codespaces)

1. Click **Open in Codespaces** above.
2. Wait ~90 seconds for the environment to build (`pip install` runs automatically).
3. Open the **Chat** panel on the right, set mode to **Agent**, click the **tools icon** — you should see two tools from `component-workshop`.
4. Ask Copilot:
   > **who owns component CM101A?**

   Copilot picks `lookup_component`, calls it, and answers.

That's the whole loop. The workshop tasks are edits to it — see `HANDS_ON.md`.

## What's here

- `server.py` — the MCP server, two working tools
- `.vscode/mcp.json` — registers the server with Copilot Chat's Agent mode
- `data/` — synthetic components and NTCs
- `client.py` — optional stdio client, prints raw wire messages (no AI)
- `HANDS_ON.md` — the two workshop tasks
- `CHEAT_SHEET.md` — restarts, common errors, troubleshooting
- `SOLUTION.md` — peek only if Copilot stalls
- `PLAN.md` — session plan and decision log (mostly for the presenter)

## The mental model

An MCP server is a **toolbox with labeled drawers**. Each tool is a drawer. The tool's description string is the label taped on it. The AI reads the labels to decide which drawer to open.

**Your job as a tool author:** write labels the AI can act on.

## What you will build

See `HANDS_ON.md`.
