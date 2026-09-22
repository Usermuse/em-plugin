---
name: spec
description: Write a feature specification grounded in real customer evidence — prioritized user stories, acceptance scenarios, and requirements with verbatim customer quotes and source links
---

Start by calling the `write_feature_spec` workflow tool with the request below — it runs the first grounding step and returns the methodology. Then:

Use the **write-feature-spec** skill to write a feature specification grounded in real customer evidence.

Follow its flow: verify product → ground (evidence searches + quotes + guidance) → clarify (evidence-attached, max 3 markers) → draft the spec with a Customer Evidence section, per-story evidence, cited functional requirements and measurable success criteria → quality gate → save to `specs/<slug>/spec.md` and to Evermuse (nature=guidance). Then offer `/dev-plan`.

Feature to spec:
