# Output templates

The templates here fix the structure; keep the prose plain and direct. Save the transcript path with every output.

## OUTPUT_SUCCESS
First line, short: `Brain Council decides: <the option in one short phrase> (<n>/<valid> GOOD, round <R>/3[, unanimity required]).` Then the full decision in one or two sentences.
Then: rationale (up to 4 bullets, keep the `[F#]`/`[M]`/`[P]` citations), assumptions, confidence, reversibility, kill-switch, first step.
Then caveats (the voters' merged `CAVEATS`) and dissent: every BAD voter's blocking reason and fix, even on a 2-1 pass, marked "fix before acting" when it says a claim contradicts the cited evidence. State once that all voters are Anthropic models, so their errors are correlated. Footer: "Advice only. Approve it before anything irreversible."

## OUTPUT_FAIL (3 rejections or early stop)
`The brain could not come up with a definitive decision after <R> rounds.`
Then: best attempt with its tally; standing objections (the dispatcher's STANDING list: all objections from all rounds including the last, deduplicated by meaning, with the number of rounds that raised each); what would resolve it (the latest round's M missing-information list plus any NEEDS_EXTERNAL_INFO items; at most 4, in M's own order, then "(<n> more in the transcript)" if cut). Then ask: halt the skill, perform another run, or accept the best attempt yourself?

## OUTPUT_NEEDS_INFO (gate)
The restated question plus at most 4 missing items. No fan-out cost was spent. Ask: halt the skill, or perform another run once you supply them?

## OUTPUT_BOARD_UNAVAILABLE
Which voter slots failed and why. This is distinct from undecidable and from a rejection. Ask: halt, or perform another run?
