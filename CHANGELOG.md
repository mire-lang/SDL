# sdl changelog

## [1.3.2] - 2026-11-25

### Added
- **`sdl3::conv` and `sdl2::conv`** — i64-handle wrappers over the runtime's
  pointer-based byte access: `alloc_raw`, `free_raw`, `read_u8/u16/u32/u64`
  and `write_u8/u16/u32/u64`. SDL's C entry points take pointer values as
  integers, so the conversion happens at this edge. Both modules are exported
  from their nested manifests.
- **Runtime symbol collision fixed.** Declaring `rt_alloc_raw` and the
  `rt_read_*` helpers locally with `:i64` collided with `mire::aux::pal`
  whenever a program loaded both. The binding now calls `pal::*` through the
  `conv` module instead of redeclaring the externs.

### Changed
- **One changelog.** The release history lives in this file; the duplicate
  `docs/CHANGELOG.md` is gone. Its pre-1.3 notes are folded into the entries
  below.

## [1.3.1] - 2026-09-22

### Changed
- **Avenys 4.x build manifest** — `owl.toml` declares `artifact = "shared"`,
  `runtime = "minimal"`, `target`, `panic = "abort"`, `incremental`,
  `debug-info`, the expanded `[paths]` and `[cfg]` sections, and explicit
  `mire`/`kioto` path dependencies.
- **Lockfile regeneration** — Regenerated with owl v1.1.1 (includes `["owl@lock"] version = "3"` + manifest-sha256)
- **Dependencies** — Updated to kioto 2.5.0, mire 1.0.0, owl tool 1.1.2

## [1.3.0] - 2026-09-17

### Added
- **SDL3 joystick/gamepad/haptic/sensor subsystems** — Full FFI coverage for:
  - Joystick: open/close, axes/buttons/hats, GUID, power level
  - Gamepad: mapping, sensor, touchpad, rumble/triggers
  - Haptic: effects (constant/sine/periodic/condition/ramp), play/stop/pause
  - Sensor: accelerometer/gyro, data rate, type
- **Struct helpers** — `create_rect`/`create_frect`/`create_point`/
  `create_fpoint`/`create_color` allocate SDL C structs through the runtime,
  with field getters and setters and a NULL-safe `destroy`.
- **Headless modules** — `version`, `power`, `filesystem`, `keyboard`, `mouse`,
  `messagebox`, `system`, `misc` and `cpuinfo`, plus display-free tests.
- **SDL3 headless tests** — `tests/sdl3_smoke.mire`, `tests/sdl3_extensions.mire`
- **SDL2 cleanup** — Removed legacy SDL2 window/renderer wrappers, consolidated to SDL3

### Changed
- **Version bump** — 1.2.0 → 1.3.1 (SDL3 subsystems + headless tests)
- **Meta.toml** — `language = "mire avenys v3.24.27"`

## [1.2.0] - 2026-09-15

### Added
- **SDL3 renderer** — `SDL_CreateRenderer`, `SDL_SetRenderDrawColor`, `SDL_RenderClear`, `SDL_RenderPresent`
- **SDL3 window** — `SDL_CreateWindow` with Wayland compositor workaround (renderer + present required before first frame)
- **SDL3 events** — Keyboard, mouse, window events via `SDL_PollEvent`
- **Full SDL3 FFI** — `SDL_Init`, `SDL_Quit`, `SDL_Delay`, `SDL_DestroyRenderer`, `SDL_DestroyWindow`
- **SDL2 fallback** — Legacy SDL2 module preserved for compatibility

### Changed
- **Entry point** — SDL3 modules now the primary API; SDL2 deprecated
- **SDL3 boolean returns** — SDL3 C API uses `_Bool` for success/failure; module return types updated to `:bool`

### Fixed
- **Wayland buffer commit** — Renderer creation + present required before window maps
- **SDL3 boolean ABI** — x86_64 `_Bool` return in `al` register vs full `rax` for int

## [1.0.1] - 2026-08-10

### Fixed
- **sdl2** — `create_window` parameter type `:str` (was `:&str`)

## [1.0.0] - 2026-08-05

### Added
- **sdl2 module** — SDL2 video + render FFI via `SDL_CreateWindow`, `SDL_CreateRenderer`, `SDL_RenderPresent`, `SDL_Delay`
- **Basic event loop** — `SDL_PollEvent` wrapper