# MCP Workshop

Build your first MCP server in 30 minutes.

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/dhansomaiah/mcp-workshop)

## What's inside

A tiny MCP server that serves synthetic embedded-flavored data — fake components (like `CM101A`) and fake NTC fault codes — and a matching client that talks to it. You will add tools to the server during the workshop.

## Quick start (in Codespaces)

1. Click **Open in Codespaces** above.
2. Wait ~90 seconds for the environment to build.
3. In the terminal, run:

   ```bash
   python client.py
   ```

4. You should see three sections in the output:
   - `[handshake]` — connected
   - `[list_tools]` — the two tools the server exposes
   - `[call_tool]` — the JSON result of `lookup_component("CM101A")`

That's the entire MCP loop. The workshop tasks are small edits to it.

## What's here

- `server.py` — the MCP server with two working tools
- `client.py` — a minimal MCP client: connects, lists tools, calls one
- `data/` — synthetic components and NTCs
- `HANDS_ON.md` — the two tasks for the workshop
- `CHEAT_SHEET.md` — restarts, common errors, optional MCP Inspector GUI
- `SOLUTION.md` — peek only if Copilot stalls

## The mental model

Think of an MCP server as a **toolbox with labeled drawers**. Each tool is a drawer. Its description string is the label taped to it. The client (or an AI) reads the labels to decide which drawer to open.

**Your job as a tool author:** write labels the AI can act on.

## What you will build

See `HANDS_ON.md`.
