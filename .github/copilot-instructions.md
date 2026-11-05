# GitHub Copilot — UnitySkills repository instructions

[AGENTS.md](../AGENTS.md) is the authoritative guide for agents working in this repository. Read it before changing code. This file restates only the constraints Copilot needs at a glance and points at the detailed sections.

## What this repository is

A Unity Editor plugin (UPM package `com.besty.unity-skills`, C#) plus a shipped Python client and skill documentation.

```text
AI agent → unity_skills.py → HTTP localhost:8090-8100 → SkillsHttpServer
         → SkillRouter ([UnitySkill] reflection) → 56 *Skills.cs / 54 categories / 839 skills
```

Key locations:

- `SkillsForUnity/Editor/` — plugin source (`Skills/`, `UI/`, `Locales/`, `Versioning/`)
- `SkillsForUnity/Tests/Editor/` — EditMode tests, including optional-package fixtures
- `SkillsForUnity/unity-skills~/` — the shipped template: `SKILL.md`, `scripts/unity_skills.py`, `skills/`, `references/`
- `.claude/commands/` — repo workflow prompts; `.github/prompts/` mirrors them for Copilot Chat
- `.github/scripts/` — CI validation scripts and their unittest suite

## Non-negotiable rules

- **Main thread only.** The HTTP thread enqueues; the Unity main thread drains the queue. Never call `UnityEngine.*` / `UnityEditor.*` off the main thread.
- **Every request goes through the main-thread queue**, including `GET /health`, `/jobs/{id}` and `/skills`. Never block that thread from a skill.
- **No auth and wildcard CORS are intentional** (loopback bind is the accepted boundary). Do not add auth, tighten CORS, or validate `Origin` as a "security fix".
- **Editor UI uses UI Toolkit only** — no IMGUI (`OnGUI`, `EditorGUILayout`, `GUILayout`, `OnInspectorGUI`). New UI is `.cs + .uxml + .uss`, one controller per subtree, styling through USS classes and `var(--color-*)` tokens.
- **All user-visible strings live in `Editor/Locales/{en,zh-CN,ru}.json`** with identical key sets. Non-ASCII characters must already exist in the font atlas; never bake the atlas (use `UISkillsFontIncrementalUpdater.AddMissingGlyphs` from a throwaway MenuItem).
- **Version comes only from `SkillsLogger.Version`.** Never hardcode or bump a version literal; version changes go through `/updateversion`.
- **Log only through `SkillsLogger.*`** with `PREFIX_*` colors, never `Debug.Log*`.
- **`AGENTS.md` is the source of truth** for architecture, shared helpers, encoding, `.meta` rules and workflow commands. Keep it in sync when behavior changes.

## Skills

New skills use the `[UnitySkill]` attribute with `Category`, `Operation`, truthful risk metadata and an `Outputs` subset of the real return value. Business errors return `SkillErrorResponse.Build(...)`. Permission gating, audit and allowlists are framework concerns — never write authorization logic inside a skill. A new module also needs a `SkillCategory` entry, a `XxxSkills.cs` file and `unity-skills~/skills/<module>/SKILL.md`; run `/skillcheck` afterwards.

## Validation

Use the smallest command that covers the change:

- C# / Unity: run the relevant EditMode tests or the package compile workflow.
- Python helper or CI script: `python -m unittest discover -s .github/scripts/tests -q`
- `.meta` coverage: `python .github/scripts/check_meta_files.py`
- Skill documentation consistency: `/skillcheck` (`.claude/commands/skillcheck.md`)
- Locales: `python .github/scripts/check_locales.py`
- Version anchors: `python .github/scripts/check_project_version.py`

Do not write a regression test whose failure mode installs a package, compiles scripts or enters Play Mode.
