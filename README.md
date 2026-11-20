# GoogleTVScummVM

A lightweight ScummVM build for 32-bit Android TV / Google TV boxes (for example the Walmart onn box),
limited to the classic DOS-era LucasArts SCUMM adventures at 320x200. Sideload only.

This repo does not contain ScummVM's source. Each build downloads a pinned ScummVM release,
applies the patches in `patches/`, and builds the APK on GitHub's servers.

## Get an APK

1. Open the **Actions** tab, choose **Build APK**, and click **Run workflow**.
   (Pushing changes to `build/`, `patches/` or the workflow also starts a build.)
2. When it finishes, the APK appears under **Releases** as `gtvscumm-YYMMDDHHMM.apk`.
   Release files download without a GitHub login, so a TV app such as Downloader can fetch the link directly.
3. Sideload it on the TV, for example with `adb install -r gtvscumm-YYMMDDHHMM.apk`.

## What gets built

- ScummVM tag: `v2026.4.0` by default; change it in the run dialog.
- Target: `armeabi-v7a` (32-bit ARM).
- Engine: SCUMM only. See `build/configure-flags.txt` to add or remove features.

## Build logs

After every run the workflow force-pushes a short summary to the `build-logs` branch
(`status.txt`, `errors.txt`, `head.txt`, `tail.txt`), which makes failures easy to read without opening the Actions UI.

## Signing key

`signing/sideload.keystore` is a throwaway key committed to this public repo on purpose, so every build is signed the same way
and installs over the previous one. Anyone can sign an APK with it, so do not treat it as proof of origin. Do not use it for the Play Store.

## Known limitations of the first builds

- Ogg Vorbis, MP3 and FLAC audio are built in.
- Stock ScummVM behaviour on TVs (4K rendering, mouse handling, game finding) is not fixed yet; see `patches/README.md` for the plan.
