# Hands-on tasks

## Task 1 — Relabel a drawer (~5 min)

Every MCP tool has a description string. The AI reads it to decide which tool to call. Change the description of `lookup_component` in `server.py`.

**Before** (what's there now):

```python
"""Look up a component by its ID (e.g. CM101A, ES249B, AR200A).

Returns the component's functional name, owner, family, and subsystem.
Use this when the user asks who owns a component, what family it belongs
to, or wants basic component metadata.
"""
```

**Try replacing it with:**

```python
"""Fetch employee HR records by their badge number."""
```

Restart the server. Ask the AI: *"who owns component CM101A?"*

What happens? Change it back. Ask the same question again. See the difference.

**The lesson:** the description IS the interface. Write it for the AI, not for humans.

---

## Task 2 — Build a new drawer (~10 min)

Add a new tool to `server.py` called `find_component_by_family`. It should take a family name (like `"Motor Control"` or `"Sensors"`) and return every component in that family.

The data you need is already loaded — see the `_load_components()` helper at the top of the file.

Copilot will do most of the typing. Structure to follow:

```python
@mcp.tool()
def find_component_by_family(family: str) -> list:
    """<< write a description the AI can use to decide when to call this >>"""
    # << your code — filter _load_components() by family name >>
```

Restart the server. Ask the AI: *"which components are in the Sensors family?"*

**Bonus if you finish early:** add a tool that combines the two — given a family name, return every component AND its NTCs.

---

## How to restart

Press `Ctrl+C` in the terminal running the Inspector, then re-run the same command. See `CHEAT_SHEET.md` if you get stuck.
