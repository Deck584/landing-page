# Jev (TypeSafe AI) and the Brehon build: where it can help, where it cannot, and what must be true first

Research note, 19 September 2026. Prepared in a Claude Code Remote session for Declan Sweeney.
Builds on, and does not replace, the Capture Inbox row of 17/18 September 2026
("TypeSafe AI (Jev, System One typed-decision models): vendor parked, one pattern harvested", 3df6c12d-30c3-81b9).
Where this note disagrees with that row it says so and gives the source.

Status, 19 September 2026 (later the same session): on Declan's instruction the capture was closed and the work
logged as one combined ACF row, ACF-1496 (3e06c12d-30c3-81e4-a3c6-fb6d33e6e53a), whose Trigger carries the six
sequenced limbs in Section 8. Declan overruled the ACF-1390 intake freeze for that one row. The addendum in Section 9
was applied to the Capture Inbox row verbatim and the row is Done. The item 18 design document carries a dated note
naming Jev as the fourth Section 5 candidate class.

---

## 1. Verdict in six lines

1. Jev is a good architectural fit for one specific job Brehon already does in a dozen places: the small Haiku
   classifier that sits in front of an agent and returns a closed-set label. It is not a fit for anything generative.
2. The cost and latency case is weak on Brehon's own numbers. The real gains are typed output with no parse-failure
   path, a calibrated numeric confidence per decision, and a screen that physically cannot emit text.
3. The three park reasons in the 17 September row have moved. TypeSafe now publishes a DPA with EU SCCs under Irish law,
   so the Article 28 objection is answerable. Residency has not moved: single region, US West Coast, and zero data
   retention is enterprise-tier only. The Prospect Legitimacy reclassification risk is a design constraint, not a vendor
   blocker, and is avoidable.
4. Because of that, Brehon Integrity (Bedrock Frankfurt bar) stays excluded until TypeSafe offers an EU region.
   Brehon Assured already sends classification calls to a US endpoint (Anthropic direct), so for Assured the delta is a
   new sub-processor and a ZDR commitment, not geography.
5. The cleanest first landings need no client personal data at all: Sheldon's legislative relevance filter (public
   inputs) and the Layer 7 Tier 2 eval grader (synthetic fixtures). Both can start the day a dev key arrives.
6. Adopting Jev anywhere in production is a Locked Decision Register change to STACK-05 (Claude as sole model vendor),
   under the clause that already anticipates non-Claude models "in Phase 1G+ behind the same interface, evaluated per
   agent". That is the door. It is not a tooling pick.

---

## 2. What Jev is (verified facts)

Sources: community-verified API notes (usejev.dev repo, checked 19 September 2026), the ask-jev-ai public wall source,
TechCrunch, DataCamp, LangChain, The Rundown, independent benchmark repos. Vendor pages (typesafe.ai, docs.typesafe.ai)
were not reachable from this session, so vendor wording below is as quoted by third parties.

**What it does.** Takes application state (text or structured data, no images or audio) and a map of typed questions,
and returns one typed answer per question with probabilities. It generates no text. TypeSafe calls this class
"System One" models. First release 15 September 2026, early access behind a waitlist, no GA date announced.
Founded by former OpenAI researcher Diogo Almeida, San Francisco, USD 40M seed.

**Three primitives.**

| Primitive | Returns | Notes |
|---|---|---|
| `noul` | one probability 0 to 1 | binary yes/no; no separate confidence field |
| `choice` | chosen option, probability per option, confidence | up to 255 options; needs a `none` option to be honest |
| `score` | position on an ordered scale, legend, probabilities, confidence | values can land between levels; threshold, do not interpolate |

**API shape.** `POST https://api.typesafe.ai/v1/systemone`, bearer key, body `{ state, model, questions }`.
Model alias `jev-latest` currently resolves to `jev-1.13.0`, which can be pinned. Response carries `model`, `answers`
keyed by your question keys, and `usage`. Python SDK `typesafe_sdk`, JS SDK `@typesafe-ai/sdk`, or plain HTTP.
Errors 401, 422, 429, 529 with the usual retry advice.

**Limits and price.**

| Item | Value |
|---|---|
| Context | 64k tokens per request, ~32k practical budget |
| Latency (vendor) | 70 to 500 ms end to end |
| Latency (independent, 60-case run) | p50 ~380 to 420 ms, p95 ~480 to 540 ms |
| Rate limits | 250,000 tokens/s, 1,200 requests/min |
| Price | USD 0.042 per million input tokens, output free |
| Language | English primary; others untested |

