# SDA Hymnal Pro — Score Images

Sheet music images for the **SDA Hymnal Pro** Android app.

This repository hosts the score image bundle as a GitHub Release asset. The app
downloads the bundle on demand, verifies it, and unpacks it locally. Nothing in
this repo is compiled into the APK.

---

## Contents

- **695 score images**, one per hymn, numbered `001.png` through `695.png`
- Hymns that span multiple pages are **pre-merged vertically** into a single
  tall image, so the app only ever deals with one file per hymn
- All files are PNG
- Total bundle size: ~18 MB (uncompressed)

---

## Download

Latest bundle:
https://github.com/<OWNER>/<REPO>/releases/download/v1/scores.zip

text

The app reads this URL from `scores_manifest.json` (see below). Do not hardcode
it anywhere else.

---

## Manifest contract

The app fetches a small JSON manifest before downloading the bundle. The
manifest lives alongside the release and has this shape:

```json
{
  "version": 1,
  "format": "image",
  "baseUrl": "https://github.com/<OWNER>/<REPO>/releases/download/v1/",
  "archive": "scores.zip",
  "sizeBytes": 0,
  "sha256": "<hash>"
}
Field reference:

Field	Type	Notes
version	int	Manifest schema version. Bump to invalidate cached downloads.
format	string	"image" today. Reserved for "musicxml" in a future release.
baseUrl	string	Prefix applied to archive to build the full download URL.
archive	string	Filename of the release asset.
sizeBytes	int	Exact byte size of the archive. Used for the download prompt.
sha256	string	Lowercase hex SHA-256 of the archive. Verified after download.
If you rebuild the archive, you must update sizeBytes and sha256 in the
manifest, and bump version. A stale hash will cause the app to reject the
download.

Building the archive
From the folder containing the renamed images:

cmd
tar -a -c -f ..\scores.zip *.png
Files must be at the root of the zip (no scores/ subfolder). The app
unpacks flat into its internal scores directory and looks files up by name.

Get the hash for the manifest:

cmd
certutil -hashfile ..\scores.zip SHA256
Get the byte size:

cmd
dir ..\scores.zip
File naming
Pattern	Meaning
001.png	Hymn 1, single image
695.png	Hymn 695, single image
012.png	Hymn 12, possibly a vertical merge of pages
Zero-padded to three digits. No _1, _2 suffixes — multi-page hymns are
stacked before naming, not split after.

Licensing and attribution
Hymn texts and tunes in the Seventh-day Adventist Hymnal are subject to
copyright. This repository is maintained for personal use with the SDA Hymnal
Pro app and is not intended for redistribution.

If you are the rights holder and have a concern about any file here, please
open an issue.

Related
App: SDA Hymnal Pro (Android, Kotlin + Jetpack Compose)

SoundFont bundle: hosted as a separate GitHub Release, same pattern
