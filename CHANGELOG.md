# Changelog

What changes for people who connect an AI app to Amberflip. The version is the one of the Amberflip
app, `amberflip-cli` and the MCP server, which always ship together. Dates are year-month-day.

## Unreleased

- First public version of this repository: install guide, MCP Registry entry, Claude Code plugin
  and marketplace, and nine skills. No change to the program.

## 1.0.1 (2026-11-16)

- Windows: an automatic update of Amberflip no longer cuts off an AI app's waiting MCP server. The
  installer leaves a running server alone and moves `amberflip-cli.exe` aside, so the AI app keeps
  Amberflip's tools after the update.
- macOS gets the same build as Windows and Linux.

## 1.0.0 (2026-11-16)

- The first numbered release; every system ships the same number (the Microsoft Store refuses a
  first number of 0).
- The MCP server in `amberflip-cli mcp` with 38 tools: conversion, compression, PDF, video, audio,
  pictures, metadata and archives.
- The app no longer shows a price: it depends on the country and is shown where the key is bought.
- A licence key is confirmed once by the licence server; after that Amberflip works offline.
- Word, Excel and PowerPoint files convert to PDF without LibreOffice.
- Windows: the installer puts its folder on the user's `Path`, so `amberflip-cli` is found in a
  terminal. macOS: Settings, AI apps can put `amberflip-cli` on the `PATH`.
