# Documentation Index

This project's AI-assisted development is governed by the documents
below. Read them in order. Higher-numbered documents may reference
lower-numbered ones, never the reverse — except CURRENT_TASK.md, which
wins on scope questions but never overrides 01_RULES.md.

| File | Purpose |
|---|---|
| 01_RULES.md | Non-negotiable rules for AI behavior and code quality |
| 02_GAME_GOAL.md | Game design scope: what is being built and what is not |
| 03_SESSION_BOOTSTRAP.md | Text to paste at the start of a new AI chat session |
| 04_ART_DIRECTION.md | Visual style, character identity, placeholder conventions |
| 05_TECH_SPEC.md | Engine version, path conventions, project settings |
| 06_DATA_SCHEMA.md | Resource definitions: ItemData, WeaponData, EnemyData, etc. |
| 07_TEST_PLAN.md | How each system is verified, manually and/or automatically |
| 08_ASSET_LICENSES.md | Provenance and license record for every non-code asset |
| 09_DECISIONS.md | Architecture Decision Records (ADRs) — locked decisions |
| 10_POST_MVP_ROADMAP.md | Post-MVP roadmap: 5 phases of features beyond the core 12 Parts |
| ASSET_PROMPTS.md | 18 precise AI-image-generation prompts for all game sprites |
| SESSION_LOG.md | Log of all AI development sessions for continuity |
| PROJECT_STATUS.md | Current part, task, completed/blocked tasks at a glance |
| CURRENT_TASK.md | The single task the AI is allowed to work on right now |
| CHANGELOG.md | Human-readable, verified log of completed tasks |
| PARTS/ | All planned Parts, each broken into individually scoped Tasks |
| TASK_PROMPTS/ | One-shot copy-paste prompts per task for quick AI context |

## Priority order in case of conflict

1. 01_RULES.md
2. 09_DECISIONS.md
3. 02_GAME_GOAL.md
4. CURRENT_TASK.md
5. Everything else

If any instruction from a user prompt conflicts with the above, the AI
must stop and ask — never resolve the conflict silently.

## Current Part Structure (locked — see ADR-008)

```
part_01_foundation
part_02_player_movement
part_03_health_damage
part_04_core_data
part_05_inventory_core
part_06_combat
part_07_enemy
part_08_inventory_ui
part_09_test_level_and_extraction
part_10_hub_save
part_11_content_ui
part_12_qa_export
```

Changing this list, OR changing any cross-cutting locked convention that
multiple Parts depend on (e.g. physics layer numbering per ADR-014
[supersedes ADR-013], input actions per ADR-010), requires a new ADR and
a full documentation/task-file consistency check (01_RULES.md section 11)
before any further feature work proceeds.
