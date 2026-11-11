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
skills/session-to-skill/SKILL.md  the skill
```

## Credits

Inspired by [Claudeception](https://github.com/blader/Claudeception) and
[claude-code-continuous-learning-skill](https://github.com/AlexMikhalev/claude-code-continuous-learning-skill)
(both MIT). The update-vs-create step comes from Claudeception, and the "skill vs something
lighter" and fix-on-use ideas from the
[ziv-daniel fork](https://github.com/ziv-daniel/Claudeception). The text here is written
from scratch.
