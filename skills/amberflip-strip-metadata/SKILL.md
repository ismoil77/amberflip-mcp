---
name: amberflip-strip-metadata
description: Remove or change hidden metadata in photos, videos, audio files and PDFs with Amberflip, offline. Use when the user wants to strip EXIF, GPS location, camera details, tags, author or title before sharing a file, clean or anonymise a file, or change a title or artist tag.
compatibility: Requires the Amberflip app or amberflip-cli on this computer. Works offline.
---

# Metadata with Amberflip

> **Needs Amberflip installed** on this computer: the app or `amberflip-cli`, from https://amberflip.com (free 3-day trial, then a licence key). If Amberflip is missing, tell the user to install it; these steps do nothing without it.

Amberflip is connected as the `amberflip` MCP server.
The names below are its tools; the `amberflip` skill gives the same with `amberflip-cli`.

Everything at once:
1. Call `strip_metadata` with the absolute paths in `files`. Each result is a copy named
   `... (no metadata).ext`; the original keeps its metadata. Photos keep their orientation.

Only some fields:
1. Call `inspect` with the file. Its `fields` list each field's `name` (`exif/Model`,
   `gps/GPSLatitude`, `file/Title`, `track1-audio/title`...), its `value`, and whether it is
   `editable`.
2. Call `metadata` with `files` and any of: `remove` (a list of names), `set`
   (`{"file/Title": "Holiday"}`, text fields only), `remove_camera: true` (camera, its settings,
   GPS and dates), `remove_all: true`. The copy is named `... (edited).ext`.

Report the paths in `outputs`. A file under `failed` could not be written, for example a picture
format whose metadata Amberflip does not write; convert it to JPG or PNG first with `convert`.
