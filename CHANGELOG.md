# Changelog

All notable changes to the Evermuse plugin are documented here.

## 0.2.0

Aligns the plugin with the Evermuse MCP surface currently served to OAuth clients
(Claude Code and Cursor), and makes `skills/` generated rather than hand-copied.

### Added

- **Workflow tools.** `customer_research`, `create_prd`, `write_brief`,
  `write_feature_spec`, `user_personas`, `user_stories`, `competitor_analysis` and
  `summarize_conversation` are documented in `using-evermuse` and the tool reference.
  The matching commands now start from their workflow tool, which runs the first
  grounding step and returns the methodology.
- **New skills:** `daily-brief` and `schedule-new-ingestion-tasks`.
- `get_product_summary`, `create_shaping_note`, `update_shaping_note`, `add_signals`
  and `update_signals` are documented.
- **Cursor command frontmatter.** All 28 `cursor-commands/*.md` carry `name` and
  `description`, per Cursor's command reference.
- An MCP tool table in the README, a CHANGELOG, and `category: Integrations` in the
  Cursor manifest.

### Changed

- **`skills/` is now generated** from the Evermuse monorepo via `yarn sync:em-plugin`.
  Request changes upstream; do not edit `skills/` here.
- `/setup` (both client layers) picks a product with `get_products` +
  `get_product_summary` and keeps `product_id`, instead of calling the removed
  `switch_product` / `switch_project`.

### Removed

- **Third-party bridge guidance** (`find_tool` / `call_tool` and
  `references/third-party-bridge.md`). These tools are never served to OAuth clients,
  so the guidance was unreachable in both Claude Code and Cursor.
- References to tools no longer on the surface: `find_supporting_quotes`, `get_notes`,
  `get_meetings`, `get_meeting_transcript`, `switch_product`, `switch_project`, and
  `see_updated_roadmap` (renamed to `get_opportunities`).

## 0.1.0

Initial release: 34 Claude Code commands, a 28-command Cursor layer, and the skills
library, backed by the Evermuse MCP.