**Design rules the vendor gives.** One judgment per question. Fan out every question that shares state into one
request. Reference state paths with backticks. Do not act below ~0.5 confidence; raise to >0.9 for high stakes.
Thresholds are not portable across primitives or model versions; retest on every registry edit or model bump.

**Documented weaknesses (jev-1.13 "jaggedness" page, as quoted).** Literal reading: it answers the question you wrote,
not the one you meant. No reliable counting or arithmetic. Dates compared as text. Indirection and double negatives
hurt. Large irrelevant state degrades accuracy. And the one that matters most for a guard: "state is an attack
surface", adversarial content in the state can move the answer, and the model "does not treat it as hostile by
default".

**The hallucination claim, stated correctly.** Jev cannot return a value outside the option set you gave it. It can
pick the wrong option. The 17 September row already recorded this as the SR-39 specimen; the vendor's own docs and
the community notes now say the same ("not zero hallucinations, constrained to your answer set").

**Independent calibration evidence (small, but consistent).**

| Source | Result |
|---|---|
| themsquared/jev-benchmark, 60 hand-labelled tool-call risk cases | 91.7% accuracy; every wrong answer carried hedged confidence (0.13 to 0.785); never 1.0 and wrong |
| jmanhype/jev-dspy-lab, selective-risk harness | 91.3% accuracy on answered, 4.2% abstention at a 0.7 gate, ECE 0.058, Brier 0.155 |
| Vendor workflow benchmark (as reported by third parties) | reference policy built from other models, not ground truth; Jev "falls far behind on invoice processing" |

Both independent runs have no frontier-LLM baseline, so they show that the confidence figure is worth routing on.
They do not show that Jev beats Haiku on Brehon's tasks. That comparison has to be run on Brehon fixtures.

**Distribution.** Direct API, Vercel AI Gateway, and Cloudflare Workers AI (`@cf/typesafe/jev`). Neither gateway
changes where TypeSafe processes the request, so neither is a residency answer on its own.

---

## 3. What has changed since the 17 September row

| Park reason in the row | Status on 19 September 2026 | Source |
|---|---|---|
| No Article 28 position | TypeSafe publishes a Data Processing Addendum with EU SCCs Module 2 and Module 3 incorporated, Irish law and Dublin courts, sub-processor flow-down, and a no-training commitment. Zero data retention exists but is enterprise tier only. | typesafe.ai/legal/data-processing (via search summary); fdsimms/todo issue 2781 |
| No EU data residency | Unchanged. Hosted in a single region, US West Coast. No EU region announced. | InfoWorld, fdsimms/todo issue 2781 |
| AI layer locked to Anthropic direct and Bedrock | Unchanged, but the lock itself carries the door: STACK-05 says non-Claude models can be introduced "in Phase 1G+ behind the same interface, evaluated per agent based on cost and quality data from Langfuse". | Locked Decision Register STACK-05 |
| Confidence-thresholded auto-act reclassifies the Prospect Legitimacy Protocol | Unchanged as a fact, but it is a design constraint. Jev can be one signal feeding a human-routed score without any auto-reject. | Prospect Legitimacy Protocol, EU AI Act section |
| Vendor early access, no GA | Unchanged. Waitlist; model version pinnable (`jev-1.13.0`). | TechCrunch, API notes |
| Security attestations | Nothing found. No SOC 2 or ISO 27001 statement located. | search, 19 September 2026 |

The row's re-open condition was "EU residency and an Article 28 DPA, or GA, whichever first". Half of the first limb
has landed. Declan's statement that he expects to use Jev is the other thing that has changed.

---

## 4. Canon that governs any adoption (read before building)

- **STACK-05** Claude sole model vendor at launch; non-Claude models Phase 1G+ behind the CBS-3 abstraction layer,
  evaluated per agent. Any production use of Jev is an LDR change under this clause.
- **STACK-06** provider split: Assured and Standalone on Anthropic direct (US), Integrity on Bedrock eu-central-1.
- **STACK-02** EU/UK client data in Supabase Frankfurt only; "no client personal data in Notion, Make.com, or any
  US-hosted store". Transient inference is the same bar OpenRouter was measured against and failed.
- **AI Infrastructure Decision** already places "Brigid interaction classification (both products)" on Haiku via
  Anthropic API direct, US, "pseudonymised before classification call". That is the precedent a Jev classifier
  would cite, and the pseudonymisation clause is the condition it inherits.
