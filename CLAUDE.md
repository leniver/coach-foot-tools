# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A small French-language collection of static web tools for sports training (the repo is `coach-foot-tools`). All served assets live under `public/`; the repo root holds only docs/config (`README.md`, `CLAUDE.md`, `LICENSE`). Each tool is its own HTML page; `public/index.html` is a tool-selector landing page that links to them.

- `public/index.html` — landing page. Renders a tile per tool from a `tools` array in its inline `<script>`; **add a tool by appending one entry** (`title`, `description`, `icon`, `href`).
- `public/chrono.html` — `Chronométrage enfants`: manage named children-lists, load one, select which children to time, run simultaneous stopwatches, stop each child individually, then auto-generate two time-balanced groups from the results.
- `public/exercices.html` — training drills as a fixed `exercices` array in its inline `<script>` (`categorie`, `titre`, `duree`, `description`); rendered as cards grouped by category, with a sticky category nav. **Add a drill by appending one entry**; categories appear in first-seen order.
- `public/styles.css` — the shared design system (see Conventions).

## Running / testing

No build step, no dependencies, no test suite. Open `public/index.html` directly in a browser (`file://`) or serve `public/` statically (it is the web root / Apache `DocumentRoot`). All served files sit together in `public/`, so the relative `styles.css` / page links resolve as-is.

## Architecture

The chrono app (`public/chrono.html`) holds everything inline: a `<style>` block (chrono-specific rules only; shared rules are in `styles.css`), three tab panels in the markup, and a single `<script>` of vanilla JS (no framework, no modules).

- **UI is three tabs** — `Enfants` (a manager for named children-lists: create / edit / load / delete), `Chrono` (select + run timers on the loaded list), `Résultats` (rankings, history, balanced groups). `showTab()` toggles `.active` classes; tabs are plain markup, not routed.
- **State is a handful of module-level globals**, not a store: `children` (array of `{name, time}` where `time` is ms or `null`), `selectedChildIndexes`, `activeChildrenState` (per-running-timer objects), `savedListNames` (sorted roster names, so buttons reference rosters by index instead of escaping names in `onclick`), plus `startTime` / `chronoRunning` / `timerInterval` / `editingListName`. Functions mutate these globals directly then call render functions.
- **Rendering is full innerHTML re-renders.** `renderAll()` fans out to `renderChildrenSelector / renderChronoChildren / renderResults / renderHistory / renderSavedLists`, each rebuilding its container's `innerHTML` from the globals. There is no diffing — after any state change, call the relevant render function(s). User-supplied names must always pass through `escapeHtml()` when interpolated into HTML.
- **Timing**: one `setInterval` (20ms) updates all running timers off a shared `startTime`; each child's elapsed is `Date.now() - startTime` captured at stop. When every active child is stopped, the interval is cleared and the selection resets.
- **Persistence — two separate `localStorage` keys.** The in-progress session (`{children, selectedChildIndexes}`) is under `chronoChildrenState` via `saveState()` / `restoreState()`; call `saveState()` after any mutation that should survive reload. Named rosters (`{ name: [names…] }`) are under `chronoSavedLists` via `loadSavedLists()` / `persistSavedLists()`. On first load `seedDefaultListIfEmpty()` seeds one roster from `defaultChildren`; loading a roster (`useNamedList` → `applyChildren`) starts a fresh session, preserving times of matching names.
- **Balanced groups** (`createBalancedGroups`): greedy — sort timed children slowest-first, assign each to whichever group currently has the smaller total time.

## Conventions

- **Shared design system in `styles.css`.** Both pages link it. It defines the palette and sizing as CSS custom properties on `:root` (`--primary`, `--success`, `--danger`, `--surface`, `--border`, `--radius-card`, …) plus base styles for `body`, `main`, `h1`–`h3`, form fields, `button` (+ `.full`/`.secondary`/`.success`/`.warning`/`.danger`), `.card`, `.muted`, `.actions`, `table`, and the shared `@media (min-width: 850px)` rules. Each page's inline `<style>` holds **only** that page's own selectors and should reference the `var(--…)` tokens rather than hard-coded hex, so all tools stay visually consistent. Add new shared primitives to `styles.css`; add page-specific layout inline.
- All user-facing text is French. Keep new UI strings French.
- The fallback name list is the `defaultChildren` array at the top of the script — edit it there to change the default roster.
- Time format is `MM:SS.cc` (centiseconds) via `formatTime()`; CSV export uses `;` separators (`exportCsv()`).
