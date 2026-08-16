# Cheat sheet

## Restart after editing `server.py`

Copilot Chat holds a live connection to the running server. After you save an edit to `server.py`:

1. Open the **MCP Servers** panel (from the Copilot Chat settings or `Ctrl+Shift+P` → "MCP: List Servers").
2. Find `component-workshop`.
3. Click **Restart**.

Then re-ask your question in chat. Copilot will see the new tool list.

## Copilot Chat doesn't see the `component-workshop` tools

- Confirm the mode selector at the bottom of the Chat panel is **Agent**, not Ask / Edit.
- Reload the window: `Ctrl+Shift+P` → **"Developer: Reload Window"**.
- Open `.vscode/mcp.json` — it should have a `servers.component-workshop` block.
- Check the MCP Servers panel for a red / errored status on `component-workshop`. Click it to see the log.

## Server won't start (error in the MCP panel)

Look at the server's stderr in the MCP Servers panel log. Usually one of:

- `ModuleNotFoundError: No module named 'mcp'` → run `pip install -r requirements.txt`
- `ModuleNotFoundError: No module named 'mcp.server.fastmcp'` → `mcp>=2.0` removed FastMCP; run `pip install --force-reinstall "mcp>=1.11,<2.0"`
- `FileNotFoundError: components.csv` → the working directory is wrong. VS Code should launch from the repo root; if it doesn't, check `.vscode/mcp.json`.
- Python syntax error → test the server alone in the terminal: `python server.py` (it should sit silently — stdio servers don't print). If you get a traceback, that's your bug.

## Tool doesn't appear after adding it

Almost always one of:

- Missing `@mcp.tool()` decorator (with parentheses) above the function.
- Forgot to restart `component-workshop` in the MCP Servers panel.

## Tool call gets "Invalid arguments"

Parameter names in your function signature must match what Copilot passes, case-sensitive. If your function is `def find_component_by_family(family: str)`, Copilot passes `{"family": "..."}`.

## Editing in Codespaces

- File tree on the left. Save with `Ctrl+S`.
- Copilot suggestions in the editor appear as grey text — `Tab` to accept.

## Optional: the `python client.py` fallback

If Copilot Chat is having a bad day (network issue, tools panel stuck), you can still exercise the MCP loop from the terminal:

```bash
python client.py
```

It does the handshake, lists tools, and calls `lookup_component("CM101A")` directly. No AI in the loop.
