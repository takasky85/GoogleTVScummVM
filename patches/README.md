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
Targets crackle/static that starts after a while of playing (any audio format). The audio thread now
keeps only ~50 ms of mixed audio buffered ahead (minimum delay) and, each time the output runs dry while
audio was being produced, buffers 40 ms more, up to 250 ms. A device that never glitches keeps the
minimum delay; one that does stops glitching. Also: >=10 ms production chunks and an audio-priority
request for the mixing thread. (Tremor was tried and did not help: static happened with any format.)

## 0007-preload-vorbis-clips.patch
Ogg Vorbis clips up to 8 MB (Sam & Max speech lines, effects) are read fully into memory before decoding,
and bigger streams (music) get a 256 KB read buffer, so slow Android shared-storage reads can't stall the
audio thread.



## Which ScummVM version?
`v2026.4.0` (and 2026.3.0) use a new Oboe-based Android audio system that crackled on the onn box.
`v2026.2.0` is the last release with the older audio thread (JNI AudioTrack), so the workflow now builds
that by default, with the patches in `patches/v2026.2.0/` (one combined patch: all-files access, Downloads
browser, bigger GUI, Vorbis clip preloading). The numbered patches 0001-0007 here are for v2026.4.0.

The v2026.2.0 patch also adds a crash reporter: if the previous run crashed, the app reads its own system
log at startup and shows the crash lines in a dialog (and saves them to Download/gtvscumm-crash.txt),
holding the native start until the dialog is dismissed. For devices without adb.

v2026.2.0 patch 0003: when the activity finishes (Quit), the process is ended after the native thread has
finished, so re-opening the app starts from a clean process (re-opening in the same process left a black
screen on the TV).

v2026.2.0 patch 0004: press F12 (keyboard) in the app to see a debug dialog with the tail of ScummVM's own log,
the start of its config file and recent system-log warnings (for startup problems without adb).

Note: the crash reporter in 0001 used to return early when the previous run had not crashed
without releasing the start-up latch, so the ScummVM thread never started (black screen on every
launch with no crash in the log). It now always releases the latch.

### 0005-mouse-speed-option (v2026.2.0 only)
Adds Options > Android > "Mouse speed" (1x to 6x, default 2x). Android gives the app absolute pointer
positions, so the movement between two mouse events is multiplied and kept inside the game area.

### Game icons
The "Add game icons to gui-icons.dat" workflow step adds the per-game icons listed in
build/game-icons.txt (from the public scummvm-icons repository) to the bundled gui-icons.dat, so the
launcher grid view has tiles without the large icon pack download.
