# Why the harness looks like this

What the better open agent-editing harnesses have in common, from their READMEs and write-ups (30 Oct 2026). None report benchmarks or failure rates, and the repos weren't read, so treat the numbers as the authors' own.

| Idea | Seen in | In our setup |
|---|---|---|
| The model reads the video as a word-level transcript (about 12 KB per 10 min), fillers tagged; it never watches frames | [Video Use](https://www.solosoft.dev/post/video-use-ai-editing-2026/) | `elevenlabs.stt`, then pack with timecodes |
| Pictures only at decision points: filmstrips at cut boundaries and transitions (20 to 50 per 10 min) | Video Use | `resolve_frame_check`, `resolve_shadow` on a range |
| One small reviewable edit file, rendered by deterministic tools (EDL/JSON) | Video Use, [Agent Video Studio](https://github.com/mikolajmikolajczak0108/agent-video-studio) | `resolve_plan` / `preview` / `apply` / `undo` |
| An automatic check after every cut (audio continuity, flash or dropped frames, caption sync); a failure loops back | Video Use | `resolve_qc` plus frame checks after each apply |
| The agent writes code or a plan, few composable tools, current docs looked up on demand (llms.txt) | [Diffusion Studio](https://re-skill.io/blog/ai-video-editing-agent-re-skill-diffusion-studio) | `skills/resolve-rules.md` from the resolve extension |
| Frames sampled (1 fps) after each step; an evaluator decides render or refine | Diffusion Studio | shadow render of the range, review, fix |
| State kept in a project file so a fresh agent resumes | Video Use (`project.md`) | `edit-log.md` |
| No GUI automation; re-render only the changed stretch | [OpenEdit](https://github.com/veedstudio/open-edit) | check only the changed range |
| Per-word timings required: captions drift without them | OpenEdit | `elevenlabs.stt` json, `elevenlabs.align` for a script you have |

Most of this is for talking-head cutting. If a cut is wrong in a way this doesn't explain, record it in `edit-log.md` and tell the person what you changed in the method.
