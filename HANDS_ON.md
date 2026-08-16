# Hands-on tasks

Before you start: run `python client.py` once. You should see three sections:

- `[handshake]` — the client connected to the server
- `[list_tools]` — the drawers the server exposes (name + first line of description)
- `[call_tool]` — the JSON result of calling `lookup_component("CM101A")`

That's the entire MCP loop. Everything below is small edits to it.

---

## Task 1 — Relabel a drawer (~5 min)

Every tool has a description string. It's what a client (including an AI) reads when deciding whether to call the tool. Look at what `[list_tools]` prints — that first line of each tool's description is the "label on the drawer."

Open `server.py`. Find the description of `lookup_component`:

```python
"""Look up a component by its ID (e.g. CM101A, ES249B, AR200A).

Returns the component's functional name, owner, family, and subsystem.
..."""
```

Change the **first line** to something totally unrelated:

```python
"""Fetch employee HR records by their badge number."""
```

Save. Re-run `python client.py`.

Look at `[list_tools]` in the output. The label the client sees has changed.

If this were an AI client, it would now think this tool is about HR records — and would not call it when someone asked about `CM101A`. **The description IS the interface for the AI.**

Change it back before moving on.

---

## Task 2 — Build a new drawer (~10 min)

Add a new tool to `server.py` called `find_component_by_family`. It should take a family name (`"Motor Control"`, `"Sensors"`, `"Arbitration"`, ...) and return every component in that family.

The data is already loaded — see the `_load_components()` helper at the top of `server.py`.

Copilot will do most of the typing. Structure to follow:

```python
@mcp.tool()
def find_component_by_family(family: str) -> list:
    """<< write a description a future AI can act on >>"""
    # << your code — filter _load_components() by family >>
```

Then open `client.py`. After the existing `lookup_component` call, add a second call for your new tool:

```python
print("\n[call_tool] find_component_by_family(family='Sensors')")
result = await session.call_tool(
    "find_component_by_family",
    {"family": "Sensors"},
)
for block in result.content:
    text = getattr(block, "text", str(block))
    print(text)
```

Save both files, re-run `python client.py`. Your tool should show up in `[list_tools]` and return a list of Sensors components.

**Bonus if you finish early:** add one more tool that combines the two — given a family name, return every component AND its NTCs.

---

## How to restart

Just re-run `python client.py`. It re-spawns the server every time. There is no separate server process to stop.
