---
name: amberflip-convert
description: Convert files on this computer between formats with Amberflip, offline. Use when the user asks to turn a picture, video, audio file, document, spreadsheet, presentation or subtitle file into another format (PNG to JPG, HEIC to JPG, WebP to PNG, MOV to MP4, video to MP3 or GIF, WAV to MP3, DOCX to PDF, XLSX to CSV, SRT to VTT...).
compatibility: Requires the Amberflip app or amberflip-cli on this computer. Works offline.
---

# Convert files with Amberflip

> **Needs Amberflip installed** on this computer: the app or `amberflip-cli`, from https://amberflip.com (free 3-day trial, then a licence key). If Amberflip is missing, tell the user to install it; these steps do nothing without it.

Amberflip is installed on this computer and connected as the `amberflip` MCP server. It converts
files locally; nothing is uploaded.
The names below are its tools; the `amberflip` skill gives the same with `amberflip-cli`.

1. Get the absolute path of every file. If the user named a file without a path, ask for it or
   find it; Amberflip refuses relative paths.
2. Call `convert` with `files` and `to`, the extension wanted (`jpg`, `mp4`, `mp3`, `pdf`,
   `csv`...). Pass `output_dir` only if the user asked for a particular folder.
3. Tell the user the paths in `outputs`. Files under `failed` say why each could not be
   converted, for example which formats that kind of file can go to.

When unsure what a file can become, call `list_operations` with the file: entries with
`"tool": "convert"` carry the extension in `to`.

Notes:
- The originals are never changed; results get a free name next to them (`photo.jpg`,
  `photo 2.jpg`).
- A failure that names FFmpeg, LibreOffice or Ghostscript means that converter is not installed;
  say so and suggest the Amberflip app's Settings, Converters page.
- An error that says the trial has ended, or links to amberflip.com/buy, means the user must buy a
  licence or enter a key (`amberflip-cli activate KEY`). Do not retry.
