# Cheat sheet

## Restart after editing anything

Just re-run:

```bash
python client.py
```

`client.py` spawns a fresh server subprocess every run. There is nothing to kill first.

## Common errors

**`ModuleNotFoundError: No module named 'mcp'`**
Run: `pip install -r requirements.txt`

**`FileNotFoundError: components.csv`**
Run from the project root (`/workspaces/mcp-workshop`), not from `data/`.

**Client hangs and prints nothing**
The server crashed at startup — usually a Python syntax error in `server.py`. Test the server alone:
```bash
python server.py
```
If you see a traceback, that's your bug. If it starts and sits silently, it's fine (stdio servers speak on stdin/stdout, not the terminal).

**Tool doesn't appear in `[list_tools]` after adding it**
Almost always a decorator typo. Confirm you have `@mcp.tool()` (with the parentheses) directly above the function.

**Tool call returns an error like "Invalid arguments"**
Parameter names in your function signature must match what `client.py` passes, case-sensitive. Example: `def find_component_by_family(family: str)` needs `{"family": "Sensors"}` in the call.

## Editing files in Codespaces

- Open from the left file explorer.
- Save with `Ctrl+S`.
- Copilot suggestions appear as grey text — press `Tab` to accept.

## Optional: the MCP Inspector GUI

We use `client.py` in the workshop because it's tiny and shows exactly what's on the wire. There is also a browser-based GUI, the **MCP Inspector**, worth knowing about after the workshop.

On Codespaces it needs an extra env var because of DNS-rebinding protection in the Inspector proxy:

```bash
ALLOWED_ORIGINS="https://${CODESPACE_NAME}-6274.app.github.dev" \
  npx @modelcontextprotocol/inspector python server.py
```

Then open port 6274 from the **PORTS** tab. If it still refuses to connect, that's a known Inspector v2.2 issue on Codespaces — the `python client.py` path always works.
