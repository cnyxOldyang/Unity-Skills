---
applyTo: "SkillsForUnity/Editor/UI/**"
description: "UI Toolkit, localization and font-atlas rules for the UnitySkills window"
---

# Editor UI rules

Read [AGENTS.md](../../AGENTS.md) first. The Editor UI is UI Toolkit only — no IMGUI.

- New UI is `.cs + .uxml + .uss` under `Editor/UI/`, loaded by the `Packages/com.besty.unity-skills/Editor/UI/...` path constant; `CreateGUI()` adds the USS then calls `CloneTree`.
- One controller per UXML subtree: `XxxController(VisualElement root, EditorWindow owner)`. The window only assembles tabs from `MainTabs`.
- New tab: `Tabs/X.uxml` (+ `.meta` with a fresh GUID), `Controllers/XTabController.cs`, and one `MainTabDefinition`. `HistoryTabController` is the smallest complete example.
- Periodic refresh only through `EditorUiScheduler.RepeatSafe(element, ms, body)`. Never use a bare `schedule.Execute().Every()` that mutates the tree, and never poll from `EditorApplication.update`. Refresh expensive data on tab activation, button press or value change.
- Visibility goes through `UiVisibility.SetVisible`; do not use `style.display` or inline `display:none` in UXML. Inline styles are only for runtime-computed values.
- Style through USS classes and `var(--color-*)` tokens from `UnitySkillsWindow.uss` `:root`; add a `.unity-theme-light` value for every new color.
- No emoji icons (Unity 2022's editor font cannot render them); use USS geometry or `UISkillsEditorIcons`.
- All UI text comes from `Editor/Locales/{en,zh-CN,ru}.json` via `SkillsLocalization.Get(key[, args])`; the three files must share identical key sets.
- Every non-ASCII character in `Editor/UI/**`, `Editor/Skills/Localization.cs` and `Editor/Locales/*.json` must already exist in `UnitySkillsCN-UI.asset`. In tooling, write CJK as `\uXXXX` escapes.
- `.uxml` / `.uss` files have no BOM. `.cs` files end with a blank line, `// Producer:Betsy`, and a newline.
