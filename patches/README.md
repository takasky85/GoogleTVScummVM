# Patches

Any `*.patch` file in this folder is applied to the ScummVM source, in name order,
before the build. Make them with `git diff` against the ScummVM tag set in the workflow
(default `v2026.4.0`).

Current patches:

1. `0001-all-files-access-and-no-picker-fallback.patch`: declares "All files access" in the manifest (so the app can appear in the TV's special-permissions list), opens that settings screen on launch, shows the one-line `adb` command if the TV has no such screen, and replaces the "no app can do this" failure of ScummVM's SAF folder picker with a clear message.
2. `0002-start-in-downloads-and-bigger-gui.patch`: the folder browser starts in `/sdcard/Download`, and the menus are 1.5x bigger by default (change it in Options if needed).

Planned next:

3. Render to a fixed 1080p or 720p surface so 4K TVs do not make ScummVM draw at 3840x2160.
4. Make mouse input work on Android TV (including mice that report as trackballs).
5. Auto-scan Downloads for supported games and show a simple grid.
