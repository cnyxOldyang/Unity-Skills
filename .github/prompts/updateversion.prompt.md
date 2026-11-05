---
description: Prepare a release candidate for a MAJOR.MINOR.PATCH version (no tag or GitHub Release).
mode: agent
---

Target version (without a leading `v`): ${input:version:2.9.2}

Read [`.claude/commands/updateversion.md`](../../.claude/commands/updateversion.md) and follow it exactly.

Update only the declared version anchors and write the CHANGELOG entry it specifies. Never search-and-replace version numbers globally and never hardcode a version in source. Verify with:

```bash
python .github/scripts/check_project_version.py
python .github/scripts/check_locales.py
```

This command must not create a tag or a GitHub Release.
