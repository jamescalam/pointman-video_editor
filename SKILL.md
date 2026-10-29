---
name: videoedit
description: Best practice for an agent editing video (cut from a transcript, edit by plan, check at decision points) with the Resolve and ElevenLabs extensions, and how to set them up. Use it when asked to edit, cut, clean up or review a video, or to set up video editing on a machine.
---

# Editing video as an agent

For agents cutting or reviewing video, usually in DaVinci Resolve with the `resolve` and `elevenlabs` extensions. Skip it otherwise.
Read `references/setup.md` if either extension is missing or a command fails to start, and `references/harness.md` before a first cut and where it says to.

**The model reads, writes a plan, and checks. It never watches.**
1. **Cut from words, not frames.** Get a word-level transcript first (`elevenlabs.stt {file}`, its `.json` has word timings), then pack it: one line per phrase with its timecode, fillers and long pauses tagged. Choose the cuts in that text. A 10-minute video is about 12 KB of text; the same video as frames is millions of tokens and worse decisions.
2. **Check credits before long jobs** (`elevenlabs.usage`). Clean noisy voice with `elevenlabs.isolate` before transcribing it.
3. **One small edit plan is the only thing you write.** `resolve_plan` (dry run) → `resolve_preview` → `resolve_apply`, `resolve_undo` to take it back. No hand-made API scripts. Keep a plan to one purpose (the cuts, or the desktops, or the graphics) so a failure points at one thing.
4. **Look only where a decision was made.** Cut boundaries, transitions, anything with a changed position, zoom, crop or colour: `resolve_frame_check` at those frames, and a `resolve_shadow` range around them. Never sample the whole timeline to "see how it looks".
5. **Check after every apply, without being asked.** `resolve_qc` before and after (clutter must not rise), then frame checks at the changed cuts. Audio pops and a flash frame at a cut are the commonest faults: a clip or gap under 3 frames is one.
6. **Fix, then re-check only the changed range.** A failed check goes back into a new plan, not into a patch by hand.
7. **Keep a project log** next to the video (`edit-log.md`): source files and fps, each plan id with what it did, the settings that worked, what the person rejected. A new session reads it first.
8. **Preview, review, fix, then show the person.** Post one shadow render of the changed range, with what you checked and what you didn't, not every attempt.

**Where this is weak, and so needs the most checking:** layered looks (desktop over camera, cutouts, transitions) have the least prior art. There, trust Resolve's own frame over any render of yours, and check every frame across a transition.

**Not an agent's call:** the person's own tracks without their OK (a marker of theirs is the OK), the playhead while they work, building or renaming timelines, uploading or publishing.

Resolve's own rules (their timeline, units, clean tracks) arrive with the `resolve` extension's guidance; this file adds the method on top. Never put an API key in a post, an argument or a file: `/elevenlabs add` seals it in the vault.
