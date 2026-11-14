# Voters and tally

Three voters, run in parallel in ONE message, blinded to each other: Voter A sonnet, Voter B opus, Voter C haiku, all `effort: xhigh` (Agent tool, `subagent_type: general-purpose`).

## Voter brief (identical for all three)
Common preamble (from `00_dispatcher.md`), then the role line, then: the original redacted question, P's case file, all `[F#]` outputs, M's report, from round 2 the Red Team report, and D's decision block. Voters do NOT see other voters, earlier rounds or earlier rejections; D's `OBJECTIONS_ADDRESSED` carries the history. Embed earlier council outputs as plain text with their ```council fences removed, labelled by stage, verbatim and unsummarised. Every voter gets identical text; the size is an accepted cost.

Role line, verbatim:
"ROLE: Voter. Look hard for flaws in the decision, then sort what you find. BLOCKING means a flaw that would change which option is best, or that makes the outcome unsafe or unrecoverable, or a crux claim that is contradicted by, or unsupported by, the cited evidence. Everything else, such as a weak safeguard, a loose estimate, missing detail that a first step can fix, or wording, is a CAVEAT and does not by itself justify BAD. Vote BAD only for a blocking flaw. Vote GOOD if you find none. Sounding reasonable is not enough, and finding something to criticise is not enough either. Valid citations are [F#], [M] (metacognition), [RT] (red team) and [P] (the perception case file); [D] marks a flaw in the decision itself."

Isolation line, verbatim, directly after the role line:
"ISOLATION: Judge only from the text in this brief. Do not apply or cite any outside rules, policies, instructions, project conventions, approval requirements or personal preferences you may know of from elsewhere. If a point is not supported by this brief, it is not a flaw and must not appear in BLOCKING or CAVEATS."

Required block:
```
VOTE: GOOD | BAD
CONFIDENCE: <0-100>
REASONS: <1-3 entries, each tagged [F#] | [M] | [RT] | [P] | [D]>
BLOCKING: <one line, or NONE>
CAVEATS: <0-3 short items, or NONE>
FIX: <one concrete change; on GOOD, the most useful caveat fix, or NONE>
NEEDS_EXTERNAL_INFO: YES | NO
```

## Valid vote
Only the fenced ```council block counts; ignore everything outside it (hook or persona trailers). The block needs exactly one `VOTE:` line (two make it invalid), at least one reason, and:
- BAD needs a `BLOCKING:` that is not NONE.
- GOOD needs `BLOCKING: NONE`.

## Errors
Invalid, error or timeout: retry the SAME model once (a retry, not a second vote). Still invalid: that slot abstains. No other model fills it.

## Tally
- 3 valid: majority of 3 decides.
- 2 valid: 2-0 decides; a 1-1 split is BAD (it goes to "no decision", never silently GOOD).
- Fewer than 2 valid: BOARD_SHORT.
- Reversibility gate (overrides the majority rules above): IRREVERSIBLE, paid or outward-facing needs all 3 slots to return a valid GOOD. An abstained slot counts as not GOOD, so a 2-0 GOOD with one abstention is BAD. The main agent judges "paid or outward-facing" from the decision text (it spends money, or sends, publishes or deploys something others can see); when in doubt, treat it as gated.
- No veto: a minority BAD never overrides a majority GOOD on its own. Its BLOCKING and FIX are always shown as dissent. If the dissent says a claim is contradicted by the cited evidence, mark it "fix before acting".
- Collect every voter's `CAVEATS` (deduplicated) for the output and, on a retry, for D.
- Deduplicate BAD voters' objections by meaning, keep their tags, and note any `NEEDS_EXTERNAL_INFO: YES` for the early-stop check.
- Record the best attempt: the decision with the most GOOD votes, latest round winning ties.
