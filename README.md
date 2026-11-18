# Amberflip for AI apps

Amberflip converts, compresses and edits pictures, video, sound, PDFs and documents on your own
computer. Connect it to Claude, Cursor, VS Code and other AI apps over MCP, and the AI can
convert, trim, redact or shrink your files while **nothing is uploaded**: the AI app and Amberflip
share the disk, not the bytes.

This repository holds the manifests, install instructions and skills for that connection. It
contains **no source code and no program files**. The Amberflip application is proprietary, with a
free trial: see [Trial and licence](#trial-and-licence).

Website: <https://amberflip.com>

## What you get

- **38 tools** over MCP (stdio): convert between formats, compress, trim and join video, fix audio,
  crop and annotate pictures, merge, split and redact PDFs, strip hidden metadata, zip and unzip.
- **Offline.** No network listener, no window, no upload. Answers contain the paths of the new
  files, never the contents of your files.
- **Safe by default.** Results are new files next to the originals (or in a folder you choose);
  originals are never changed.
- Windows, Linux and macOS, with the same engine as the Amberflip app and the `amberflip-cli`
  command.

Example: "Make these 12 phone photos JPG, remove the GPS location, and put them in one ZIP." The AI
calls `convert`, `strip_metadata` and `zip`; the files stay on your computer.

## Install

You need one of:

1. **The Amberflip app** (<https://amberflip.com>). Open **Settings, AI apps** and press
   **Connect**. It adds Amberflip to every AI app it finds on the computer (Claude, Claude Code,
   Codex, Cursor, VS Code, Windsurf, Zed, Gemini CLI, Antigravity, LM Studio) and installs the
   skills below. Disconnect takes the entry out again.
2. **The `amberflip-cli` package from npm**, started by your AI app with `npx`. It needs
   [Node.js](https://nodejs.org) 18 or newer. The steps below use this.

Supported systems: Windows x64, Linux x64, and macOS 13 or later (Apple silicon and Intel). For
video and sound Amberflip uses FFmpeg: the Windows and macOS packages include it, on Linux install
it with your package manager (`apt install ffmpeg` and the like). Ghostscript (PDF) and
LibreOffice (office documents) are optional extras; an operation whose program is missing does not
appear in `list_operations`.

The server command is the same everywhere:

```sh
npx -y amberflip-cli mcp
```

### Claude Code

```sh
claude mcp add --transport stdio --scope user amberflip -- npx -y amberflip-cli mcp
```

Or install the plugin, which adds the server and the skills:

```
/plugin marketplace add ismoil77/amberflip-mcp
/plugin install amberflip@amberflip
```

### Claude Desktop

Add this to `claude_desktop_config.json` and restart Claude:

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

The file is at `%APPDATA%\Claude\claude_desktop_config.json` on Windows,
`~/Library/Application Support/Claude/claude_desktop_config.json` on macOS and
`~/.config/Claude/claude_desktop_config.json` on Linux. If Claude cannot find `npx`, put the full
path from `where npx` (Windows) or `which npx` (macOS, Linux) in `command`.

### Cursor

[Add to Cursor](https://cursor.com/install-mcp?name=amberflip&config=eyJjb21tYW5kIjoibnB4IiwiYXJncyI6WyIteSIsImFtYmVyZmxpcC1jbGkiLCJtY3AiXX0%3D)
(opens Cursor), or add the same JSON as above to `~/.cursor/mcp.json`.

### VS Code (Copilot)

[Add to VS Code](https://insiders.vscode.dev/redirect/mcp/install?name=amberflip&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22amberflip-cli%22%2C%22mcp%22%5D%2C%22type%22%3A%22stdio%22%7D)
(opens VS Code), or run this in a terminal (macOS and Linux):

```sh
code --add-mcp '{"name":"amberflip","type":"stdio","command":"npx","args":["-y","amberflip-cli","mcp"]}'
```

or put this in the `mcp.json` of your VS Code profile:

```json
{
  "servers": {
    "amberflip": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "amberflip-cli", "mcp"]
    }
  }
}
```

### Other apps

Any MCP client that starts a local stdio server takes the command `npx -y amberflip-cli mcp`.
Cline reads [llms-install.md](llms-install.md). With the Amberflip app installed, Codex and Gemini
CLI are connected from Settings, AI apps as well.

## Check it works

Ask your AI app: "Use Amberflip's `license_status`." It says whether the computer is in the free
trial (and for how long) or has a licence. The server's own instructions tell the AI to pass
absolute paths, call `inspect` before an edit that needs numbers (a picture's size, a video's
length, a PDF's page count), and call `list_operations` to see what applies to the files.

## Tools

Every tool takes `files` (absolute paths) and most take `output_dir` (an absolute folder for the
results; default: the user's setting in the app).

| Group | Tools |
|---|---|
| Find out | `list_operations` (what can be done with these files), `inspect` (kind, size, pages, picture size, length, metadata fields), `license_status` |
| Convert and shrink | `convert` (to another format by extension), `compress` (a preset or a target size), `run_operation` (any operation of `list_operations` by id, e.g. `tool.rotate_cw`) |
| PDF | `pdf_pages` (reorder, remove, duplicate, turn), `pdf_split`, `pdf_merge`, `pdf_redact` (remove areas for good) |
| Video | `video_trim`, `video_crop`, `video_speed`, `video_split`, `video_frames`, `video_join`, `video_redact`, `video_resize`, `video_mute`, `video_subtitles` |
| Audio | `audio_trim`, `audio_normalize`, `audio_bleep`, `audio_channels`, `audio_visualize`, `audio_fade`, `audio_bitrate` |
| Pictures | `image_crop`, `image_adjust`, `image_frame`, `image_collage`, `image_redact`, `image_annotate`, `image_resize` |
| Metadata | `strip_metadata` (remove EXIF, GPS, tags, PDF info), `metadata` (change or remove single fields) |
| Archives | `zip`, `extract` (ZIP, TAR, TAR.GZ) |

`amberflip-cli mcp --open-editor` adds `open_editor`, which opens one of the app's editor windows
for you to finish by hand; it needs the Amberflip app.

## Skills

The [skills](skills) folder holds guides that teach an AI app which tool to use for which job.
They need Amberflip installed on the same computer.

| Skill | For |
|---|---|
| `amberflip` | Choosing the right tool, and what its errors mean |
| `amberflip-convert` | Converting between formats |
| `amberflip-compress` | Making files smaller, fitting a size limit |
| `amberflip-strip-metadata` | Removing location, camera and author data |
| `amberflip-pdf` | Merging, splitting, reordering and redacting PDFs |
| `amberflip-video` | Trimming, cropping, joining and redacting video |
| `amberflip-audio` | Trimming, loudness, bleeping, waveforms |
| `amberflip-image` | Cropping, adjusting, collages, annotations |
| `amberflip-recipes` | Jobs that take several tools |

The plugin installs them for Claude Code. Without the plugin, the Amberflip app installs them into
`~/.claude/skills/` and `~/.agents/skills/` when you press Connect.

## Command line

The same engine without a window, for scripts and servers:

```sh
npx amberflip-cli convert photo.heic --to jpg
npx amberflip-cli compress talk.mp4 --target 40% --height 720
npx amberflip-cli --help
```

Install it with `npm install -g amberflip-cli`. Results always get new names; inputs are never
changed.

## Trial and licence

- **Free trial: 3 days from first use**, on each computer, with every tool.
- After that, a **one-time licence key** is needed. One key works on **one computer** that you
  own or control; the app, the command line, MCP and the skills on that computer are all
  included. No subscription.
- The price depends on your country and is shown where you buy the key:
  <https://amberflip.com/buy>.
- Enter a key yourself, in the app or with `amberflip-cli activate KEY`. Never paste a key into a
  chat with an AI.
- When the trial is over, the tools stop with a message that says where to buy a key. Your files
  are not touched.
- A new key is confirmed once by the licence server (within 7 days of entering it); after that
  Amberflip works offline.

The full end-user agreement is at <https://amberflip.com/legal/>.

## Privacy

- Your files and their contents never leave your computer. Amberflip has no analytics, telemetry,
  advertising or crash reporting.
- The only network contact of `amberflip-cli` and the MCP server is the **licence check**: about
  every six hours when the computer is online, it sends an anonymous one-way hash of the
  computer's id and, after you enter a key, your licence key, so the free trial and keys cannot be
  abused. Without internet, Amberflip keeps working with its local checks.
- The MCP server listens on no port. It talks to your AI app over standard input and output.
- The AI app you connect sees the paths of your files and the paths of the results, as any tool
  call shows them. What your AI app sends to its own provider is governed by that app.

Full policy: <https://amberflip.com/legal/>.

## Issues and support

Questions, bugs and requests: [open an issue](https://github.com/ismoil77/amberflip-mcp/issues).
To contact the developer privately, write to <support@amberflip.com>. Please do not post licence
keys in issues.

## What is in this repository

| Path | What |
|---|---|
| `server.json` | Entry for the official MCP Registry (`com.amberflip/amberflip`) |
| `glama.json` | Ownership file for Glama |
| `.claude-plugin/`, `.mcp.json`, `skills/` | The Claude Code plugin and marketplace |
| `llms-install.md` | Install guide written for Cline |
| `AGENTS.md` | Short guide for coding agents |
| `CHANGELOG.md` | What changed in each version |

## Licence of this repository

The manifests, documentation and skills in this repository are under the [MIT License](LICENSE).
The Amberflip application and `amberflip-cli` program are proprietary and are not part of this
repository: see [NOTICE](NOTICE).
