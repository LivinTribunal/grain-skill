# grain

A Claude Code skill that stops AI coding agents bloating your repo.

Agents are good at adding code and bad at noticing that the code already exists.
Left alone they give you a second helper next to the first, a route that skips
the layer its siblings use, a module of loose functions where the codebase uses
injected classes. Each one passes its tests. Together they mean the repo stops
having a shape, and every reader has to check both copies to trust either.

Two habits, written down so an agent will follow them:

- **Grain.** Find the nearest existing example of the same kind of thing and
  mirror its shape, place and names. If a reviewer can tell which lines the agent
  wrote purely from their shape, it fought the grain.
- **Ponytail.** The laziest solution that actually works. Climb a ladder from
  "does this need to exist at all" to "the minimum code that works" and stop at
  the first rung that holds. Lazy about the diff, never about the reading.

The story, with numbers, is in [the blog post](https://www.flowhunt.io/blog/code-with-the-grain-ai-coding-agent-skill/).

## Install

```bash
git clone https://github.com/LivinTribunal/grain-skill.git
cd grain-skill
mkdir -p ~/.claude/skills/grain
cp -r SKILL.md docs ~/.claude/skills/grain/
```

For one project instead of your whole account, use `.claude/skills/grain/` inside
the repo.

## Then put the rule where the agent already reads

This is the part that decides whether any of it works. Over five weeks of
transcripts on the repo this came from, the agent invoked the skill by name
**zero times**. The rules landed anyway, because the same text was also in three
places the agent reads without choosing to.

**1. One paragraph in your `CLAUDE.md`**, which is in context from the first
turn:

```markdown
Before writing or changing code, find the nearest existing example of the same
kind of thing and mirror its shape, location and naming. Reach for the laziest
solution that actually works: reuse an existing helper, extend a sibling, stdlib
or native before a new dependency. Keep the diff minimal, no drive-by refactors.
Close with one line naming the exemplar you mirrored. Full rules in the `grain`
skill.
```

**2. Two imperative lines in your implementer agent's definition**, as rules
rather than as a skill to consider:

```markdown
- Mirror the nearest existing example's shape, location and naming.
- Reach for the laziest solution that actually works: reuse an existing helper,
  extend a sibling, stdlib or native before a new dependency. A new function,
  component or file is the last resort.
```

**3. The review step**, using the tags in [`docs/review-tags.md`](docs/review-tags.md),
so a change that ignores the rules fails rather than merges.

A skill is a good place for the long version of a rule. It is a bad place for the
rule itself, because the agent has to decide to load it and it mostly will not.

## What is in here

| File | What it is |
|---|---|
| `SKILL.md` | The reflex: seven steps, the closing line, the boundaries |
| `docs/grain-and-ponytail.md` | The law: picking an exemplar, the call chain, the three anti-patterns, the laziness ladder, when not to be lazy |
| `docs/review-tags.md` | The two review passes, thirteen tags, and why structure runs before cuts |

## Make it yours

The doc ships with a worked example from a Python backend with a vertical-slice
layout: a table of artifact kinds, where each lives and what to mirror, plus the
canonical call chain. Replace that table with yours and name your own canonical
exemplar, a slice that is small, current and spans the layers. That one edit is
most of the value, because "mirror the nearest example" only works if the agent
knows which example is the right one.

## The closing line

Every implementer's report ends with one line:

```
mirrored: <exemplar path>. <what you added or extended>.
```

or, when nothing fits:

```
new pattern: <what>. No existing <kind> because <reason>. Closest sibling: <path>.
```

This is cheap and it is the single best review aid here. A `mirrored:` line
pointing at the wrong kind of file tells you the diff is wrong before you open
it. A missing one tells you the agent did not look.

## Companion skill

[architect-skill](https://github.com/LivinTribunal/architect-skill) is the other
half: the expensive model reads, decides and writes a brief; a cheaper worker
writes the code. The brief names the exemplar, and grain is how the worker uses
it.

## Licence

MIT.
