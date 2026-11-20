# Patches

Any `*.patch` file in this folder is applied to the ScummVM source, in name order,
before the build. Make them with `git diff` against the ScummVM tag set in the workflow
(default `v2026.4.0`).

Current patches:

1. `0001-all-files-access-and-no-picker-fallback.patch`: declares "All files access" in the manifest (so the app can appear in the TV's special-permissions list), opens that settings screen on launch, shows the one-line `adb` command if the TV has no such screen, and replaces the "no app can do this" failure of ScummVM's SAF folder picker with a clear message.
2. `0002-start-in-downloads-and-bigger-gui.patch`: the folder browser starts in `/sdcard/Download`, and the menus are 1.5x bigger by default (change it in Options if needed).

3. `0003-use-builtin-folder-browser.patch`: "Add Game" always uses ScummVM's own folder browser instead of the Android system picker, which does not exist on Android TV. This is what caused "you don't have an app that can do this".

4. `0004-storage-check-dialog.patch`: adds a storage-check diagnostic (no longer shown automatically after patch 5; the code is kept for future debugging).
5. `0005-shared-storage-roots-no-add-folder.patch`: the folder browser's top level now lists "Downloads" and "Internal storage"; the unusable "Add folder" entry is removed; the browser starts in Downloads unless the last used folder is already inside shared storage.

The workflow also builds Ogg and Vorbis from source so Ogg audio works.

Planned next:

3. Render to a fixed 1080p or 720p surface so 4K TVs do not make ScummVM draw at 3840x2160.
4. Make mouse input work on Android TV (including mice that report as trackballs).
5. Auto-scan Downloads for supported games and show a simple grid.

## 0006-steadier-audio-buffering.patch
Targets crackle/static in the sound (seen with Sam & Max's Ogg `.sog` speech): shared, non-low-latency
Oboe stream, ~250 ms ring buffer ahead of the output, producer thread wakes every >=20 ms instead of
every few ms, and the producer thread asks for audio priority. Costs a little audio latency (not
noticeable in these games).

Update to 0006: the first version buffered ~250 ms and caused a noticeable delay before speech. It now
uses a ~100 ms buffer with the stock low-latency stream.
