# TODO List

This project isn't valuable enough to warrant using Jira or similar task-tracking services.
The basic tasks that need to be addressed in the future are outlined here.

# Current Maintenance & Polish (v3.2.x)

## Core Engine & Compiler Fixes

1. [ ] Fix act compilation fall-through in `compiler.ts`: automatically emit a termination boundary between main act steps and label references (`[ref]`) to prevent unintended execution of the first reference.
2. [ ] Namespace `[ref]` label references per act in `compiler.ts` to prevent name collisions across different scenario acts.
3. [ ] Isolate JavaScript expression evaluation in `script_runtime.ts` and `dialogue.ts` in a secure sandbox to prevent arbitrary host system access.
4. [ ] Refactor save game storage in `saves.ts`: separate heavy screenshot data from metadata in `.saves.json` (or store screenshots asynchronously) to prevent event-loop blocking.
5. [ ] Retain legacy `dr-hide-active` CSS class alongside `hide` on image elements for backward compatibility with existing custom CSS.

## User Experience (UX) Enhancements

6. [ ] Implement a Dialogue Backlog / History overlay (mouse wheel up or UI button) allowing players to review past dialogue and replay voice clips.
7. [ ] Add comprehensive keyboard navigation and hotkeys (`Space`/`Enter` to advance, `Esc`/Right-click to toggle menu, `Ctrl` to fast-skip, `H` to hide dialogue window, `F5`/`F9` for quick save/load).
8. [ ] Implement Auto-play mode (configurable delay) and Skip mode (fast-forward dialogue) with UI controls and status indicators.
9. [ ] Split single master audio volume into dedicated mixer buses: Master, BGM (Music), SFX (Sound effects), and Voice.

## Developer Experience (DX) Enhancements

10. [ ] Provide standard default CSS staging stylesheets (`left.css`, `center.css`, `right.css`, etc.) for seamless sprite positioning without hardcoded coordinates in the engine.
11. [ ] Implement scenario hot-reloading (HMR) for `.dreamrun` files to push updated scenario steps over WebSocket without restarting the server or resetting runtime variables.
12. [ ] Add built-in screen transition and camera tags (`[fade-in]`, `[fade-out]`, `[dissolve]`, `[shake]`).

# Major Milestone: Version 4.0.0 (Unified Object Architecture & DSL Overhaul)

## 1. Unified Named Tag Attributes (HTML-like Syntax)

13. [ ] Overhaul tag argument parser to support named HTML-like attributes across all `.dreamrun` tags:
    - Example: `[image show id=hero src="/sprites/hero.png" layer=1 container-css="/styles/center.css"]`
    - Example: `[audio music src="/audio/ambient.wav" id=bgm volume="0.1~1500" pitch="1.0"]`
    - Support unquoted identifiers (`id=bgm`), string literals (`src="..."`), and expressions (`volume={bgm_vol}`).
    - Make optional arguments truly optional without requiring placeholder quotes (`""`) or zeroes.

## 2. Universal `[object]` Foundation

14. [ ] Implement the `[object]` tag system as the universal primitive for all frontend visual & interactive entities:
    - `[object create id=<id>] ... [/object]`: instantiate a dynamic client-side object.
    - `[object modify id=<id>] ... [/object]`: update properties, styles, or state of an existing object.
    - `[object delete id=<id>]`: unmount and cleanly dispose of the object.
15. [ ] Support Svelte component embedding and compilation for `[object]`:
    - Server-side compilation of inline Svelte code into executable client chunks, or reference to pre-existing `.svelte` components (`component="Widget.svelte"` with props).
    - Lifecycle management: proper mounting and cleanup (`onDestroy`/unmount) to prevent memory leaks, listeners, or WebGL context leaks.
16. [ ] State serialization for `[object]` instances in `.dreamsave`:
    - Define a standardized state contract so dynamic objects can persist and restore their state upon save/load.
17. [ ] Refactor `GameState.svelte.js` to use a unified object registry:
    - Replace separate collections (`images`, `audio`) with a unified `objects: Map<string, GameObjectState>` store.

## 3. Re-architected `[image]` Tag on Top of `[object]`

18. [ ] Rebuild `[image]` as a high-level syntax sugar compiling down to `[object type="image"]`:
    - New syntax: `[image show/modify id=<id> layer=<layer> container-css={...} image-css={...}]`
    - Make `layer` optional (relying on CSS classes for z-index ordering when omitted).
    - Support multiple CSS stylesheets via list syntax (e.g., `container-css={"/styles/pos.css", "/styles/anim.css", dynamic_css}`).

## 4. Character Sprite Expression Syntax

19. [ ] Implement inline sprite-switching syntax in dialogue statements:
    - Full form: `:char:[id{sprite}] > "Dialogue text"` (updates image `id` with `sprite` image path or variable simultaneously with the spoken line).
    - Shorthand form: `:char:[{sprite}] > "Dialogue text"` (automatically targets the object whose `id` matches `char`).