# Hands-on tasks

## Warm-up (~2 min) — make sure Copilot Chat sees our server

1. Open the **Chat** panel on the right of your Codespace.
2. Set the mode selector at the bottom to **Agent**.
3. Click the **tools icon** in the chat input area. You should see two tools from `component-workshop`:
   - `lookup_component`
   - `list_ntcs_for_component`
4. Ask Copilot in chat:
   > **who owns component CM101A?**

   Expected: Copilot calls `lookup_component` and answers with something like *"CM101A (MotAgCorrln) is owned by Dave Smith, in the Motor Control family, EPS subsystem."*

That's the entire loop. The two tasks below are small edits to it.

---

## Task 1 — Relabel a drawer (~5 min)

Every tool has a description string. Copilot reads it when deciding whether to call the tool. It is the label taped to the drawer.

Open `server.py`. Find the `lookup_component` function and change the **first line** of its docstring:

**Before:**
```python
"""Look up a component by its ID (e.g. CM101A, ES249B, AR200A).
..."""
```

**Try:**
```python
"""Fetch employee HR records by their badge number.
..."""
```

Save. In the **MCP Servers** panel, find `component-workshop` and click **Restart** so Copilot re-reads the tool list.

Now ask again in chat:
> **who owns component CM101A?**

Copilot probably won't call your tool this time. The label now says "HR records / badge number", which has nothing to do with components. Copilot may say it doesn't have a way to answer, or reach for the wrong tool.

Change the description back. Restart the server. Ask again. It works again.

**Lesson:** the description IS the interface for the AI. Write it for the AI, not for humans.

---

## Task 2 — Build a new drawer (~10 min)

Add a new tool to `server.py` called `find_component_by_family`. It should take a family name (`"Motor Control"`, `"Sensors"`, `"Arbitration"`, ...) and return every component in that family.

The data helper is already there — see `_load_components()` at the top of `server.py`. Copilot will do most of the typing:

```python
@mcp.tool()
def find_component_by_family(family: str) -> list:
    """<< write a description Copilot can act on >>"""
    # << your code — filter _load_components() by family >>
```

Save. Restart `component-workshop` in the MCP Servers panel.

In chat:
> **which components are in the Sensors family?**

Copilot should call your new tool and answer with `HwTqEstm`, `HwTqArbn`, `TorqRateLmt`.

**Bonus if you finish early:** add one more tool that combines the two — given a family name, return every component AND its NTCs. Then ask:
> **what faults does the Sensors family cover?**

---

## Peek under the hood (optional)

Curious what a tool call actually looks like on the wire, without any AI in the loop? Run:

```bash
python client.py
```

That is a minimal stdio client: handshake → `list_tools` → `call_tool("lookup_component", "CM101A")` → prints the raw JSON. It is the same signal-trace flow you saw on the slides, in ~40 lines.
