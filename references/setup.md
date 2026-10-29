# Setting up Resolve and ElevenLabs

Both are Pointman extensions. Installing is `pointman add <id>` on the machine, or Extensions in the app. Check with `pointman status` or the extension's health in the app.

## DaVinci Resolve (`resolve`)

```
pointman add resolve
```

Needs, on a Mac (Apple silicon or Intel):
- **DaVinci Resolve Studio** at `/Applications/DaVinci Resolve/DaVinci Resolve.app`, **open**. The free edition's scripting isn't enough.
- **Preferences › System › General › External scripting using › Local.**
- **ffmpeg and ffprobe** on the PATH (`brew install ffmpeg`): previews, shadow renders and QC use them.

The extension's commands only appear while Resolve is connected: a request made with Resolve closed waits until it opens.

Use (agents get these as tools `resolve_*`, anyone with `/resolve <verb>`):
| For | Command |
|---|---|
| Render a preview of a timeline, or a still of the playhead | `resolve.render`, `resolve.still` |
| The timeline as data (tracks, clips, markers), read only | `resolve.snapshot` |
| Edits as plans: dry run, shadow preview, apply, undo | `resolve.plan`, `resolve.preview`, `resolve.apply`, `resolve.undo` |
| Any range drawn from the source files, without touching Resolve | `resolve.shadow` |
| QC: black or flash frames, frozen screen recordings, clutter per track, loudness | `resolve.qc` |
| Resolve's own frame against the shadow render | `resolve.frame_check` |
| Pixels per Crop/Pan/Tilt unit at this timeline's size | `resolve.calibrate` |

Markers that call an agent: write `@pm`, `@pointman` or `@<agent>` at the start of a marker's name or note. Routes (project to thread and agent) are in `~/.pointman-resolve/routes.toml`. Notes on a preview come back as markers: purple open, green resolved.

Settings (`pointman config set node.extensions.resolve.<key> <value>`): `preview_height` (480, 720, 1080), `auto_render_idle_minutes` (0 is off), `watch`, `marker_calls`, `markers`.
Your project is left as it was: a render restores its settings, page and timeline.

## ElevenLabs (`elevenlabs`)

```
pointman add elevenlabs
```
Then, once, in the app: `/elevenlabs add` in any thread. Paste an API key from elevenlabs.io (Developers › API Keys) into the form, which seals it in the vault. Never paste a key into a post, an argument or a file; the extension never runs `elevenlabs auth`.

The CLI is the machine's own `elevenlabs` (`brew install elevenlabs/tap/elevenlabs`); with none, the extension installs ElevenLabs' release itself and checks its sha256.

| For | Command |
|---|---|
| Clean up a voice track | `elevenlabs.isolate {file}` |
| A transcript with word timings (`.txt`, `.json`) | `elevenlabs.stt {file, model?, language?}` |
| Word timings for a script you have | `elevenlabs.align {file, text}` |
| Speech from text | `elevenlabs.tts {text, voice, model?}` |
| Voices, and credits left | `elevenlabs.voices {search?}`, `elevenlabs.usage` |
| Any other CLI command | `elevenlabs.run {args, dry_run, confirm?, file?}`: a dry run unless `dry_run: false`; deleting or cloning also needs `confirm: true`, so ask the person |

`file` is a board file (`pointman://asset/<id>`) or a path in the extension's folders. Long files are fine: commands run in the background and can be cancelled. Credits first: run `elevenlabs.usage` before many minutes of isolating or transcribing.

## Check it works

1. `resolve.snapshot` returns the open timeline.
2. `elevenlabs.usage` returns the plan and credits.
3. `elevenlabs.stt` on a short clip returns a `.json` with word timings.
