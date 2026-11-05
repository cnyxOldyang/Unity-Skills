---
applyTo: "SkillsForUnity/Tests/**/*.cs"
description: "Testing rules for UnitySkills EditMode tests"
---

# Test rules

Read [AGENTS.md](../../AGENTS.md) first.

- Tests run in the Unity Editor's EditMode. Keep every test isolated: use temp directories, restore overridden statics in `[TearDown]`, and never touch the user's real install locations or EditorPrefs.
- Do not write a regression test whose failure mode installs a package, compiles scripts or enters Play Mode. A regressed guard must not be able to change the project running the test. Test such guards through an internal seam, or not at all.
- Optional-package fixtures derive from `OptionalPackageTestBase`: call skills by name, read state back by reflection, and let the fixture be ignored when the probe skill's package is missing. A new optional module needs a fixture and a floor in `.github/optional-package-floors.json`.
- Prefer NUnit `[TestCase]` tables for path/alias mappings and pure functions; prefer real Editor read-back for skills that mutate state.
- `SkillsForUnity/Tests/` must stay inside the assembly's `InternalsVisibleTo` contract; do not widen production visibility just to make a test compile.
- `.cs` files are UTF-8 with BOM and end with a blank line, `// Producer:Betsy`, and a newline.
