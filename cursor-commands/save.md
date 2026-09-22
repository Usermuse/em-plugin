---
name: save
description: Save the current research, spec, plan, or analysis back into Evermuse so the knowledge base compounds
---

Save the most recent substantial deliverable back into Evermuse so the knowledge base compounds.

First read `skills/using-evermuse/references/saving-to-evermuse.md` in this plugin for the exact conventions. Then decide what to save, pick the `nature` (customer-voice synthesis → evidence; market or competitor material → context; company direction such as specs, plans and strategy → guidance), resolve the project via `get_projects` (ask if there are several), and call `add_source(source_type: "document", tags: ["evermuse-plugin", …])`. Confirm what was saved and where.

What to save (optional):
