---
name: setup
description: Connect and verify Evermuse — confirm the MCP is authorized, pick the right Product (and Project if needed), and run a smoke-test search
---

Use the **setup-evermuse** skill to connect and verify Evermuse before any other command.

1. Confirm the `evermuse` MCP tools are available. If they aren't, authorize the Evermuse server in Cursor Settings → MCP and stop — nothing else works until it's connected.
2. Call `get_products`. If one product exists, use it; if several, list them and ask which. Call `get_product_summary` for it and keep its `product_id` — every product-scoped call takes it as a parameter.
3. Call `get_projects` and mention them. Most tasks stay at product scope; pass `project_id` only when focused on one research effort.
4. Smoke-test with one `search` and show 1–2 real quotes so grounding is visibly working.
5. Report the active product/project and suggest a next step.

Product (optional):
