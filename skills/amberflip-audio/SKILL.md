---
name: amberflip-audio
description: Edit audio with Amberflip, offline - trim or cut silence, shorten pauses, even out loudness, bleep words, make mono or stereo and balance the channels, turn sound into a waveform video or picture, convert between MP3, WAV, FLAC, M4A, OGG. Use when the user wants to change a sound file (or the sound of a video) on this computer.
compatibility: Requires the Amberflip app or amberflip-cli on this computer. Works offline.
---

# Audio with Amberflip

> **Needs Amberflip installed** on this computer: the app or `amberflip-cli`, from https://amberflip.com (free 3-day trial, then a licence key). If Amberflip is missing, tell the user to install it; these steps do nothing without it.

Amberflip is connected as the `amberflip` MCP server and edits sound with FFmpeg. Pass absolute
paths. `inspect` gives `duration` in seconds. Results keep the file's format.
The names below are its tools; the `amberflip` skill gives the same with `amberflip-cli`.

| Task | Call |
| --- | --- |
| Keep a section | `audio_trim` with `from`, `to` (seconds) |
| Cut the silence at both ends | `audio_trim` with `auto: true` |
| Shorter pauses | `audio_trim` with `shorten_pauses: true` |
| Even loudness | `audio_normalize` (`lufs` -16 for podcasts and video, -14 for music) |
| Beep out words | `audio_bleep` with `at`: `[{"from": 3.2, "to": 3.9}]` |
| Mono, stereo, balance | `audio_channels` with `mono`, `left`, `right` (volumes 0 to 2) |
| Waveform video or picture | `audio_visualize` (`still: true` for a PNG; `shape`: landscape, portrait, square) |
| Fade in, fade out | `audio_fade` with `fade_in`, `fade_out` (seconds) |
| Another bitrate | `audio_bitrate` with `kbps` (64 speech, 128 music online, 320 the most for MP3); not WAV, FLAC, AIFF |
| Other format | `convert` with `to: "mp3"`, `"wav"`, `"flac"`, `"m4a"`... |
| Smaller file | `compress` |

Report the paths in `outputs`; the originals are never changed.
