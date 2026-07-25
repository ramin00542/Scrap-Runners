## 📄 فایل: `final_commit_and_readme_update.prompt.md`

```markdown
# Final Commit, Push, and README.md Update Prompt

## Purpose
After all repair prompts have been executed and verified, perform two final
actions:
1. Stage, commit, and push ALL modified documentation files to GitHub.
2. Update `README.md` at the repository root to accurately reflect the
   current project state, structure, and conventions.

This is the final synchronization step. No gameplay code, no Game/ files,
and no Task files are modified by this prompt.

---

## Mandatory Reading (Before Any Change)
Read the current versions of:
1. `AGENTS.md`
2. `AI_DOCS/01_RULES.md`
3. `AI_DOCS/00_INDEX.md`
4. `AI_DOCS/02_GAME_GOAL.md`
5. `AI_DOCS/05_TECH_SPEC.md`
6. `AI_DOCS/09_DECISIONS.md`
7. `AI_DOCS/CURRENT_TASK.md`
8. `AI_DOCS/PROJECT_STATUS.md`
9. Repository-root `README.md`
10. Repository-root `.gitignore`
11. Repository-root `.gitattributes`
12. Repository-root `.editorconfig`
13. Directory tree at repository root (list all files and folders)

Do not assume any previously pasted content is current.

---

## Absolute Restrictions
- Do NOT modify any file under `Game/`.
- Do NOT modify any Task file under `AI_DOCS/PARTS/`.
- Do NOT modify any Task Prompt file under `AI_DOCS/TASK_PROMPTS/`.
- Do NOT modify `AI_DOCS/CURRENT_TASK.md`.
- Do NOT modify `AI_DOCS/CHANGELOG.md`.
- Do NOT modify `AI_DOCS/01_RULES.md`, `05_TECH_SPEC.md`, or `09_DECISIONS.md`.
- Do NOT create gameplay code, scenes, GDScript, resources, or assets.
- Do NOT run Godot or claim runtime/editor verification.
- Do NOT modify any file other than `README.md` (and Git staging).

---

## Part 1 — Git Commit and Push

### Step 1: Inspect Git Status
Run:
```bash
git status
git diff --stat
```

Report:
- Total number of modified files.
- Total number of untracked files.
- List of all files that will be staged.

### Step 2: Stage All Modified Documentation Files
Stage every modified and untracked file under:
- `AI_DOCS/`
- Repository-root configuration files (`.gitignore`, `.gitattributes`, `.editorconfig`)
- `README.md`

Do NOT stage:
- `Game/.godot/`
- `.freebuff/`
- Any `*.import` files
- Any binary or generated cache files

### Step 3: Commit with Descriptive Message
Use this exact commit message format:
```
Final sync: all documentation repairs applied, README updated

- All 60 identified issues resolved across 12 Parts
- Repair prompts executed and verified
- README.md updated to reflect current project structure
- Project ready for TASK-01-01 implementation
```

### Step 4: Push to GitHub
Push to the `main` branch:
```bash
git push origin main
```

If push fails, report the exact error and stop. Do not force-push.

### Step 5: Verify Push
Confirm:
- Commit hash
- Branch name
- Remote URL
- Number of files changed

---

## Part 2 — README.md Update

### Required Content Structure
Rewrite `README.md` with the following sections in this exact order:

#### 1. Project Title and Description
```markdown
# Scrap Runners
**A 2D top-down extraction shooter built with Godot 4.7.1**

