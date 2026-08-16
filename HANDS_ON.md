# Hands-on tasks

## Warm-up (~2 min) — make sure Copilot Chat sees our server

1. Open the **Chat** panel on the right of your Codespace.
2. Set the mode selector at the bottom to **Agent**.
3. Click the **tools icon** in the chat input area. Uncheck every tool EXCEPT the three under `component-workshop` — this stops Copilot from cheating by reading the CSV file or grepping the workspace.
4. Click **New Chat** (top of the panel) so we start with no prior conversation memory.
5. Ask Copilot in chat:
   > **who owns component CM101A?**

   Expected: Copilot calls `lookup_component` and answers with something like *"CM101A (MotAgCorrln) is owned by Dave Smith, Motor Control family, EPS subsystem."*

That's the entire loop. The two tasks below are small edits to it.

---

## Task 1 — Relabel the drawer (~5 min)

Every tool advertises itself to the AI through **three** things: its **name**, its **description**, and its **parameters**. Copilot reads all three to decide which tool to call. Change any one badly and Copilot may pick a different tool — or none at all.

Open `server.py`. Rename `lookup_component` and rewrite its docstring so it looks like an HR tool:

**Before:**
```python
@mcp.tool()
def lookup_component(component_id: str) -> dict:
    """Look up a component by its ID (e.g. CM101A, ES249B, AR200A).

    Returns the component's functional name, owner, family, and subsystem.
    Use this when the user asks who owns a component ...
    """
    ...
```

**After:**
```python
@mcp.tool()
def fetch_hr_record(component_id: str) -> dict:
    """Fetch employee HR records by their badge number."""
    ...
```

The body doesn't change — the tool still works internally. It's just **misadvertised**.

Save. Restart `component-workshop` in the MCP Servers panel.

**Before asking, click New Chat again** (kills Copilot's memory of your last successful answer).

Ask:
> **who owns component CM101A?**

Copilot has no tool that looks relevant. It backs off ("I don't have a tool for that"), tries to answer without one, or reaches for a wrong tool.

Revert the changes. Save. Restart. New Chat. Ask again. Original answer returns.

**Lesson:** the whole signature is the interface — **name, description, parameters, all three**. Copilot picks tools by reading them together. Get any one badly wrong and your tool becomes invisible.

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

Save. Restart `component-workshop` in the MCP Servers panel. Click **New Chat**.

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

---

## Two rules to remember across both tasks

1. **New Chat before every test.** Copilot remembers prior answers within a conversation.
2. **Keep only `component-workshop` tools checked.** Otherwise Copilot may bypass your tool entirely and read files or grep the workspace.
