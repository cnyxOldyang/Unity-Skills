---
description: Audit [UnitySkill] C# definitions against the shipped SKILL.md documentation and sync every skill count.
mode: agent
---

Read [`.claude/commands/skillcheck.md`](../../.claude/commands/skillcheck.md) and follow it exactly.

Scope: every `[UnitySkill]` definition under `SkillsForUnity/Editor/` and every document under `SkillsForUnity/unity-skills~/skills/` plus `SkillsForUnity/unity-skills~/SKILL.md`.

Report ghost skills, undocumented skills, parameter mismatches and count drift, then apply the fixes the command specifies. Finish by running `python .github/scripts/check_skill_frontmatter.py` and `python .github/scripts/check_locales.py`.
