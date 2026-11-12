# claude-self-learning-skills

A Claude plugin that lets Claude learn from its own sessions.

## Skill: session-to-skill

Reviews the current chat, collects the lessons worth keeping, decides whether to update an
existing skill or create a new one, then hands off to `skill-creator` to write and package
it. When the `writing-for-agents` skill is installed, the draft is tightened with it first.

It needs no hooks or local files, so it works on claude.ai as well as Claude Code.

### Use

Say any of these in a chat:

- "save this as a skill"
- "what did we learn?"
- "this skill gave me a wrong result, fix it"

Claude may also offer it once, in one line, after a task that took real trial and error. It
writes only what you approve.

### Self-learning footer

Every skill that `session-to-skill` creates or updates ends with a short "After each run"
section. After a task, that skill checks for a real lesson and proposes up to 3 edits to
itself, applied only after you approve. To add it to a skill you already have, paste
[`skills/session-to-skill/self-learning-footer.md`](skills/session-to-skill/self-learning-footer.md)
at the end of its `SKILL.md`.

It runs when the skill's task ends, not when the chat closes. On claude.ai, installed skills
are read-only, so approved edits come back as an updated skill to re-upload.

## Install

Claude Code:

```bash
/plugin install self-learning-skills --marketplace EngDawood/claude-self-learning-skills
```

claude.ai: add this repository as a plugin marketplace in the plugin settings, or upload a
zip of the repository as a plugin.

## Layout

```
.claude-plugin/plugin.json        plugin manifest
.claude-plugin/marketplace.json   lets the repo be installed as a marketplace
skills/session-to-skill/SKILL.md                 the skill
skills/session-to-skill/self-learning-footer.md  footer added to each skill
```

## Credits

Inspired by [Claudeception](https://github.com/blader/Claudeception) and
[claude-code-continuous-learning-skill](https://github.com/AlexMikhalev/claude-code-continuous-learning-skill)
(both MIT). The update-vs-create step comes from Claudeception, and the "skill vs something
lighter" and fix-on-use ideas from the
[ziv-daniel fork](https://github.com/ziv-daniel/Claudeception). The text here is written
from scratch.
