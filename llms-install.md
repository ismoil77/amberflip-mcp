# Installing Amberflip for Cline

Amberflip is an MCP server that converts and edits files on the user's own computer, offline
(pictures, video, sound, PDFs, documents). It is started by `npx`; there is nothing to clone or
build. This repository contains no source code.

## 1. Check Node.js

Run `node --version`. It must print v18 or newer. If `node` is missing, ask the user to install
Node.js from https://nodejs.org and stop until they have.

## 2. Add the server

Open Cline's MCP settings file (`cline_mcp_settings.json`, reached from the MCP Servers panel,
Configure) and add this entry to `mcpServers`, keeping any servers that are already there:

```json
{
  "mcpServers": {
    "amberflip": {
      "command": "npx",
      "args": ["-y", "amberflip-cli", "mcp"]
    }
  }
}
```

On Windows, if Cline cannot start `npx`, put the full path printed by `where npx` in `command`.

The first start downloads the package for this system from npm: Windows x64, Linux x64 and macOS
(Apple silicon and Intel) are supported. On Linux, video and sound need FFmpeg
(`apt install ffmpeg` and the like); on Windows and macOS it is included.

## 3. Check it works

Call the tool `license_status`. It answers whether the computer is in the free trial (3 days from
first use, with every tool) or has a licence key, and where to buy one: https://amberflip.com/buy.
Never ask for or type a licence key in the chat: the user enters it themselves with
`amberflip-cli activate KEY`, or in the Amberflip app.

## 4. Use it

- Pass absolute paths in `files`.
- Call `inspect` first when an edit needs numbers (a picture's size, a video's length in seconds,
  a PDF's page count), and `list_operations` to see what applies to the files.
- Results are new files next to the originals, or in `output_dir`. Originals are never changed.
- The tools are: `list_operations`, `inspect`, `convert`, `run_operation`, `compress`,
  `strip_metadata`, `metadata`, `zip`, `extract`, `license_status`, `pdf_pages`, `pdf_split`,
  `pdf_merge`, `pdf_redact`, `video_trim`, `video_crop`, `video_speed`, `video_split`,
  `video_frames`, `video_join`, `video_redact`, `video_resize`, `video_mute`, `video_subtitles`,
  `audio_trim`, `audio_normalize`, `audio_bleep`, `audio_channels`, `audio_visualize`,
  `audio_fade`, `audio_bitrate`, `image_crop`, `image_adjust`, `image_frame`, `image_collage`,
  `image_redact`, `image_annotate` and `image_resize`.

Nothing is uploaded: the server has no network listener and only the licence check goes online.
See README.md for the trial and licence terms and the privacy statement.
