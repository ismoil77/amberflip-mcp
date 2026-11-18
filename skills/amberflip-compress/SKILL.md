---
name: amberflip-compress
description: Make pictures, videos, audio files or PDFs smaller with Amberflip, offline. Use when the user wants to compress or shrink a file, reduce its size for email or upload, or fit it under a size limit.
compatibility: Requires the Amberflip app or amberflip-cli on this computer. Works offline.
---

# Compress files with Amberflip

> **Needs Amberflip installed** on this computer: the app or `amberflip-cli`, from https://amberflip.com (free 3-day trial, then a licence key). If Amberflip is missing, tell the user to install it; these steps do nothing without it.

Amberflip is connected as the `amberflip` MCP server and compresses files on this computer.
The names below are its tools; the `amberflip` skill gives the same with `amberflip-cli`.

1. Collect the absolute paths of the files (pictures, videos, audio, PDFs).
2. Call `compress` with `files`.
   - The default is balanced: much smaller, quality kept. `strong: true` gives the smallest
     file at lower quality.
   - A target: `target_percent` (5 to 95, a share of each original) or `target_mb` (a size,
     "under 10 MB" is `target_mb: 10`). Pictures search the best quality that fits, video and
     sound aim their bitrate at it, PDFs step down until one fits.
   - Smaller pictures: `max_side` in pixels (or `half: true`); smaller video: `video_height`
     (1080, 720, 480).
3. Report the paths in `outputs` (named `... (compressed).ext`) and, if you can, the new sizes
   (`inspect` gives `bytes`). A result that missed its target is still written and is listed in
   `over_target` with its `bytes` and `target_bytes`: tell the user, do not call it done.

Video and sound need FFmpeg, PDFs need Ghostscript; pictures need nothing else.