- **SR-63** model testing boundary: bake-offs on synthetic or public inputs only; validation numbers count only when
  produced on the surface the agent runs on. A Jev threshold table produced on synthetic fixtures is selection
  evidence; the production threshold is measured on the production path.
- **Claude Model Version Pinning Rule 7**: no literal model string outside the Pinning page and the agent config row.
  `jev-1.13.0` lives in those two places only.
- **Agent Pre-Build Playbook Step 8 Section F** (ACF-1420, landed 15 September): steps that run without approval are a
  closed set and "no step running without approval may produce a decision about an individual". Jev decisions that
  run unattended are therefore limited to routing, document class, matter category, relevance, and evidence
  recording. The 17 September row's harvest (record the confidence figure and the threshold, not just the gate) is
  the shape every landing below should take.
- **Prospect Legitimacy Protocol**: low-risk only because no automated decision excludes a prospect. Any Jev use there
  is a signal into a human-routed score, never a gate.
- **ACF-1390 intake freeze** to 14 October 2026: tooling evaluations file one Capture Inbox line. Declan can overrule
  per row, as he did for the Hunter cluster on 15 September.
- **Enterprise Security Architecture / Trinity**: a new inference vendor is a new sub-processor and a new item on the
  client DPA sub-processor schedule for Assured.

---

## 5. Candidate landing surfaces, ranked

Each row names what runs today, what Jev would do, the primitive mapping, the data that would leave Brehon, the
blocker, and a verdict. "Data class" uses the item 18 provenance classes: (a) vetted library, (b) client-supplied,
(c) third-party surface.

### 5.1 Sheldon legislative change detection and relevance filter. Verdict: best first landing.

- Today: Sheldon is Tier 2 Sonnet with a Haiku carve-out for "legislative source ingestion and initial change
  detection". Inputs are public: Oireachtas, legislation.gov.uk, WRC and tribunal feeds, Statutory Change Calendar
  sources.
- Jev: per item, `noul` "describes a change to employment law in force or announced", `choice` jurisdiction
  {ROI, NI, GB, none}, `score` relevance to Brehon's client base, `noul` "affects a statutory rate or threshold in
  the CFR rates table". Fan out in one request per item.
- Data leaving Brehon: public text only. No client data, no PII.
- Blocker: vendor gate items 1 to 3 in Section 7 (dev key, DPA signed, LDR note). Nothing else.
- Why first: it is the one place where the residency objection does not bite at all, the volume is real (daily
  scans across three jurisdictions), and a calibrated relevance score is exactly what the "Sheldon detects, Brigid
  advises on impact" chain lacks today.

### 5.2 Layer 7 Tier 2 eval grader and the Layer 2 self-review gate. Verdict: strong, build-side, start early.

- Today: the Monitoring Architecture Layer 7 addendum (ACF-1361) grades Tier 2 single-agent evals "by exact-match,
  assertion, or a cheaper model grader against a written rubric". The Layer 2 self-review loop (Buzz, Zazu, Harvey)
  has a Review node scoring drafts against CFR writing rules and brand voice.
