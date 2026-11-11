---
name: session-to-skill
description: >-
  Extracts lessons from the current conversation into a new skill or an update to one the
  user already has. Use when the user asks to save this chat as a skill ("save this as a
  skill", "remember how we did this"), asks "what did we learn?", or reports that a skill
  they own just gave a wrong or outdated result. Offer it in one line when a task ends after
  real trial and error, repeated user corrections, or a workflow the user will repeat.
metadata:
  version: "0.2.0"
---

# Session to Skill

A **lesson** is something this conversation taught that a fresh chat would get wrong again.
Find the lessons, pick the lightest form that preserves each, and hand the ones that need a
skill to `skill-creator`. Most sessions hold zero or one lesson, and "nothing here is worth a
skill" is a good result, because every extra skill competes for attention with the useful
ones.

## Offering unasked

When you were not asked, offer once, in one line, after the task is finished: "This took a
few tries. Want me to save the working procedure as a skill?" Proceed on a yes. On anything
else, carry on with the conversation.

## 1. Collect lessons

Read the whole conversation from the first message. Sources of lessons:

- **Corrections**: each place the user rejected or redid your output.
- **Dead ends**: each approach that failed, and the one that finally worked.
- **Workflows**: a sequence of steps, tools, or formats whose result the user accepted.
- **Hidden facts**: tool quirks, misleading errors and their real cause, required settings,
  house style.

A lesson must pass all four tests:

1. **Reusable**: it will come up in other tasks.
2. **Non-obvious**: you would have got it wrong at the start of this session.
3. **Specific**: you can state when it applies and exactly what to do.
4. **Verified**: it worked here and the user accepted the result.

Done when every correction and every dead end in the conversation has been tested and either
kept or dropped with a reason.

## 2. Pick the lightest form

| Lesson | Form |
|---|---|
| A procedure, workflow, or format with several steps | Skill |
| One standing preference ("be brief") | A line for the user's preferences or project instructions. Give the exact line |
| A fact about the user or their project | Memory or project knowledge |

## 3. Update or create

Compare each skill-bound lesson against the user's existing skills, listed by name and
description in your available skills. Read the `SKILL.md` of any related one if its files are
readable.

| Found | Verdict |
|---|---|
| Nothing related | Create |
| A skill of the user's covers this and was wrong, outdated, or missed a step | Update it |
| A skill of the user's should have triggered in this chat and stayed silent | Update its description with the phrasing the user used |
| Same area, different task | Create, and name the related skill inside it |
| The related skill is built in or from a plugin | Create a small companion skill, or fall back to a preference line |

Prefer updating: two overlapping skills both trigger less reliably.

Done when every skill-bound lesson has one verdict.

## 4. Confirm

Show this and wait for the user's choice. Write only what they approve.

```
Found 2 lessons worth keeping:
1. NEW skill `<name>`: <what it does and when it triggers>
2. UPDATE `<existing-skill>`: <what changes and why>
Dropped: <what you considered and why it failed a test>
Which should I build?
```

Ask here about anything the session left open, such as whether a correction was a one-off or
a standing rule. A wrong guess gets baked into every future chat.

## 5. Hand off to skill-creator

For each approved item, invoke `skill-creator` with a brief drawn from this conversation, so
it can skip its interview:

- **Goal**: what the skill lets Claude do.
- **Triggers**: the phrases the user actually used here, plus close variants.
- **Procedure**: the steps that worked, in order, with the reason for each non-obvious one.
- **Pitfalls**: each dead end and correction as "do X because Y".
- **Output format**: an example from the output the user accepted.
- **For updates**: the existing skill's name, kept unchanged, and the specific edits.

Generalize the brief: replace this session's file names, topics, and data with placeholders,
unless the skill is about this specific project. Replace secrets, private links, and details
about third parties with `<PLACEHOLDER>`.

Without `skill-creator`, write the `SKILL.md` yourself: frontmatter with `name` and
`description`, then instructions under 200 lines.

Done when each approved item exists as a file the user can save.

## 6. Report

Two lines: what was created or updated and how to install it (the Save skill button on the
file, or upload in the skills settings), then a prompt to try in a fresh chat. A fresh chat
is the real test, because this one already knows the answer.
