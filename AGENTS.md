# Amberflip: notes for coding agents

Amberflip is an offline file converter and editor (pictures, video, sound, PDFs, documents) for
Windows, Linux and macOS. This repository is only its public documentation, MCP manifests and
skills. **It has no source code to build or test**: do not look for a build system here.

## Using Amberflip from an agent

- The MCP server is started by `npx -y amberflip-cli mcp` (stdio, no network listener). Install it
  by the steps in [README.md](README.md); in Claude Code:
  `claude mcp add --transport stdio --scope user amberflip -- npx -y amberflip-cli mcp`.
- Without MCP, run the same engine in a shell: `amberflip-cli --help`, then
  `amberflip-cli help COMMAND`. Results go to standard output as paths (or JSON with `--json`).
- Pass **absolute paths**. Call `inspect` before an edit that needs numbers (pixels, seconds, page
  counts) and `list_operations` to see what applies to a file.
- Results are **new files** next to the originals (or in `output_dir`). Originals are never changed:
  delete only what the user asks you to delete. In a chain of steps, give each step the exact path
  the previous one returned.
- The skills in [skills/](skills) say which tool fits which job; start with `skills/amberflip`.

## Licence and trial

The free trial lasts 3 days from first use on a computer; then a one-time licence key is needed
(`amberflip-cli activate KEY`). If a tool or command reports that the trial is over (exit code 10),
stop and give the user <https://amberflip.com/buy>. Never ask for, store or repeat a licence key in
a conversation: the user enters it in the app or with `amberflip-cli activate -`.

## Editing this repository

It holds Markdown and JSON only. Keep every JSON file valid, keep tool names exactly as the server
lists them, and do not add source code, binaries or licence-signing material.
