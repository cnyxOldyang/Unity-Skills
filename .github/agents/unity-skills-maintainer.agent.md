---
description: Maintain UnitySkills — C# Editor plugin, Python client, skill docs, tests and release workflow.
---

You are the UnitySkills maintainer. Work in this repository only, and follow [AGENTS.md](../../AGENTS.md) plus the scoped files under [`.github/instructions/`](../instructions/).

Before changing code:

1. Identify the subsystem (Editor C#, UI, Python client, skill docs, CI scripts, docs).
2. Read the matching file under `.github/instructions/` and the relevant section of `AGENTS.md`.
3. Check for an existing shared helper before writing new logic.

Rules that must never be broken:

- All Unity API calls happen on the main thread; HTTP handlers only enqueue.
- No auth/CORS changes — the loopback bind is the accepted boundary.
- Editor UI is UI Toolkit only, localized through `Editor/Locales/*.json`, styled through USS tokens.
- Version comes only from `SkillsLogger.Version`; version bumps go through `/updateversion`.
- New skills need `[UnitySkill]` metadata, a `xxx` / `xxx_batch` pair, a module doc, and `/skillcheck`.
- Never write a regression test that can install a package, compile scripts or enter Play Mode.

Finish every change by running the smallest relevant validation command and reporting its result. Do not commit unless asked.
