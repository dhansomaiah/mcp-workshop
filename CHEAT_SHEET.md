# Cheat sheet

## Restart the server after editing `server.py`

Press `Ctrl+C` in the terminal running the Inspector, then re-run:

```bash
npx @modelcontextprotocol/inspector python server.py
```

## Reconnect the Inspector

If the Inspector loses connection after a restart, click **Connect** in the top-left of its UI.

## Common errors

**`ModuleNotFoundError: No module named 'mcp'`**
Run: `pip install -r requirements.txt`

**`FileNotFoundError: components.csv`**
Run `python server.py` from the project root, not from `data/`.

**Tool doesn't show up in the Inspector after adding it**
Restart the server. The tool list is captured at the handshake, not live.

**AI says "I don't have a tool for that"**
The description string is what the AI reads. If it doesn't mention the concept the user asked about, the AI will not pick the tool. Rewrite the description.

**Codespace feels slow to boot**
First boot on a fresh repo can take 60–90 seconds. Subsequent boots are ~10 seconds.

## MCP Inspector — where things are

- **Tool list:** left sidebar → **Tools** tab
- **Call a tool:** click it, fill the form, hit **Call Tool**
- **See the raw JSON going over the wire:** **History** tab at the bottom — this is your "signal trace"

## Editing `server.py` in Codespaces

- Open the file from the left file explorer.
- Save with `Ctrl+S`.
- Copilot suggestions appear as grey text — press `Tab` to accept.
