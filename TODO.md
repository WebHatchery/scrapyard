# TODO — Scrapyard Planet

## Controls and layout

- [ ] Add visible touch controls for movement, repair/upgrade, held scrap gathering, tutorial continuation, pause, view switching, and system details; wire the drawn action chips to input (`src/ui/gameplay_input.rs`, `src/ui/ship_hud.rs`, `src/ship/player.rs`; §7.5).
- [ ] Make settings, game-over recovery, victory continuation, and upgrade purchases/next-run navigation operable by tap. Use toolkit widgets with release activation and shared drawing/hit-test bounds (`src/ui/ui_input.rs`, `src/ui/pause_menu.rs`; §7.4–7.5).
- [ ] Update tutorial/HUD prompts, `README.md`, and `game_page.json` to name the exact visible controls alongside shortcuts; correct Escape's pause behavior and label the exterior view as automated defense with a visible return control (§7.5).
- [ ] Fix responsive layout: pause options overflow the 320px panel, settings Back overlaps its footer, and HUD/routing dimensions assume fixed pixel sizes. Share layout calculations with input and verify desktop and touch-sized browser viewports (§7.5).
- [ ] Add persisted key remapping and derive shortcut hints from active bindings, checking the toolkit for reusable support first.

## State and regression coverage

- [ ] Move gathering timers/rewards, repair/upgrade, routing, tutorial advancement, and settings mutations out of UI input handlers into explicit state actions. Pass movement intent and simulation `dt` instead of polling keys/frame time in game logic; replace dummy purchase/toggle events with meaningful intents (§5.1, §7).
- [ ] Expose testable game logic through `src/lib.rs`, have the binary use it, and migrate `src/state/game_actions/tests.rs` and `src/ship/interior/tests.rs` into `tests/` before adding coverage. Preserve the existing payout, threat-tier, and layout regressions (§11.4).
- [ ] Add isolated, deterministic regression fixtures for power-routing events/capacity, life-support timers, threat changes, rapid toggles, low-hull escape, salvage, and combat. Inject settings/profile data and isolate gameplay RNG from visual effects; target at most five cases per cohesive feature without dropping useful coverage (§11).

## Data and error handling

- [ ] Move remaining balance/configuration values from `src/simulation/constants.rs`, player stats, ship initialization, and engine updates into typed JSON loaded through the toolkit. Reconcile unused `assets/enemies.json`/`assets/stats/weapons.json` with the live combat and spawn rules (§5.3).
- [ ] Add semantic validation for module definitions, ship dimensions/IDs/connections/grid references, tutorial targets, upgrades, and contract multipliers. Honor the ship JSON's `player_start_room`; report invalid content clearly instead of silently creating empty layouts/modules (§5.3, §6).
- [ ] Move hardcoded UI labels, objectives, event text, and room names into JSON; remove `tutorial_prompt` overrides that bypass `assets/tutorial.json` (`src/ui/ship_hud.rs`, `src/ui/ui_renderer.rs`, `src/ship/interior.rs`; §5.3).
- [ ] Surface settings load/save failures and retain the underlying save-slot load error (`src/data/settings.rs`, `src/ui/gameplay_input.rs`, `src/state/mod.rs`); show save success only after persistence succeeds (§6).

## Code and artifact maintenance

- [ ] Remove blanket dead-code/lint suppressions and unused members, including the BFS path cache/helper, legacy `TutorialStep`, `_scale_x`/`_scale_y`, and unused parameters. Remove the no-op music placeholder and misleading music control until playback exists; replace scratch commentary with concise rationale (§1.4, §10.2).
- [ ] Split oversized functions such as `draw_rooms`, `draw_settings_panel`, and `update_engine` into cohesive helpers. Extract responsibilities from `ship_hud.rs` (753 lines), `game_actions.rs` (681), and `world_renderer.rs` (606) before expansion; migrate affected `mod.rs` roots to named files, add missing `//!` purpose docs, and correct the source-gate test's obsolete “non-test lines” comment (§2.2–2.3, §4.1, §9.2).
- [ ] Consolidate retained captures from `screenshots/` directly into `docs/verification/`, replacing duplicate screen/state captures and removing superseded images (§12).
