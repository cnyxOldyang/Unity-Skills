---
applyTo: "SkillsForUnity/unity-skills~/scripts/**/*.py,.github/scripts/**/*.py"
description: "Python client and CI script conventions"
---

# Python rules

Read [AGENTS.md](../../AGENTS.md) first.

## Shipped client (`unity-skills~/scripts/`)

- `unity_skills.py` runs on the user's machine with no third-party dependencies. Keep it standard-library only and preserve its shebang on line 1.
- Preserve existing skill names, parameter names and response shapes; the client is a compatibility surface for every installed copy.
- Identify the calling agent through the `X-Agent-Id` header read from `scripts/agent_config.json`; never hardcode an agent name.
- Poll `GET /jobs/{id}` instead of `job_wait` so a blocked main thread cannot freeze liveness.

## CI scripts (`.github/scripts/`)

- Add or update a unit test in `tests/` for every behavior change. Run `python -m unittest discover -s .github/scripts/tests -q`.
- Scripts are read-only validators unless the workflow explicitly says otherwise; exit non-zero with a clear message on violations.

## Files

- `.py` files are UTF-8 with BOM (the shipped `unity_skills.py` is the exception: it starts with its shebang and has no BOM) and end with a blank line, `# Producer:Betsy`, and a newline.
- `.md` files have no BOM.
