# claude-self-learning-skills

Skills that let Claude on claude.ai learn from its own sessions.

## session-to-skill

Reviews the current chat, picks what is worth keeping, decides whether to update an existing
skill or create a new one, then hands off to the built-in `skill-creator` to write and
package it.

It is a thin judgment layer (about 130 lines) and needs no hooks or local files, so it works
on claude.ai, where the Claude Code approach of a per-prompt hook is not available.

### Use

Say any of these in a chat:

- "save this as a skill"
- "what did we learn?"
- "turn this session into a skill"

Claude may also offer it once, in one line, after a task that took real trial and error. It
never creates or changes a skill without your yes.

### Install

Build the package and upload it in the claude.ai skills settings:

```bash
zip -r session-to-skill.skill session-to-skill
```

### Credits

Inspired by [Claudeception](https://github.com/blader/Claudeception) and
[claude-code-continuous-learning-skill](https://github.com/AlexMikhalev/claude-code-continuous-learning-skill)
(both MIT). The update-vs-create step comes from Claudeception, and the "skill vs something
lighter" and fix-on-use ideas from the
[ziv-daniel fork](https://github.com/ziv-daniel/Claudeception). The text here is written
from scratch for claude.ai.
