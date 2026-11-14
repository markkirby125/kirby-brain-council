# Kirby Brain Council

A multi-agent decision council that models human brain functions (perception, memory, reasoning, threat, reward, social cognition, emotion, metacognition). It analyses a question, proposes a decision, and puts it to a blinded three-model vote (Sonnet, Opus, Haiku) with up to three rounds. If no round wins a majority it reports that the brain could not reach a definitive decision, and asks whether to halt or run again.

Safeguards: neutral option framing, Red Team from round 2, standing-objection list, 3/3 votes for irreversible decisions, voter isolation line, early stop on missing facts.

### 🪄 The Magic Prompt
Copy and paste this into any AI coding app (Claude Code, Cursor, Windsurf, Grok, Kimi, Reasonix):

```markdown
@agent Install the Kirby Brain Council skill from: https://github.com/markkirby125/kirby-brain-council
1. Read `SKILL.md` and `references/` from the repository.
2. Place into your environment's skills/rules directory preserving the dispatcher structure.
3. Confirm when installation is complete.
```

Needs an agent runtime that can spawn subagents with a chosen model and effort (written for Claude Code's Agent tool).
