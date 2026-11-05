---
applyTo: "README.md,README_CN.md,docs/**/*.md,CHANGELOG.md"
description: "Documentation rules for README, setup guides and changelog"
---

# Documentation rules

Read [AGENTS.md](../../AGENTS.md) first.

- `README.md` and `README_CN.md` are kept in lockstep; every user-facing change lands in both.
- `docs/SETUP_GUIDE.md` and `docs/SETUP_GUIDE_CN.md` are also bilingual pairs. Keep tables, paths and version claims identical.
- Never hand-edit skill counts, module counts or category tables. Run `/skillcheck` to sync them.
- CHANGELOG entries are written by `/updateversion` (`.claude/commands/updateversion.md`), which also bumps the declared version anchors. Do not invent a version number or add an `Unreleased` heading — `check_project_version.py` expects the top heading to match the declared version.
- Use relative Markdown links with `/` separators. `.md` files have no BOM.
