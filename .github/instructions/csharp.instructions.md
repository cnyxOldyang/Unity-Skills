---
applyTo: "SkillsForUnity/Editor/**/*.cs"
description: "C# conventions for the UnitySkills Editor plugin"
---

# C# Editor plugin rules

Read [AGENTS.md](../../AGENTS.md) first. These are the rules most often violated in `SkillsForUnity/Editor/`.

## Threading and lifecycle

- HTTP handlers only enqueue work. The Unity main thread drains the queue from `EditorApplication.update` (≤20 requests per tick).
- Never touch `UnityEngine.*` / `UnityEditor.*` from an HTTP thread, background task or `Task.Run`.
- A skill that blocks the main thread also freezes `/health`; use `AsyncJobService` / `BatchJobService` for long work.

## Skills

- Required metadata: name, description, `Category`, `Operation`. Fill `Tags`, `Outputs` and risk flags truthfully; `Outputs` must be a subset of what the method returns.
- `Mode` defaults to `FullAuto`; use `SemiAuto` only for pure reads with zero side effects.
- Validate with `Validate.Required` / `Validate.RequiredJsonArray`, not hand-written if-chains.
- Return `SkillErrorResponse.Build(code, message, ...)` for business errors; throw only for unknown framework failures.
- Writers register Undo and call `WorkflowManager.SnapshotXxx(...)` when `TracksWorkflow = true`.
- Provide `xxx` / `xxx_batch` pairs; batch uses `BatchExecutor.Execute<TItem>`.
- Find GameObjects with `GameObjectFinder.FindOrError`, and scene-wide objects with `FindHelper.FindAll<T>()`.
- Optional-package modules use the `XxxReflectionHelper.cs` / `DOTweenPresenceDetector` pattern, never direct type references.
- Never write permission or allowlist logic inside a skill.

## Logging, prefs, versions

- Log only through `SkillsLogger.{Log,LogWarning,LogError,LogVerbose,LogAgent}` with `PREFIX_*` colors.
- Prefix EditorPrefs keys with `UnitySkills_`.
- Version literals come only from `SkillsLogger.Version`.
- Wrap Unity 6 APIs in `#if UNITY_6000_0_OR_NEWER`.

## Files

- `.cs` files are UTF-8 with BOM and end with a blank line, `// Producer:Betsy`, and a newline.
- Every new `.cs` / `.uxml` / `.uss` / `.json` needs a `.meta` created as a byte copy of a sibling's full importer form with a fresh uuid4 GUID.
- Source comments are English.
