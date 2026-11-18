---
name: amberflip-video
description: Edit videos with Amberflip, offline - trim, cut into clips, crop or rotate, speed up or slow down, join clips, blur or cover faces and areas, save frames as pictures, extract the sound, make a GIF. Use when the user wants to change a video file on this computer.
compatibility: Requires the Amberflip app or amberflip-cli on this computer. Works offline.
---

# Video with Amberflip

> **Needs Amberflip installed** on this computer: the app or `amberflip-cli`, from https://amberflip.com (free 3-day trial, then a licence key). If Amberflip is missing, tell the user to install it; these steps do nothing without it.

Amberflip is connected as the `amberflip` MCP server and edits video with FFmpeg on this
computer. Pass absolute paths.
The names below are its tools; the `amberflip` skill gives the same with `amberflip-cli`.

First call `inspect` with the video: it gives `duration` in seconds and `size` (width, height) in
pixels. Times are seconds; areas are pixels of the picture as shown, from the top left
(`{"x": 0, "y": 0, "w": 640, "h": 360}`).

| Task | Call |
| --- | --- |
| Keep a section | `video_trim` with `from`, `to` (seconds); `exact: false` is fast but cuts at key frames |
| Cut into clips | `video_split` with `at` (a list of seconds) or `parts` |
| Crop, rotate | `video_crop` with `area`, `turn` (0, 90, 180, 270) and `size` to scale the result, e.g. 1080×1350 |
| Faster, slower | `video_speed` with `factor` (0.25 to 4); `keep_pitch: false` changes the voices |
| Join | `video_join` with the files in order (fitted to the first one's size) |
| Hide faces, plates | `video_redact` with `areas` (each `x, y, w, h` and `from`, `to`), `blur: true` or `color` |
| Smaller picture, HD to 720p | `video_resize` with `height` or `width` (proportions kept; both: fits inside them) |
| No sound | `video_mute` (the picture is copied as it is) |
| Burn in subtitles | `video_subtitles` with `file` (SRT, VTT, ASS or SSA in UTF-8), `size`; needs FFmpeg with libass (`list_operations` marks it `available: false` without it) |
| Pictures of moments | `video_frames` with `at` (seconds) or `every` |
| Sound only, GIF, other format | `convert` with `to: "mp3"`, `"gif"`, `"mp4"`, `"webm"`... |
| Smaller file | `compress` (with `video_height` for a lower resolution) |
| Louder or even sound | `audio_normalize` works on videos too |

To find where a face is, save a frame with `video_frames` and look at it before choosing an area.
Results are new files next to the original (`... (trimmed).mp4`); report the paths in `outputs`.