Scrap Runners is a documentation-first game project. The game has NOT been
built yet. This repository contains only design documents, task specifications,
and repair prompts.
```

#### 2. Project Status
```markdown
## 📊 Current Status
- **Phase:** Documentation complete, awaiting implementation
- **Current Task:** TASK-01-00 (Godot version locked)
- **Next Task:** TASK-01-01 (Project Skeleton)
- **Total Tasks:** 58 across 12 Parts
- **Known Issues:** 0 (all 60 issues resolved)
- **Last Updated:** [Current date]
```

#### 3. Repository Structure
```markdown
## 📁 Repository Structure
```
Scrap Runners 2D_V2/
├── AI_DOCS/                    # All project documentation
│   ├── PARTS/                  # 12 implementation parts (58 tasks)
│   ├── TASK_PROMPTS/           # One-shot prompts for each task
│   ├── REPAIR_PROMPTS/         # Documentation repair prompts
│   ├── 00_INDEX.md             # Documentation index
│   ├── 01_RULES.md             # Project rules and conventions
│   ├── 02_GAME_GOAL.md         # Game design goals
│   ├── 05_TECH_SPEC.md         # Technical specification
│   ├── 06_DATA_SCHEMA.md       # Data/resource schemas
│   ├── 09_DECISIONS.md         # Architecture Decision Records
│   ├── CURRENT_TASK.md         # Active task
│   ├── PROJECT_STATUS.md       # Overall project status
│   └── CHANGELOG.md            # Change history
├── Game/                       # Canonical Godot project root (to be created)
│   ├── project.godot           # Godot project file (to be created)
│   ├── scenes/                 # Game scenes (to be created)
│   ├── scripts/                # GDScript files (to be created)
│   ├── resources/              # Game resources (to be created)
│   ├── assets/                 # Art assets (to be created)
│   └── tests/                  # Test scenes and scripts (to be created)
├── AGENTS.md                   # AI agent instructions
├── README.md                   # This file
├── .gitignore                  # Git ignore rules
├── .gitattributes              # Git attributes
└── .editorconfig               # Editor configuration
```

**Note:** The `Game/` directory does not yet exist. It will be created during
TASK-01-01 implementation.
```

#### 4. Technology Stack
```markdown
## 🛠️ Technology Stack
- **Game Engine:** Godot 4.7.1 (v4.7.1.stable.official [a13da4feb])
- **Language:** GDScript only
- **Project Type:** 2D top-down extraction shooter
- **Art Style:** Placeholder geometry (Polygon2D, ColorRect) for MVP
- **Architecture:** Documentation-first, 12-Part structure (ADR-008)
```

#### 5. Key Architecture Decisions
```markdown
## 🏗️ Key Architecture Decisions
- **ADR-002:** Godot 4.7.1 locked
- **ADR-004:** 640x360 base resolution, canvas_items stretch, keep aspect, nearest filter
- **ADR-007:** No speculative autoloads until Part 10
- **ADR-008:** 12-Part implementation structure
- **ADR-012:** Fixed loadout (Scrap Pistol), no loadout picker, no gear upgrades
- **ADR-014:** Physics layer/mask conventions (7 layers)
- **ADR-016:** Slight isometric tilt (2D visual offset, Y-sorting)
```

#### 6. Project Rules Summary
```markdown
## 📜 Project Rules (Summary)
1. **Documentation-first:** All decisions documented before implementation
2. **One task at a time:** Only one task active at a time (CURRENT_TASK.md)
3. **No speculative code:** Only implement what is explicitly authorized
4. **Tests required:** Every non-trivial task must include a test (Rule 10)
5. **Changelog discipline:** Only update CHANGELOG after real verification (Rule 6.4)
6. **Task transition:** Only transition CURRENT_TASK after verification (Rule 6.5)
7. **No magic numbers:** All numeric values must be documented and approved (Rule 9)
8. **Polygon2D placeholders:** No PNG assets for MVP (use Polygon2D/ColorRect)
9. **StringName comparison:** Use StringName for Resource identity (not ==)
10. **AI agent restrictions:** No silent architecture decisions, no runtime claims
```

#### 7. How to Use This Repository
```markdown
## 🚀 How to Use This Repository

### For Developers
1. Read `AI_DOCS/00_INDEX.md` for documentation overview
2. Read `AI_DOCS/01_RULES.md` for project conventions
3. Check `AI_DOCS/CURRENT_TASK.md` for the active task
4. Follow the 12-Part structure in `AI_DOCS/PARTS/`

### For AI Agents
1. Read `AGENTS.md` for agent-specific instructions
2. Always check `CURRENT_TASK.md` before making changes
3. Never modify files outside the active task's Allowed Files
4. Use Clarification Mode for ambiguous decisions
5. Never claim runtime verification without actual execution

