---
description: Connect and verify Evermuse — confirm the MCP is authorized, pick the right Product (and Project if needed), and run a smoke-test search
argument-hint: "[product name (optional)]"
---

# /evermuse:setup — Get grounded

Prepare the session so every later skill is grounded in the right customer data.

## Workflow
1. **Check the MCP.** Confirm the `mcp__evermuse__*` tools are present. If not, tell the user to authorize the Evermuse connector (via `/mcp` in an interactive session, or their claude.ai connector settings) and stop — nothing else works until it's connected.
2. **Pick the Product.** Call `get_products`. If `$ARGUMENTS` names one, match it. If one product exists, use it. If several, show the list and ask which one. Then call `get_product_summary` for that product and keep its `product_id`: every product-scoped call takes it as a parameter — there is no session to switch. (Read `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Rule 1.)
3. **Offer a Project.** Call `get_projects`. Mention the projects (Discovery/Support/Sales/…) and note that most tasks stay at product scope; pass `project_id` only when the user is focused on one research effort.
4. **Smoke-test.** Run one `search` (nature=evidence) on the product's core area and show 1–2 real quotes, so the user sees grounding working.
5. Report which product/project is active and suggest a next step (`/evermuse:research`, `/evermuse:spec`).