- Jev: rubric items become `noul` per item ("cites the jurisdiction it applies", "makes a factual claim not present
  in the supplied source", "uses hedged language where the rubric demands a direct answer") and a `score` for voice.
  Deterministic checks (banned terms, em dashes, retired names) stay in code and run first.
- Data: synthetic fixtures (CBS-2 cases, the harness Sets A to D) or Brehon's own marketing copy. No client data.
- Limit: Jev cannot write the review notes a REVISE loop feeds back to Generate. It can be the PASS/ESCALATE gate,
  not the reviewer. Keep Sonnet for notes on REVISE.
- Blocker: dev key and DPA only. SR-63 is satisfied because the grader is tooling, not the agent under test.

### 5.3 Dispatch Stage 4 intent classifier. Verdict: good fit, Assured only, after the sub-processor amendment.

- Today: one Haiku call returns agent, HIGH/MEDIUM/LOW confidence, secondary agent, flags {urgent,
  multi_jurisdiction, escalation_needed}. LOW routes to Pepper and a repeated-LOW counter fires on three in a row.
- Jev: `choice` agent over the routing table plus `none`, `choice` secondary agent, `noul` urgent, `noul`
  multi_jurisdiction, `noul` escalation_needed. Confidence becomes a number written to the EV-1 tag and agent_events.
  The repeated-LOW counter becomes a threshold on a scalar, which is the Section F harvest the 17 September row
  already took.
- Data: raw client message, class (b), with client PII possible. For Assured this is the same class of text Haiku
  already receives on Anthropic direct US. For Integrity it must stay on Bedrock.
- Blocker: TypeSafe added to the Assured DPA sub-processor schedule; ZDR confirmed in writing (enterprise tier
  question); pseudonymisation step before the call per the AI Infrastructure Decision clause; keep the existing
  "classification_failed routes to Pepper" path as the fallback.
- Honest value note: Haiku's classifier costs "fractions of a cent" and ~0.2 s. Jev is not faster on the
  independent numbers and the saving at 500 queries a month rounds to zero. The gain is typed output, numeric
  confidence, and one parallel call for all five questions.

### 5.4 Pepper email triage and Nala lead scoring (Layer 2). Verdict: viable, low priority.

- Today: PEPPER_EMAIL_TRIAGE (Make.com, Haiku) labels CRITICAL/HIGH/NORMAL/LOW and tags INBOUND_INTEREST and
  ACCOUNTANT_REFERRAL. Nala's Signal Engine scores ICP fit.
- Jev: `choice` priority; `noul` per routing tag; `score` ICP fit. usejev.dev carries worked examples for both
  ("email triage priority scoring", "lead scoring").
- Data: inbound email bodies and prospect records. Prospect PII, class (c). Same STACK-02 question as 5.3.
- Blocker: as 5.3. Also note the internal tension already present in canon: Layer 2 runs on Make.com, a US host,
  while STACK-02 forbids client personal data in any US-hosted store. Whatever resolution Brehon applies to Make
  applies to Jev.

### 5.5 Prospect Legitimacy Protocol Signal 3. Verdict: viable, small, keep it a signal.

- Today: a Haiku call reads fetched website text and returns PASS, SOFT_PASS or FAIL on "does the contact name
  appear in a context indicating they work here".
- Jev: `choice` {PASS, SOFT_PASS, FAIL} with confidence recorded into `legitimacy_signals`. The score still routes
  review and flagged to Pepper; nothing auto-rejects. Classification stays low-risk.
- Data: public website text plus a contact name. Class (c). Low sensitivity.

### 5.6 Brigid v1.1 intra-agent classifier. Verdict: cleanest technical fit in the estate, gated hardest.

- Today (merged 18 September, PR #106): a strict five-field contract, routing {HAIKU, SONNET, OPUS},
  needs_external_lookup, needs_rate_lookup, trigger_family {A, B, AB, null}, confidence {high, medium, low}, with
  a fail-closed parse fallback to SONNET/low.
- Jev: `choice` routing, `noul` needs_external_lookup, `noul` needs_rate_lookup, `choice` trigger_family with
  `null` as an option, confidence native. The parse-failure branch disappears because the answer set is enforced.
  The v1.1 ruling that guards "move only downward" is preserved: a low Jev confidence can only lower the band.
- Data: the client's employment-law question, class (b), Data Tier 1 stepping to Tier 2. Brigid is Annex III.
- Blocker: everything in 5.3 plus the Annex III evidence pack. This is the surface the 17 September row named
  ("model-tier routing" is a non-individual decision) and it is the right long-term home, but it should be the
  last classifier moved, not the first, and it should never move for Integrity until an EU region exists.

### 5.7 Item 18 LLM Guard Pipeline, input guard injection detector. Verdict: fourth candidate for the Stage 11 bake-off, not a commitment.

- Today: Section 5 names ClawKeeper, Microsoft Agent Governance Toolkit and custom code. Section 2.4 (ACF-1342)
  records that a pattern-keyed detector raised HIGH against Brehon's own CLAUDE.md and sets Rule 2: measure
  false-positive rates on 200 vetted chunks, 50 client-style documents and 50 third-party items before any
  threshold is written; class (b) false positives under 5 percent or choose a different keying.
- Jev: `noul` override_attempt, `noul` embedded_instruction, `noul` obfuscated_content, `choice` policy category,
  as the usejev.dev moderation pattern lays out. Graded output fits Rule 1's provenance-tiered thresholds
  directly: log-only for class (a), flag for class (b), block for class (c), each at its own threshold.
- The countervailing fact is now on the vendor's own jaggedness page: "state is an attack surface" and the model
  "does not treat it as hostile by default". A detector that can be steered by the text it is screening is one
  layer, not the layer. The usejev.dev page says the same.
- Placement problem stands: 2.1 also does PII detection and reads the most sensitive text in the system. Split the
  layer. PII detection stays local and deterministic (or Bedrock for Integrity); only the injection questions are
  candidates for Jev, and only on Assured.
- Action now: run the Rule 2 measurement with Jev as one of the detectors, on the build side, synthetic and public
  corpus only (SR-63). Record the table on the item 18 page with detector name and version. That is a measurement
  input the design already asks for, and it costs cents.

### 5.8 Output filter (item 18 Section 2.3) and Brigid R5 jurisdiction match. Verdict: strong pattern fit, same gate as 5.6.

- `noul` "the answer applies law from a jurisdiction other than the one stated", `noul` "the answer quotes a
  statutory figure not present in the rate block", `noul` "the answer names a case not present in the citations
  array", `noul` PII leak. These are literal, closed, one-judgment questions, which is where Jev is strongest.
- Data: Brigid's generated answer, Annex III output, client context. Gate as 5.6.

### 5.9 Not a fit, do not spend time here

- **Monk substantive review.** Locked to Opus with no carve-out. The Tier 1 structural checks (placeholder tokens,
  signature block, compliance stamp, statutory floors) are regex and table lookups and should stay code; Jev is
  worse than regex at "is `[INSERT]` present" and cannot do the floor arithmetic.
- **Mulder intake triage, Scully stage decisions.** Opus, high consequence, decisions about individuals. Section F
  bars unattended use and the liability surface forbids it anyway.
- **Statutory threshold validation (R2), case name existence (R4).** Table and index lookups. Jev's own docs say
  no numbers, no dates.
- **Anything that produces text**: Quill, Brigid answers, briefs, chase content.
- **Chase Engine, Raj analytics, Louis billing.** Deterministic by design.

---

## 6. What Jev actually buys Brehon (and what it does not)

Buys:
- Typed answers with no JSON parse path, so the fail-closed fallbacks in the v1.1 classifier and Dispatch become
  dead code.
- A calibrated 0 to 1 confidence per decision, which is the scalar the 17 September harvest wants on agent_events
  `guardrail_evaluated` rows and which strengthens the Article 14 and 15 record even where a human decides.
- A screen that cannot be talked into producing text, useful on the output side of item 18.
- Many questions per call in parallel, which is how Dispatch's five fields or Brigid's five fields should be asked.

Does not buy:
- Money. Brehon's modelled AI spend is USD 180 to 280 a month at 100 Assured clients, mostly Opus and Sonnet.
  Classifier calls are already the cheap part.
- Latency. Haiku's classifier is ~0.2 s in Brehon's own spec; Jev's measured p50 is similar.
- Accuracy proof. No public head-to-head against Haiku exists. Brehon has to run its own on CBS-2 fixtures.
- Any relief on residency for Integrity.

Costs:
- A second inference vendor in a stack that has held a single-vendor line since March.
- A US-only early-access product with no located security attestations.
- Threshold re-measurement on every model bump and every routing-table edit (vendor's own rule).

---

## 7. Vendor gate: what must be true before any production call carries client text

1. Dev key obtained; model pinned to `jev-1.13.0` on the Pinning page and the agent config row (Rule 7).
2. DPA executed. Confirm which tier carries ZDR and what it costs; without ZDR, STACK-02 is not met for any
   client text.
3. LDR entry under STACK-05's Phase 1G+ clause, per agent, with the dated bake-off table as evidence and the
   surface it was produced on recorded (SR-63).
4. Assured client DPA template: TypeSafe added to the sub-processor schedule; Integrity excluded in writing until
   an EU region exists.
5. Trinity vendor review: security posture, breach notification window, sub-processor list, retention.
6. Fallback path kept on every surface (the existing Haiku classifier or `classification_failed`), so a 529 from
   TypeSafe never drops a client request.
7. Threshold table per question type, dated, stored in Supabase agent config (item 18 Section 6 boundary), with a
   re-measure rider on model or registry change.
8. Confidence figure and threshold written to agent_events on every decision (the ACF-1420 Section F harvest).

---

## 8. Recommended sequence if Declan proceeds

| Phase | What | Data leaving Brehon | Gate |
|---|---|---|---|
| 0, now | Get a dev key. Run three bake-offs on synthetic and public inputs: Dispatch classifier vs Haiku on CBS-2 routing cases; Brigid v1.1 classifier vs Haiku on harness Sets A to D; item 18 Rule 2 injection measurement (200/50/50). Record calibration tables. | none (synthetic, public) | SR-63 satisfied; no LDR change needed for a bake-off |
| 1 | Sheldon legislative relevance filter; Layer 7 Tier 2 eval grader; Layer 2 self-review PASS/ESCALATE gate | public text, synthetic fixtures, Brehon's own copy | DPA signed; LDR note |
| 2 | Pepper email triage tags; Prospect Legitimacy Signal 3; Nala ICP score | prospect PII, class (c) | ZDR confirmed; sub-processor amendment |
| 3 | Dispatch Stage 4 classifier, Assured only, numeric confidence on the EV-1 tag | client messages, class (b), Assured | pseudonymisation step; Trinity review; fallback kept |
| 4 | Brigid v1.1 classifier and item 18 output filter, Assured only | Annex III inputs and outputs | Annex III evidence pack; Lewis Silkin awareness |
| never until EU region | Anything on Integrity | | Bedrock Frankfurt bar |

Phase 0 is inside the freeze as a measurement, not an ACF row, if it is recorded on the existing Capture Inbox row
and on the item 18 page's Rule 2 table.

---

## 9. Addendum as applied to Capture Inbox row 3df6c12d-30c3-81b9 (row now Done, discharged into ACF-1496)

> ADDENDUM, 19 September 2026 (Claude Code Remote research session, Declan intent to adopt stated). Re-open
> condition partly met: TypeSafe publishes a DPA with EU SCCs Modules 2 and 3 under Irish law, no-training
> commitment, ZDR enterprise tier only. Residency unchanged: single region, US West Coast; no SOC 2 or ISO
> statement located. Adoption path is STACK-05's Phase 1G+ per-agent clause, Assured only; Integrity excluded until
> an EU region. Ranked landings: (1) Sheldon legislative relevance filter and Layer 7 Tier 2 eval grader, public
> and synthetic inputs, no client data; (2) Layer 2 triage and Prospect Legitimacy Signal 3 as signals, never
> gates; (3) Dispatch Stage 4 classifier with numeric confidence on the EV-1 tag; (4) Brigid v1.1 classifier and
> item 18 output filter last. Item 18 input guard: run the Section 2.4 Rule 2 measurement with Jev as a fourth
> detector, build side, synthetic corpus, record the table on the design page. Vendor's own jaggedness page states
> "state is an attack surface", so Jev is one guard layer, not the layer. No cost or latency case on Brehon's
> numbers; value is typed output, calibrated scalar confidence, and a text-free screen. Full note:
> research/jev-typesafe-brehon-assessment.md on deck584/landing-page, branch claude/jev-brehon-research-tlq06u.

---

## 10. Sources

Reachable and read in this session:
- Rassl/usejev `docs/jev-api-notes.md` (verified API shape, 19 September 2026) and use-case pages: intent routing,
  LLM output QA, content moderation and injection screening, invoice classification. https://github.com/Rassl/usejev
- waynesutton/ask-jev-ai (raw HTTP usage, latency and cost figures). https://github.com/waynesutton/ask-jev-ai
- themsquared/jev-benchmark (60-case calibration run). https://github.com/themsquared/jev-benchmark
- jmanhype/jev-dspy-lab (selective-risk and ECE). https://github.com/jmanhype/jev-dspy-lab
- fdsimms/todo issue 2781 (residency, ZDR tier, waitlist). https://github.com/fdsimms/todo/issues/2781
- Web search summaries of: TypeSafe blog "Introducing System One Models and Jev"; TechCrunch 18 September 2026;
  DataCamp; LangChain "Building a harness with Jev"; The Rundown; InfoWorld; docs.typesafe.ai "Jev 1.13 jaggedness";
  typesafe.ai/legal/data-processing and privacy policy; Cloudflare AI docs `@cf/typesafe/jev`; Vercel changelog.

Not reachable from this session (egress blocked), quoted only via the summaries above: typesafe.ai, docs.typesafe.ai,
techcrunch.com, langchain.com, datacamp.com, developers.cloudflare.com, vercel.com, arize.com, infoworld.com.

Brehon Notion pages read: Capture Inbox row 3df6c12d-30c3-81b9; ACF-1390; ACF-1420; ACF-783; Locked Decision Register
(STACK-02, 05, 06 extracts); AI Infrastructure Decision; OpenRouter hard boundary (SR-63); Brehon Dispatch Protocol;
Agent Interface Standard; Two-Layer Agent Architecture; Pepper Unified Build Spec (classification and triage
extracts); Monk Document Quality Gate; Legal Accuracy Architecture (Brigid guardrails); LLM Guard Pipeline item 18
design document; Agent Monitoring Architecture Layers 1 to 7; Prospect Legitimacy Protocol; Chase Engine.
