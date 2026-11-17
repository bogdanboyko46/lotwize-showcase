# Engineering practices

The identifier is easy to make look better and hard to actually make better. Most of the process below
is there to tell those two apart.

## Measure before shipping, with rules written in advance

Every change that could move identification accuracy follows the same path:

1. **Write the kill rules first.** Before any run, the pass/fail criteria are written down: which fields
   may not gain wrong answers, how much exact-year accuracy may move, how many correct answers a field may
   lose. They are dated and kept with the run.
2. **Run both arms on the same machine.** The live configuration and the candidate run side by side on a
   rented cloud machine, in both modes, on the full hand-checked benchmark.
3. **Check the baseline first.** The live arm must reproduce the previous benchmark exactly. If it
   doesn't, something in the environment changed and the run is void.
4. **Read the cards, not only the totals.** Every part whose answer changed is listed by make and field.
   A gain of 37 right answers doesn't cover 5 new wrong ones if the rule allows 3.
5. **Ship or kill.** A failed rule means no ship. The result is recorded, and the next step is decided by
   what the cards show.

### A recent week, as an example

| Candidate | Result | Decided by |
|---|---|---|
| Promote a trim-colour detection model to assert automatically | +37 right, +5 wrong (limit 3) | **Killed** |
| Offer more year windows where generations overlap | Exact years flat; model names and trim lists got shorter on some parts | **Killed** |
| Re-embed ~18,000 library photos with a different image preprocessing | One part went from an honest dropdown to a confident wrong answer | **Killed** |
| Owner rule: one generation's bulb becomes a two-option dropdown | Targeted change, unit-tested, no effect elsewhere | Shipped |

Three of the four didn't ship. That is the process working, not failing.

## Honest evaluation

- **No leakage.** The retrieval library was audited for copies of benchmark photos. About 2,700 were found
  and removed. Scores dropped by 0.4 to 2.4 points per field, and the lower numbers are the published ones.
- **A fixed benchmark.** Benchmark labels are final. When a model disagrees with one, the model is the
  thing that gets investigated.
- **Per-make reporting.** An average can hide one brand failing badly, so results are always read make by make.
- **Two numbers, not one.** The public "correct or in the dropdown" figure is always paired with the
  stricter "filled in correctly" figure and a count of wrong answers.
- **Thresholds can't fix accuracy.** A confidence threshold only decides whether a value is shown or
  offered. It can't make a wrong prediction right. Accuracy work goes into retrieval, labels and coverage.

## Data quality

- **Text-only labelling.** Labels come from listing text, read by a language model that never sees the photo.
- **Features need their own evidence.** A feature such as "Full LED" or "Adaptive" is recorded only when
  that listing's own caption names it, not because the generation sometimes shipped with it. This cut
  recall sharply and removed tens of thousands of unsupported labels. The trade was made on purpose.
- **Expert knowledge, documented.** Where photos can't decide a field, such as one housing sold with two
  bulb types, the rule comes from manufacturer documentation for that exact model and year range. It is
  never inferred from a sibling model.

## Testing

| Repository | Test functions | Test files |
|---|---:|---:|
| Product (web app, services, billing, eBay integration) | ~28,000 | ~2,300 |
| Identification research (data pipeline, aggregation, rules) | ~1,100 | ~170 |
| Front-end unit tests | ~120 cases | 14 |

- **Regression tests.** Many tests pin a specific failure that was found once: a misread field on one
  generation, or a publish edge case. That failure has to stay fixed.
- **No live services.** External services are stubbed. The suite never calls eBay, the payment provider or
  the language model for real.
- **Secret scanning.** A secret scanner runs as part of the suite.
- **Parallel runs.** The suite runs in parallel in a few minutes.

## Production safety

- **Accounts.** Two-factor sign-in is required, sensitive actions ask for it again, and new devices and
  signups face a captcha.
- **Abuse limits.** Rate limits apply per account and per IP address, and request-size limits apply
  before authentication.
- **Browser hardening.** A strict Content Security Policy with per-page script hashes, plus HSTS.
- **Money and listings.** Billing webhooks are signature-verified, and billing calls carry idempotency
  keys. A guard stops one draft being published twice while a publish is in flight.
- **Releases.** Each release pins exact code and model versions together, and rollback is a single
  configuration change. Health and readiness checks run on every service.
- **Privacy.** Sellers can export all their data and delete their account immediately. No seller listing
  is used for training unless the seller submits a correction.

## Who does what

The system's design is mine:
- the identification pipeline and its output schema;
- the voting and aggregation algorithms;
- how the confidence thresholds are calibrated;
- the retrieval index and the re-ranking around it;
- how the photo library is built, cleaned and curated;
- the CI/CD pipeline;
- the evaluation method on this page.

Much of the day-to-day implementation is done with AI coding agents working in parallel, each on its own
branch with its own files. They are held to the same rules as any contributor:

- every claim must come with evidence;
- every benchmark must pass kill rules written before the run;
- nothing ships without my explicit go.

The agents add speed. The design, the measurement and the decision to ship stay with me.
