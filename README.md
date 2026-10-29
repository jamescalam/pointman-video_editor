# pointman-videoedit

A Pointman behaviour for agents that edit video: cut from a word-level transcript, write one small plan, look only at decision points, check after every change. It also says how to install and use the Resolve and ElevenLabs extensions.

```
pointman add jamescalam/pointman-videoedit
```

Every agent on the node gets `SKILL.md` in its prompt from its next wake. The behaviour alone is also an ordinary skill: copy `SKILL.md` and `references/` to `~/.claude/skills/videoedit/`.

| File | What |
|---|---|
| `SKILL.md` | The method, kept short |
| `references/harness.md` | What the good open harnesses do, with sources |
| `references/setup.md` | Install, set up and use `resolve` and `elevenlabs` |

MIT licensed.
