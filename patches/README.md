# Patches

Any `*.patch` file in this folder is applied to the ScummVM source, in name order,
before the build. Make them with `git diff` or `git format-patch` against the
ScummVM tag set in the workflow (default `v2026.4.0`).

Planned patches, one per file, so each can be tested on its own:

1. `0001-fixed-size-surface.patch`: render to a fixed 1080p or 720p surface so 4K TVs do not make ScummVM draw at 3840x2160.
2. `0002-tv-mouse.patch`: make mouse input work on Android TV (including mice that report as trackballs).
3. `0003-auto-scan-launcher.patch`: scan Downloads for supported games and show a simple grid.

No patches yet, so the first builds are stock ScummVM with only the engine trim from `build/configure-flags.txt`.
