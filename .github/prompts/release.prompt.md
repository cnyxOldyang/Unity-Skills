---
description: "Run the UnitySkills release workflow: candidate matrix, beta/main sync, tag and stable GitHub Release."
mode: agent
---

Optional version (leave blank to use the declared version): ${input:version}

Read [`.claude/commands/release.md`](../../.claude/commands/release.md) and follow it exactly.

Key constraints:

- Develop on `beta`; `main` is linear and must contain no merge commits.
- Never move or re-publish a released tag — fix forward with a new version.
- Star History chart commits live on `main` only; never sync them into `beta`.
- Verify the candidate matrix, the version anchors and the CHANGELOG before tagging.

Stop and report if any gate fails; do not push a partial release.
