# FH6 FastBoot
 
 Drop-in mod that skips Forza Horizon 6's startup wait the 25s black screen shown after the disclaimer/logo. Disk loading is left untouched, so the game reaches the menu as soon as loading finishes. The skip is fully automatic; no configuration required.
 
 [Download latest release](https://github.com/TigerHixTang/fh6_fastboot/releases/latest)
 
 ## How it works
 
 The hold is a busy-wait: a game thread spins on `QueryPerformanceCounter`, comparing elapsed time to a fixed deadline. It coincides with the startup logo video and overlaps real disk loading.
 
 FastBoot loads in-process as a `version.dll` proxy and hooks the timing APIs. It scopes the skip to the startup video's lifetime by watching for the video file to open and tagging the matching `BinkOpen` handle. Inside that window, it detects the hold's busy-wait by measuring its QPC call rate relative to the same boot's own baseline (not a fixed threshold), then shifts the clock forward past the deadline. The offset shifts the clock forward while leaving the rate unchanged, so frame timing is unaffected and gameplay runs at normal speed.
 
 ## Install
 
 1. Copy `version.dll` into the game folder, next to `forzahorizon6.exe`.
 2. Optionally copy `fastboot.ini` there to adjust settings.
 3. Launch the game.
 
 No launcher or injector needed. `version.dll` is a system library that the game loads; this copy proxies it and forwards calls to the real one in `System32`.
 
 Already have another `version.dll` mod? Rename it to `version_orig.dll` 鈥?FastBoot loads it alongside the skip.
 
 ## Uninstall
 
 Delete `version.dll` and `fastboot.ini` (plus `fastboot.log` if it exists).
 
 ## Configuration
 
 See [fastboot.ini](fastboot.ini) for all options. Press `F8` during the intro for a manual skip.
 
 ## Building from source
 
 Requires Rust (MSVC toolchain):
 
 ```
 cargo build --release
 ```
 
 Output: `target/release/version.dll`.
 
 ## License
 
 [GNU Lesser General Public License v3.0](LICENSE)
 
