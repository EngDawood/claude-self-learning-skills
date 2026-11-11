---
name: session-to-skill
description: >-
  Reviews the current conversation and turns what was learned into a reusable skill, or into
  an update of a skill the user already has. Use whenever the user says "save this as a skill",
  "make a skill from this chat", "turn this session into a skill", "what did we learn?",
  "remember how we did this for next time", or asks to fix a skill that just gave a wrong or
  outdated result. Also offer it, in one line, at the end of a task that took real trial and
  error, several user corrections, or a multi-step workflow the user is likely to repeat.
  Applies to any kind of work: debugging, writing, research, document or data pipelines.
metadata:
  version: "0.1.0"
---

# Session to Skill

Review this conversation, decide what in it is worth keeping, and hand the result to
`skill-creator` to write and package. This skill is the judgment layer only. It does not
replace `skill-creator`, so do not restate its authoring or packaging rules here.

The reason this skill exists: a hard-won session ends and the knowledge is gone. Next time
the user pays for the same trial and error again. A skill is the only thing on claude.ai that
carries a procedure into future chats, so the job is to capture the few things that would
have saved time, and nothing else.

## When to speak up unasked

There are no hooks on claude.ai, so nothing forces a review. If a task just finished and it
involved real trial and error, repeated corrections from the user, or a multi-step workflow
they will likely run again, offer once in a single line, for example: "This took a few tries.
Want me to save the working procedure as a skill?" Then drop it. Never create or change a
skill without a yes, and never interrupt unfinished work to offer.

## Step 1: Find candidates

Read back through the whole conversation, not just the last few turns. Look for:

- **Corrections**: places the user said "no, not like that" or redid your output. What they
  corrected is exactly what a future chat will get wrong again.
- **Dead ends and what finally worked**: the working path, plus the failed attempts worth
  warning about.
- **Repeatable workflows**: a sequence of steps, tools, or formats that produced a result the
  user accepted, and that they would plausibly ask for again.
- **Non-obvious facts**: tool quirks, misleading errors and their real cause, required
  settings, the user's house style or format rules.

Keep a candidate only if it passes all four:

1. **Reusable**: it will come up again, not just in this one task.
2. **Non-obvious**: a fresh Claude would not do it right unaided. If you would have got it
   right first time without the session, there is nothing to save.
3. **Specific**: you can state when it applies and exactly what to do.
4. **Verified**: it worked in this session and the user accepted the result. Do not save
   something that only sounds right.

Most sessions yield zero or one candidate. Saying "nothing here is worth a skill" is a good
outcome, because every extra skill competes for attention with the useful ones.

## Step 2: Choose the right form

A skill is not always the answer. Pick the lightest form that works:

| What was learned | Best form |
|---|---|
| A procedure, workflow, or format with several steps | Skill |
| A single standing preference ("no em dashes", "be brief") | A line in the user's preferences or project instructions. Tell them the exact line to add |
| A fact about the user or their project | Memory or project knowledge, not a skill |
| Something tied to this one task only | Nothing |

## Step 3: Update or create

Check the skills the user already has before proposing a new one. Their names and
descriptions are in your available skills list. If an existing skill looks related and its
files are readable, read its `SKILL.md` before deciding.

| Found | Action |
|---|---|
| Nothing related | Create a new skill |
| An existing skill covers this and was wrong, outdated, or missed a step | Update that skill |
| An existing skill should have triggered in this chat but did not | Update its description with the phrasing the user actually used |
| Same area, clearly different task | Create a new skill and mention the related one in it |

Prefer updating. Two skills that overlap make both trigger less reliably. Skills that are
built in or come from a plugin are not the user's to edit: for those, create a small
companion skill or suggest a preference line instead.

## Step 4: Confirm with the user

Before writing anything, show a short list and wait for their choice:

```
Found 2 things worth keeping:
1. NEW skill `<name>`: <one line on what it does and when it would trigger>
2. UPDATE `<existing-skill>`: <one line on what changes and why>
Skipped: <one line on what you considered and dropped, and why>
Which should I build?
```

Ask about anything the session left unclear, such as whether a correction was a one-off or a
standing rule. A wrong guess here gets baked into every future chat.

## Step 5: Hand off to skill-creator

For each approved item, use the `skill-creator` skill to write and package it. Give it a
brief built from this conversation so it does not have to re-interview the user:

- **Goal**: what the skill lets Claude do.
- **Triggers**: the real phrases the user used in this chat, plus obvious variants.
- **Procedure**: the steps that worked, in order, with the reason for each non-obvious one.
- **Pitfalls**: what failed or was corrected, written as "do X because Y", not as a story
  of this session.
- **Output format**: an example taken from the output the user accepted.
- **For updates**: the existing skill's name (keep it unchanged) and the specific edits.

Generalize as you write the brief. Replace this session's file names, topics, and data with
placeholders, unless the skill is meant to be about that specific project. Leave out secrets,
tokens, private links, and personal details about third parties.

If `skill-creator` is not available, write the `SKILL.md` yourself (frontmatter with `name`
and `description`, then instructions kept under about 200 lines) and give the user the file.

## Step 6: Report

End with one or two lines: what was created or updated, how to install it (the saved file
has a Save skill button, or it can be uploaded in the skills settings), and a suggestion to
try it in a fresh chat with a prompt like the one that started this session. A fresh chat is
the real test, because this one already knows the answer.
