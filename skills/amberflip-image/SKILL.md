---
name: amberflip-image
description: Edit pictures with Amberflip, offline - crop, rotate, mirror or resize (also to social media sizes), adjust exposure and colours, put a photo on a background, make a collage, blur or cover faces and areas, draw arrows, boxes, text and numbers on a screenshot. Use when the user wants to change a picture on this computer.
compatibility: Requires the Amberflip app or amberflip-cli on this computer. Works offline.
---

# Pictures with Amberflip

> **Needs Amberflip installed** on this computer: the app or `amberflip-cli`, from https://amberflip.com (free 3-day trial, then a licence key). If Amberflip is missing, tell the user to install it; these steps do nothing without it.

Amberflip is connected as the `amberflip` MCP server and edits pictures in-process. Pass absolute
paths. `inspect` gives a picture's `size` (width, height) in pixels; positions are pixels from
the top left.
The names below are its tools; the `amberflip` skill gives the same with `amberflip-cli`.

| Task | Call |
| --- | --- |
| Crop, rotate, mirror | `image_crop` with `area`, `turn`, `flip_h`, `flip_v`, `size` (e.g. `{"w": 1080, "h": 1350}`, exactly) |
| Resize, keeping proportions | `image_resize` with `width`, `height` (both: fits inside them) or `percent` |
| Brighter, warmer, sharper | `image_adjust` (`exposure` -3..3; `contrast`, `saturation`, `temperature`... -100..100; `sharpness` 0..100) |
| On a background | `image_frame` with `gradient` (1-9), `color` or `image`; `padding` in percent; `ratio` like `"4:5"` |
| Collage | `image_collage` with the files in order; `layout` grid, row, column or featured; `ratio`, `width` |
| Hide faces, plates, text | `image_redact` with `areas` (`x, y, w, h`, `outline`: rect, ellipse or free with `points`), `blur: true` or `color` |
| Arrows, boxes, text, numbers | `image_annotate` with `marks`, e.g. `[{"shape": "arrow", "from": [10, 10], "to": [200, 120]}, {"shape": "text", "at": [20, 300], "text": "Here", "size": 48, "color": "#ffffff"}]` |
| Other format | `convert` with `to: "png"`, `"jpg"`, `"webp"`, `"avif"`, `"pdf"`... |
| Smaller | `compress` (`max_side` to make it smaller too) |

Shapes for `image_annotate`: arrow, rectangle, ellipse, pen, highlight, text, callout, number,
magnifier, picture (with a `path`). Results are new files next to the original
(`... (cropped).png`); report the paths in `outputs`.
