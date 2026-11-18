---
name: amberflip-recipes
description: Step-by-step Amberflip recipes for jobs that take several tools, offline. Use when the user wants to share photos privately, fit files under an email or upload limit, clean up a voice recording, make a vertical clip for social media, hide faces in a video, remove private text from a PDF, combine scans into one PDF, or prepare a screenshot.
compatibility: Requires the Amberflip app or amberflip-cli on this computer. Works offline.
---

# Amberflip recipes

> **Needs Amberflip installed** on this computer: the app or `amberflip-cli`, from https://amberflip.com (free 3-day trial, then a licence key). If Amberflip is missing, tell the user to install it; these steps do nothing without it.

Each step takes the path the step before returned. A recipe ends on its check: repeat a step until
the check passes, then report the final paths. Names are the tools of the `amberflip` MCP server;
the `amberflip` skill gives the same in `amberflip-cli`, and what to do on an error. Intermediate
files stay next to the originals, so name the final one to the user.

## Share photos privately

1. `compress` with `max_side: 2048`.
2. `strip_metadata`.

Check: `inspect` lists no `gps/` or `exif/` fields on the result.

## Fit under a size limit

1. `compress` with `target_mb` set a little under the limit (24 for a 25 MB limit).
2. If the result is still over, repeat from the original with a lower target; for video add
   `video_height: 720`, then 480.

Check: the answer has no `over_target`. A target only aims the bitrate of video and sound, and a
short file may land above it. When nothing fits, tell the user how close it came and offer
`video_split`.

## Clean up a voice recording

1. `audio_trim` with `auto: true`, for the silence at both ends.
2. `audio_trim` with `shorten_pauses: true`, only if the user wants it tighter.
3. `audio_normalize` with `lufs: -16`.
4. `convert` with `to: "mp3"`, then `audio_bitrate` with `kbps: 64` (128 with music), if the user wants a file to publish.

Check: `inspect` gives a `duration` shorter than the original by about the silence cut. It reports no
loudness, so say that the level was set and not measured.

## Vertical clip for social media

1. `inspect` for `size` (width, height) and `duration`.
2. `video_trim` with `from`, `to`.
3. `video_crop` with `area`: the full height `h`, width `h * 9 / 16`, `x` centred or on the subject,
   `y: 0`. From 1280x720 that is `{"x": 437, "y": 0, "w": 405, "h": 720}`.
4. `video_resize` with `width: 1080`, `height: 1920`.
5. `video_subtitles` with `file`, if the user has subtitles.
6. `compress` with `target_mb`, if the platform has a limit.

Check: `inspect` gives a `size` of about 1080x1920 (sizes can round by a few pixels) and the expected `duration`.

## Hide faces or number plates in a video

1. `video_frames` with `at`, the moments the subject appears.
2. Look at the frames. Note each area in pixels and the seconds it needs covering.
3. `video_redact` with `areas` (each `x, y, w, h`, `from`, `to`) and `blur: true`.
4. `video_frames` on the result, at the same moments.

Check: look at the new frames; every subject is covered. Widen the areas and repeat from the
original if not, since a subject that moves needs a larger area or several shorter ones.

## Remove private text from a PDF

1. `inspect` for `page_sizes`, in points.
2. `convert` with `to: "png"`, which makes a folder with one `NAME-001.png` per page, and look at the
   page. Points = pixels x the page's width in points / the PNG's width in pixels.
3. `pdf_redact` with `areas` (each `x, y, w, h`, `pages`).
4. `convert` the result with `to: "png"`, and with `to: "txt"` to search for the private words.

Check: look at a new PNG of the page, the real test, since a redacted page has no text left at all;
the private words are covered and nothing else the user needs is. The text search covers the other
pages, where the same words may also appear. Tell the user that redacted pages become pictures, and that bookmarks, attachments and document information
are gone from the whole file.

## Combine scans into one PDF

1. `convert` each picture with `to: "pdf"`.
2. `pdf_merge` with the PDFs, in reading order.
3. `pdf_pages` with `rotate`, if a page lies sideways.
4. `compress`.

Check: `inspect` gives one `pages` entry per scan, and a PNG of the first page reads the right way up.

## Prepare a screenshot

1. `image_redact` over anything private, first, so that it stays covered.
2. `image_annotate` with `marks`, in pixels from the top left.
3. `image_frame` with `gradient` and `ratio`, if it is for a post.

Check: look at the result; the marks sit on what they point at.