### Implementation Order
```
Part 01: Foundation (TASK-01-01 to 01-04)
Part 02: Player Movement (TASK-02-01 to 02-05)
Part 03: Health & Damage (TASK-03-01 to 03-04)
Part 04: Core Data (TASK-04-01 to 04-03)
Part 05: Inventory Core (TASK-05-01 to 05-03)
Part 06: Combat (TASK-06-01 to 06-06)
Part 07: Enemy (TASK-07-01 to 07-06)
Part 08: Inventory UI (TASK-08-01 to 08-03)
Part 09: Test Level & Extraction (TASK-09-01 to 09-06)
Part 10: Hub & Save (TASK-10-01 to 10-06)
Part 11: Content UI (TASK-11-01 to 11-03)
Part 12: QA & Export (TASK-12-01 to 12-03)
```
```

#### 8. New Features (Proposed)
```markdown
## 💡 New Features (Proposed — Not Yet Authorized)
The following features are proposed but NOT yet part of the MVP:
- **NavigationAgent2D pathfinding** (requires new ADR)
- **Fog of War / Line of Sight** (requires new ADR)
- **Alert/Searching AI state** (requires new ADR)
- **Hitscan weapon option** (requires new ADR)

**BLOCKED:** Hideout loadout/upgrade system (conflicts with ADR-012)

See `AI_DOCS/10_POST_MVP_ROADMAP.md` for post-MVP plans.
```

#### 9. Contributing
```markdown
## 🤝 Contributing
This is a documentation-first project. Before contributing:
1. Read `AI_DOCS/01_RULES.md` completely
2. Check `AI_DOCS/CURRENT_TASK.md` for the active task
3. Do not modify files outside the active task's scope
4. All changes must be documented and verified
5. No speculative features or architecture changes
```

#### 10. License
```markdown
## 📄 License
[Specify license here, or state "All rights reserved" if not yet determined]
```

#### 11. Contact
```markdown
## 📧 Contact
[Add contact information if desired]
```

---

## Acceptance Criteria
- [ ] All modified files are staged and committed
- [ ] Commit message is descriptive and follows format
- [ ] Push to GitHub `main` branch succeeds
- [ ] `README.md` contains all 11 required sections
- [ ] `README.md` accurately reflects current project state
- [ ] `README.md` mentions that `Game/` does not yet exist
- [ ] `README.md` lists all 12 Parts in order
- [ ] `README.md` mentions New Features as proposed (not authorized)
- [ ] `README.md` mentions Prompt 05 as BLOCKED
- [ ] No Game/ files were modified
- [ ] No Task files were modified
- [ ] No CURRENT_TASK or CHANGELOG files were modified
- [ ] Git push is verified and commit hash is reported

---

## Required Final Report
Provide:
1. **Git Status Before Commit:** List of all modified/untracked files
2. **Files Staged:** Complete list
3. **Commit Hash:** Exact hash
4. **Push Result:** Success/failure with remote URL
5. **README.md Sections:** List all 11 sections created
6. **Verification:** Confirm all acceptance criteria met
7. **Known Limitations:** Any issues or warnings

End with:
```
Final sync complete. All documentation pushed to GitHub.
README.md updated to reflect current project state.
Project is ready for TASK-01-01 implementation.
```
```

---

## 🎯 دستور اجرایی برای Agent

این دستور را به Agent بدهید:

```text
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/final_commit_and_readme_update.prompt.md
```

---

## ✅ نتیجه مورد انتظار

بعد از اجرا، Agent باید:
1. ✅ تمام فایل‌های اصلاح‌شده را commit و push کند
2. ✅ `README.md` را با ۱۱ بخش کامل بازنویسی کند
3. ✅ ساختار فعلی پروژه را دقیقاً منعکس کند
4. ✅ ذکر کند که `Game/` هنوز وجود ندارد
5. ✅ لیست ۱۲ Part را به ترتیب نشان دهد
6. ✅ ویژگی‌های جدید را به عنوان PROPOSAL علامت‌گذاری کند
7. ✅ Prompt 05 را BLOCKED نشان دهد
8. ✅ گزارش نهایی با commit hash و push result بدهد

این فایل کاملاً آماده است و می‌توانید مستقیماً به Agent بدهید! 🚀