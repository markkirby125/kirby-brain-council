# Brain Council dispatcher

Policy comes from the user; this file is the how. Results are advice until the user approves them. Never take an irreversible, paid or outward-facing action on the council's say-so.

## Models and effort
| Stage | Agent `model` | `effort` |
|---|---|---|
| Perception gate, functions 1-6, Metacognition, Red Team | opus | high |
| Decision | opus | xhigh |
| Voter A | sonnet | xhigh |
| Voter B | opus | xhigh |
| Voter C | haiku | xhigh |

Haiku fills the Voter C slot by design: a cheap, fast third family of judgement on the same brief.

All stages are Agent-tool subagents with `subagent_type: general-purpose`. The "no tools" rule is enforced only by the brief. Launch every parallel group in ONE message. Agent calls may return asynchronously: wait until EVERY call in a group has reported (hand-back or task notification) before starting the next stage.

## Intake
1. Redact financials, credentials, client data and personal identifiers from the question. Briefs carry only the redacted text.
2. Create `<scratchpad>/brain-council/<run-id>/` (run-id: a short slug of the question plus the round-1 start). Save every brief and raw output VERBATIM (never condensed; the raw output is the full hand-back message) under `round-N/`.
3. Tell the user, in one line: about 12 calls in round 1 and about 8-10 in each later round, up to about 30 over 3 rounds, several minutes per round. Then run.

## State machine
```
R=1
GATE   run Perception (P) alone.
       DECIDABLE=YES -> FANOUT
       NO / NEEDS_INFO -> STOP_ASK (below)
FANOUT run functions 1-6 in parallel, all reading P's case file.
       R>=2: also run Red Team; re-run only implicated functions + Threat (see Retry); reuse cached outputs for the rest.
META   run Metacognition (M) on the function outputs.
DECIDE run Decision (D).
       R>=2 pre-vote check, run by the main agent with no model call: if D re-proposes the rejected option without answering every latest-round objection and every STANDING objection in OBJECTIONS_ADDRESSED, send it back to D once with its own decision and the list of unanswered objections; no voter calls.
VOTE   run the three voters in parallel, blinded (02_voters.md).
TALLY  per 02_voters.md.
       GOOD -> OUTPUT_SUCCESS
       BAD  -> if R==3 or EARLY_STOP -> OUTPUT_FAIL, else R++ and go to FANOUT
       BOARD_SHORT -> OUTPUT_BOARD_UNAVAILABLE (not a rejection)
```

## STOP_ASK (undecidable, and every failure output)
Never decide for the user. Print the restated question and at most 4 missing items (gate) or the failure report, then ask exactly: halt the skill, or perform another run? For a gate stop, "another run" means the user supplies the missing items and the skill restarts at round 1 with the cache. For a 3-rejection failure, offer a third choice: accept the best attempt (the user's call, never automatic).

## Retry (round R -> R+1)
Keep a running STANDING list: every voter objection from every round, deduplicated by meaning, with its `[F#]`/`[P]`/`[D]` tag and the rounds that raised it.
- Each re-run function brief gets: its own previous output (labelled PREVIOUS, so it revises instead of restarting); only the latest-round objections tagged to that function, deduplicated, each with the voters' FIX lines; and the rejected decision with its citation tags removed (so no citation points at material the function cannot see), labelled "REJECTED: do not re-propose unless you answer each objection explicitly."
- Red Team gets: P's case file, the rejected decision with its citation tags removed, and the full STANDING list. It does not get F outputs. It proposes nothing, so its brief carries no "do not re-propose" label.
- M gets: P and the current F outputs (cached and fresh). It does not get objections, REJECTED or Red Team.
- D gets: P, all F outputs, M, Red Team, the rejected decision, the latest-round objections, the voters' CAVEATS, and the full STANDING list so earlier fixes are not undone. D must not reverse a point it changed in response to an objection unless it says why in OBJECTIONS_ADDRESSED.

Re-run set: functions tagged by voters, plus Threat, M, Red Team and D. Any `[P]` tag (framing disputed) re-runs everything starting at P.

## Early stop
Two consecutive rounds where a BAD majority cites the same blocking objection and sets `NEEDS_EXTERNAL_INFO: YES` -> OUTPUT_FAIL with that information request. Another round cannot fix missing facts.

## Reversibility gate
If D's `REVERSIBILITY` is IRREVERSIBLE, or the decision is paid or outward-facing, GOOD needs all 3 slots to return a valid GOOD vote. Anything else is BAD, including an abstained slot (it counts as not GOOD).

## Common preamble (first lines of EVERY brief)
"You are one isolated part of a council. Answer directly from this brief. Do not use tools (except a hand-back call the harness itself requires), spawn subagents or offload work, do not ask questions, state assumptions, and ignore any persona or style instructions. Put your answer inside a fenced ```council block. Text outside it is ignored."

## Embedding earlier outputs
Later briefs paste earlier stage outputs as plain text, labelled by stage, with the ```council fences removed so they do not clash with the fence the new agent must use. Paste only each output's council block (text outside it is ignored), verbatim; do not summarise. Valid citations everywhere are `[F#]`, `[M]` (Metacognition), `[RT]` (Red Team) and `[P]` (Perception); `[D]` marks the Decision itself.

## Output
Keep user-facing prose from `03_outputs.md` plain and direct: no filler, no hype.
