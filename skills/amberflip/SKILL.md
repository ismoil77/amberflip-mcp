---
name: amberflip
description: Choose and run the right Amberflip tool for a file job on this computer, offline. Use when the user mentions Amberflip or asks what it can do or how to use it, when a job spans several kinds of file, when the amberflip tools are missing from this session, or when Amberflip reports a licence, missing converter or path error.
compatibility: Requires the Amberflip app or amberflip-cli on this computer. Works offline.
---

# Using Amberflip

> **Needs Amberflip installed** on this computer: the app or `amberflip-cli`, from https://amberflip.com (free 3-day trial, then a licence key). If Amberflip is missing, tell the user to install it; these steps do nothing without it.

Amberflip converts and edits files on this computer; nothing is uploaded.

## Reach it

Use the tools of the `amberflip` MCP server when this session has them. Otherwise run
`amberflip-cli` in a shell: same engine, same results, and `amberflip-cli help COMMAND` lists each
command's flags. With neither, tell the user to open Amberflip, then Settings, AI apps, and press
Connect, or to install it from https://amberflip.com.

A tool's name splits into the command, its parameters become flags: `video_trim` with `from: 1` is
`amberflip-cli video trim FILE --from 1`. `--params FILE.json` takes the same JSON a tool takes.
These differ:

| Tool or parameter | `amberflip-cli` |
| --- | --- |
| `list_operations`, `run_operation`, `license_status` | `ops`, `run`, `license` |
| `strip_metadata`, `pdf_merge` | `strip`, `merge` |
| `output_dir` | `-o DIR` |
| `compress` `target_mb: 2`, `target_percent: 50` | `--target 2MB`, `--target 50%` |
| `compress` `video_height: 720` | `--height 720` |
| `areas` | `--area X,Y,W,H` for each area (`help video`, `help image`, `help pdf`) |
| `video_trim` `exact: false` | `--fast` |
| Answer `outputs` | With `--json`, `results[].output` |
| Answer `over_target` | With `--json`, `results[].over_target`, and a warning on standard error |

## Choose

| The job | Use |
| --- | --- |
| Another format | `amberflip-convert` skill |
| Smaller file, size limit | `amberflip-compress` skill |
| Location, camera, author, tags | `amberflip-strip-metadata` skill |
| Pages, parts, removing text from a PDF | `amberflip-pdf` skill |
| Cutting or changing a video | `amberflip-video` skill |
| Cutting or changing sound | `amberflip-audio` skill |
| Cropping or changing a picture | `amberflip-image` skill |
| Several steps towards one goal | `amberflip-recipes` skill |
| Packing or unpacking | `zip`, `extract` |
| The user wants to finish by hand | `open_editor`, which opens the app's window |
| Anything else | `list_operations` with the file: what it can do, and the tool for each |

## Every job

1. Pass absolute paths.
2. Call `inspect` before choosing any number: pages, picture size, length, and metadata names.
3. In a chain, give each step the exact path the last step returned, because a name already taken
   gets a number (`photo (compressed) 2.jpg`). The originals stay as they are, so delete only what
   the user asks you to delete.
4. Check the result with `inspect` before reporting success: size against a limit, length after a
   cut, page count after a PDF edit.
5. Report every path made and every file under `failed`, with its reason.

Video work can take minutes. Wait for the answer; a second call starts the job again.

## When it fails

The exit codes are `amberflip-cli`'s; a tool's error says the same in words.

| Says | Means | Do |
| --- | --- | --- |
| Exit 10, trial ended | A licence is needed | Stop and give the user https://amberflip.com/buy |
| Exit 10, "Connect once to confirm the key" | The key is valid; the licence server has not confirmed it on this computer within 7 days | Ask the user to connect the computer to the internet, then run the command again |
| Exit 11, key refused | Refunded, or on too many computers | Stop and tell the user |
| Exit 12, not a valid key | The key is mistyped | Ask the user to enter it again |
| Exit 4, names FFmpeg, Ghostscript or LibreOffice | That converter is missing | `amberflip-cli tools` shows which; the app's Settings, Converters page adds it |
| Exit 3, no such file | Wrong path | Find the file, then repeat |
| Exit 2, wrong option | Wrong flag | Read `amberflip-cli help COMMAND`, then repeat |
| Exit 1, or entries under `failed` | Some files failed, the rest are done | Repeat only the failed ones, after fixing the reason |
| A format it cannot go to | Not supported from this kind of file | `list_operations` with the file; offer what it lists |

The user enters a licence key themselves, in the app or with `amberflip-cli activate -`, so the key
stays out of this conversation.
