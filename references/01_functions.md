# Roster and brief templates

Every brief starts with the common preamble from `00_dispatcher.md`, then the original (redacted) question. Function agents never see each other's output. Language is built into every agent, attention into Perception, motor planning into Decision.

## Perception gate (P), runs alone
Question: what is actually being asked, and can it be decided?
Output block fields:
- `RESTATED:` the question in one or two sentences.
- `FACTS:` known facts vs `ASSUMPTIONS:` stated assumptions.
- `CONSTRAINTS:` hard limits.
- `OPTIONS:` 2-5 options, always including "Defer / gather info: <what>".
- `DECIDABLE:` YES, NO or NEEDS_INFO, with `MISSING:` up to 4 items when not YES.
Word every option neutrally and with equal charity: no loaded adjectives ("heavy", "obvious"), and do not state or hint at the answer anywhere in RESTATED, FACTS or OPTIONS. Bias: literal, filters noise. Do not pad: if the question lacks the facts any sane person would need, say NEEDS_INFO.

## Functions 1-6 (parallel, all read P's case file and options)
Each returns, per option: findings, each claim tagged `KNOWN` (stated in the brief) or `ESTIMATE` (with a one-line reason). Then `CONFIDENCE: 0-100` and `MISSED_BY_OTHERS:` the single most important thing the other functions will miss. Stay in your lane. Label your output `[F<n>]` so later stages can cite it.

| # | Function | Answers | Must produce | Bias |
|---|---|---|---|---|
| F1 | Memory (hippocampus) | What precedents and base rates apply? | 2-4 analogies with outcomes; a base rate per option marked KNOWN or ESTIMATE | Anchors on the past |
| F2 | Reasoning (dlPFC) | Which option follows logically? | Pros/cons per option; crux assumptions; hidden dependencies | Undervalues feelings and people |
| F3 | Threat (amygdala) | What can go wrong, how badly? | Worst case per option; likelihood; `REVERSIBLE`/`COSTLY`/`IRREVERSIBLE` per option; tripwires | Pessimistic on purpose |
| F4 | Reward (striatum) | What is the upside and the cost of not acting? | Expected gain per option; opportunity cost; time to payoff | Optimistic on purpose |
| F5 | Social cognition (TPJ) | Who is affected and how will they react? | Stakeholder map; reaction per option; trust and reputation effects | Over-weights others' opinions |
| F6 | Emotion (insula/limbic) | Which option fits the person's stated values, where is regret likely? | Values fit; regret forecast (1 year, 10 years); whether fear or excitement is driving the choice | Subjective |

Round 2 onward: add the objections aimed at this function and the rejected decision (labelled REJECTED).

## Metacognition (M), after F1-F6
Reads P and all current `[F#]` outputs only (no objections, no REJECTED, no Red Team). Output: `CONFLICTS:` where functions disagree; `UNSUPPORTED:` claims with no source; `CALIBRATION:` which functions look over- or under-confident; `MISSING:` information that would change the answer. Bias: sceptical of everyone.

## Red Team (RT), round 2 onward
Inputs: P's case file, the rejected decision (citation tags removed) and the STANDING objections list (see dispatcher Retry); no F outputs. Brief: "Assume the rejected decision, or any decision close to it, failed a year from now." Output: three failure stories, each with the cheapest guard against it. Adversarial. Runs in parallel with the re-run functions.

## Decision (D), after M (and RT in round 2+)
Reads P, every `[F#]` output, M, and in round 2+ the REJECTED decision, its objections and RT. May only choose among P's options (including Defer). Do not reverse a point you changed in response to an objection unless you say why. Required block:
```
DECISION: <one option, may be Defer / gather info>
RATIONALE: <at most 6 bullets, no nested bullets; each claim cites [F#], [M], [RT] or [P]>
ASSUMPTIONS: <top 3>
CONFIDENCE: <0-100>
REVERSIBILITY: REVERSIBLE | COSTLY | IRREVERSIBLE
KILL_SWITCH: <trigger that reopens the decision>
FIRST_STEP: <one action>
OBJECTIONS_ADDRESSED: <round 1: write NONE (round 1). Round 2+: one line per objection, how it is answered>
```
Uncited crux claims will be treated as blocking by the voters.
