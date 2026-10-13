# Changelog

All notable changes to the DreamRun engine are documented in this file.

The format is based on [https://keepachangelog.com/en/1.1.0/@LINKMID@Keep a Changelog](), and this project adheres to [https://semver.org/spec/v2.0.0.html@LINKMID@Semantic Versioning]().

## About versions before 3.1.0

Versions before 3.1.0 are not listed here. At those stages the engine had not
yet reached a stable shape: the public interfaces — the `.dreamrun`
syntax, the save format, and the client API — were still being redesigned, and
the engine had no defined method of practical use. No official description was
ever produced for them.

Those earlier commits remain accessible in the repository history, but this
changelog does not describe them.

## Unreleased

## 3.2.0 — 2026-10-13

### Added

- Optional trailing slashes in single-line tags: tags such as `[next]`, `[goto]`, `[pause]`, `[image]`, and `[sound]` can now be written without a trailing slash (`/`).
- Direct CSS class styling for `[image]` tags: authors can specify CSS class names directly (`class="..."` or `.classname`) without having to provide a dedicated CSS file path.
- `show` animation class for `[image]` elements during visual entrance transitions.
- Fallback transition styling and safety timer (350ms) for `[image hide]` when no custom `hide` animation keyframes are provided.
- Asset preloading: image assets referenced in upcoming dialogue steps are preloaded asynchronously prior to rendering the dialogue, preventing visual stutter and layout flash.
- Delta variable transmission: the engine now tracks variable usage and mutations during script execution and sends only changed variables across WebSockets, reducing payload size.
- Explicit source location tracking (`sourceFile`, `sourceLine`, step index) in runtime error diagnostics for TypeScript and Python script blocks.

### Changed

- Replaced internal `dr-hide-active` CSS class with the standard `hide` class for image exit animations (retaining backwards compatibility).
- Menu screens (Save, Settings) are now rendered as overlays without unmounting the active game screen, preventing unwanted retriggering of image entrance animations.

### Fixed

- Fixed screenshot generation for save game slots using `html-to-image` rendering and canvas fallbacks.
- Fixed `pendingScreenshot` state leakage across save slots.
- Fixed variable interpolation (`{var}`) preservation for CSS object paths in save state serialization.
- Fixed save menu displaying empty slots when accessed from the main menu before starting a game (WebSocket connection and identification are now established on client initialization).
- Fixed dialogue repeating twice after loading a save by preserving the cursor advance performed by `replayTo` when materializing `firstFrame`.

## 3.1.0 — 2026-09-21

The first release with a stable, documented public interface. From this point
forward, the engine has three contracts that consumers can rely on:

- the `.dreamrun` scenario syntax,
- the `.dreamsave` save format,
- the `GameState` client API.

### Added

- `.dreamrun` scripting language with dialogue, choices,
  conditionals, subroutines, jumps, pauses, image layers, and audio commands.
- Expression evaluation: `{variable}` interpolation in dialogue
  and inline TypeScript and Python blocks.
- `Character` and `Ramp` runtime types exposed to
  scenario authors.
- Audio subsystem built on the Web Audio API: gapless looping,
  sample-accurate ramps, click-free fade transitions.
- Layered image system with per-layer CSS attachments.
- Save system with nine visible slots per page and up to twenty pages.
- Settings screen with text speed and master volume.
- Error screen with copyable diagnostics.
- Loading overlay with debounced appearance for slow networks.

### Notes

- The engine runs as a SvelteKit application on Node.js.
- Python blocks execute in a browser-side Pyodide sandbox.
- Configuration files under `static/config/` are loaded as
  ES modules.