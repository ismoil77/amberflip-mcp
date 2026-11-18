---
name: amberflip-pdf
description: Work with PDF files using Amberflip, offline - merge PDFs, split a PDF by page ranges, reorder, delete, repeat or rotate pages, turn pages into PNG or JPG images, extract the text, make it grayscale, compress it, or turn pictures and documents into PDF. Use when the user wants to change a PDF on this computer.
compatibility: Requires the Amberflip app or amberflip-cli on this computer. Works offline.
---

# PDFs with Amberflip

> **Needs Amberflip installed** on this computer: the app or `amberflip-cli`, from https://amberflip.com (free 3-day trial, then a licence key). If Amberflip is missing, tell the user to install it; these steps do nothing without it.

Amberflip is connected as the `amberflip` MCP server. Always pass absolute paths. Pages count
from 1; `inspect` gives a PDF's page count.
The names below are its tools; the `amberflip` skill gives the same with `amberflip-cli`.

Page lists read like a print dialog: `5`, `2-4`, `4-2` (backwards), `7-` (to the end), `-3`
(from the start), separated by commas; in `pdf_pages` an entry can be turned with `@90`, `@180`
or `@270`.

| Task | Call |
| --- | --- |
| Merge PDFs, in the order given | `pdf_merge` with the files |
| Reorder, delete, repeat or rotate pages | `pdf_pages` with `pages` (`"3,1-2,5-"`), `remove` (`"4"`), `rotate` (`"2:90"`) |
| Split by ranges or every N pages | `pdf_split` with `ranges` (`"1-3,4-"`) or `every` |
| One file per page | `pdf_split` with no ranges |
| Pages as pictures | `convert` with `to: "png"` or `"jpg"` |
| Text only | `convert` with `to: "txt"` |
| Grayscale | `run_operation` with `tool.pdf_grayscale` |
| Smaller file | `compress` |
| Pictures or documents to PDF | `convert` with `to: "pdf"` on those files |
| Author, title | `metadata` or `strip_metadata` |
| Remove text for good (names, accounts, signatures) | `pdf_redact` with `areas`: `[{"x": 72, "y": 90, "w": 200, "h": 20, "pages": "1,3"}]`; needs Ghostscript |

For `pdf_redact`, areas are points from the top left of the page as shown (72 to the inch); `inspect`
gives `page_sizes`. To find where the words are, `convert` the page to `png` and look at it, scaling
by its size. A page with an area becomes a picture, so its text can no longer be selected or
searched, and the bookmarks, attachments, form list and document information go from the whole
file. Say so to the user. Never cover text by drawing over it some other way: it stays in the file.

Results with several files (pages, parts) come back as a folder path. Report every path in
`outputs`; the originals are never changed.
