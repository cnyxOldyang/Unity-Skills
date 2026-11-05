---
description: Audit .meta file pairing and GUID collisions across the Unity package.
mode: agent
---

Read [`.claude/commands/metacheck.md`](../../.claude/commands/metacheck.md) and follow it exactly.

Start with `python .github/scripts/check_meta_files.py`. Then perform the manual parts the command describes: look for pseudo-random GUIDs and compare against the known third-party blacklist.

Report every finding with the file path and GUID, and fix only the files that violate the rules. Do not change a GUID that is referenced from outside the repository without explaining why.
