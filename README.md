# MCP Workshop

Build your first MCP server in 30 minutes.

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/dhansomaiah/mcp-workshop)

## What's inside

A tiny MCP server that serves synthetic embedded-flavored data — fake components (like `CM101A`) and fake NTC fault codes. You will add tools to it during the workshop.

## Quick start (in Codespaces)

1. Click the **Open in Codespaces** button above.
2. Wait ~60 seconds for the environment to build.
3. In the terminal, launch the MCP Inspector — it will start the server for you:

   ```bash
   npx @modelcontextprotocol/inspector python server.py
   ```

4. Open the Inspector URL it prints, click **Tools**, click `lookup_component`, enter `CM101A`, hit **Call Tool**.

## What's here

- `server.py` — the MCP server with two working tools
- `data/` — synthetic components and NTCs
- `HANDS_ON.md` — the two tasks for the workshop
- `CHEAT_SHEET.md` — restart commands, common errors, MCP Inspector tips
- `SOLUTION.md` — peek only if Copilot stalls

## The mental model

Think of an MCP server as a **toolbox with labeled drawers**. Each tool is a drawer. Its description string is the label taped to it. The AI reads the labels to decide which drawer to open.

**Your job as a tool author:** write labels the AI can act on.

## What you will build

See `HANDS_ON.md`.
