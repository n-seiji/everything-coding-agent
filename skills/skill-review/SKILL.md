---
name: skill-review
description: Use when writing a new skill or editing an existing one, and again before committing it.
---

# Skill review

A skill you just wrote is almost always too long and too prescriptive. Review it
against the principles below before you commit it.

Source: [Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra).
Read it if the summary here is not enough to settle a question.

## Principles

**The description is the routing signal.** Keep it to one line that names the
trigger — "when adding or changing a migration", not "for database work". A long
description gets truncated, which makes the model worse at picking the skill.

**Keep the root document small.** If a skill spans several workflows, make the
root a router and put the detail in separate files the model can open when it
needs them.

**Do not over-specify.** Guidance that would have helped an older model can now
get in the way. Exact flag lists, shell pipelines, and step-by-step recipes for
things the model can discover with `--help` belong in the model's hands, not the
skill. State the policy and the decision points; leave the mechanics out unless
getting them wrong is expensive.

**Drop the reflexes.** "Always read the docs before editing", "remember to run
the tests" — the model already does this, and the instruction just burns context.

**Grant permission instead of withholding it.** Where a workflow is safe, say it
is allowed rather than making the model ask.

**Say where "done" is.** "Implement, run, inspect, fix" beats "check after the
first implementation".

**Do not write for the most constrained model in the room.** Constraints that
protect a weaker model make a stronger one worse.

## Review pass

Read the skill back and cut:

- Any sentence the model would have done anyway
- Any command that exists only to show syntax
- Any example that repeats a rule already stated
- Any hedge or repetition of when to use the skill

Then check the description alone still tells a model whether to open the file.
