# Agent Instructions — Discover (Unreal)

Unreal Engine Mixed Reality showcase that demonstrates the Meta Quest MR APIs (Passthrough, Scene API, Spatial Anchors, Shared Spatial Anchors, Colocation, Interaction SDK) wired into a single project, with multiplayer powered by Epic Online Services.

## Source-of-truth files (read these first, do not duplicate their contents in this file)

For setup, build steps, SDK versions, and project layout, read:

- `README.md` — official requirements, engine-fork instructions, and run flow
- `UnrealDiscover.uproject` — Unreal engine association and enabled/disabled plugins
- `Config/` — `DefaultEngine.ini`, `DefaultGame.ini` (Online Subsystem EOS keys live here)
- `Source/UnrealDiscover/` — gameplay C++ module sources and `.Build.cs`
- `Documentation/Configuration.md` — Meta Quest configuration steps
- `Documentation/EOSConfiguration.md` — Epic Online Services configuration
- `Documentation/DiscoverOverview.md` and `Documentation/ProjectStructure.md` — design and layout notes
- `LICENSE` — license terms (Meta SDK License for SDK material; EOS components governed by Epic's EOS license)

## Quest / Horizon-specific notes

- **Hard dependency on the Oculus / Meta UE fork** (`Oculus-VR/UnrealEngine`, the `oculus-5.x` branch matching `EngineAssociation` in `UnrealDiscover.uproject`). This project will **not** build on stock Epic UE — do not propose a build path that skips the fork.
- Git LFS is required; run `git lfs install` before cloning.
- `ResonanceAudio` and `Metasound` are intentionally disabled in `UnrealDiscover.uproject`; re-enabling them risks audio conflicts with the Meta XR Audio plugin.
- EOS configuration touches `DefaultEngine.ini` (Online Subsystem EOS keys) — keep Client ID / Client Secret out of commits.
- Editor testing is via **Quest Link** / **Air Link** with `MainMenuMap` loaded; for shader iteration, use the helpers in `ShaderCacheScripts/` rather than wiping the DDC by hand.

## Meta Quest tooling

This repository is part of the Meta Quest / Horizon OS ecosystem (a sample, library, template, or related project — the bespoke intro above describes which). Use that intro and the source-of-truth files it references for project-specific decisions; don't restate or invent facts from memory.

When the user asks anything about Quest device behavior, build / deploy / debug / capture flows, on-device performance, or Horizon OS APIs, reach for these tools instead of generic Unreal answers:

- **`hzdb`** — Quest-aware ADB wrapper (device list, install / launch / stop, logs, screenshots, Perfetto traces, on-device docs search). Already wired up as an MCP server via `.mcp.json`, `.vscode/mcp.json`, and `.cursor/mcp.json`. Also runnable directly: `npx -y @meta-quest/hzdb <subcommand>`.
- **Meta Quest Agentic Tools** — the full skill set, including Unreal-specific skills: [github.com/meta-quest/agentic-tools](https://github.com/meta-quest/agentic-tools). Install per your client (Claude Code: `/plugin install meta-vr@meta-quest`; Gemini CLI: `gemini extensions install https://github.com/meta-quest/agentic-tools`; Cursor / VS Code: install the **Meta Horizon** extension from the Marketplace).

A few behavior expectations:

- **Read this repo's files first.** Before answering anything project-specific, read `README.md` and whichever source-of-truth files the intro above points at. Don't restate their contents in chat — quote or link instead.
- **Use `hzdb` for device-side work.** Anything that touches an attached Quest (install, launch, logs, screenshot, capture, manifest inspection) goes through `hzdb`, not raw `adb`.
- **Check live Horizon OS docs before answering API questions.** `hzdb docs search "..."` queries the live docs; training data on Horizon OS APIs goes stale fast.
- **Don't fabricate SDK / engine versions.** If a version isn't visible in this repo's files, say so rather than guessing.
