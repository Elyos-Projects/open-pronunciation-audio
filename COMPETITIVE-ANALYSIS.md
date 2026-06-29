# Competitive & Improvement Analysis — `open-pronunciation-audio`

> Analyst pass, 2026-06-29. Grounded against PLAN.md v0.1.0 and TASKS.md, with web-researched,
> cited competitor claims. Scope: open, native-speaker, CC-licensed, dialect-labeled word-level
> pronunciation audio + IPA, esp. for under-served languages.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually rigorous: it already nails the two hazards that usually sink a voice-data
project (consent/PII and accuracy), treats them as hard gates rather than checkboxes, and is
honest about the unsecured-partner gap. The review below is therefore mostly about sharpening
edges, not fixing holes.

**Native-speaker authenticity + consent — strong, with two gaps.**
- The consent model (irrevocability disclosure, ML-reuse transparency, pseudonymity, minors,
  registry-outside-repo) is best-in-class and exceeds every competitor (Forvo, Tatoeba, even
  Lingua Libre, whose own Wikipedia entry does not document how speaker consent is managed —
  see §2). Good.
- **Gap A — "native speaker" is asserted, never verified.** The plan requires a *language*
  reviewer and stores self-described `speaker.l1l2`, but there is no gate that actually
  establishes the recorded speaker is a native/competent speaker of the *labeled variety*. A
  fluent L2 speaker with an L1-influenced accent could be published as "native." Recommend an
  explicit **speaker-authenticity attestation** (self-declared L1 + variety/region) cross-checked
  by the language reviewer against the audio, recorded as a distinct field, not folded into
  `l1l2`.
- **Gap B — synthetic-vs-human is a stated guardrail but has no detection gate.** The plan
  forbids "synthetic-passed-as-native" and excludes TTS in spirit, but Path B has no control that
  *prevents* a contributor from submitting TTS/voice-cloned audio as a human recording. The
  audio-quality check measures loudness/clipping/trim, not human-vs-synthetic provenance.
  Recommend: a mandatory `audioProvenance = human-recorded | curated-open` attestation tied to
  the consent release (a real consent release implies a real human), plus a reviewer "sounds
  human / sounds synthetic" check, plus an explicit record field. This is the single most
  important *missing* gate relative to the guardrails.

**Dialect/accent labeling — excellent.** Mandatory `variety` on every clip, descriptive-not-
prescriptive rule, side-by-side varieties, no ranking, BCP-47 + ISO 639-3. This is a genuine
differentiator (§4) and is correctly enforced in the rubric (dimension 4) and metrics
(variety-labeling completeness = 100%). Minor: the plan should define a **controlled vocabulary /
registry for variety labels** (free-text "Munster Irish" vs "Irish (Munster)" will fragment),
ideally mapping to Glottolog/IETF variant subtags where they exist.

**Recording quality/standards — solid, slightly under-specified.** Opus/Ogg delivery, WAV/FLAC
master, LUFS loudness, clipping, trim, single-word content all named. Missing concrete
**thresholds** (target LUFS, max noise floor / SNR, min sample rate, silence-trim window) — those
belong in the style guide (`opa-style-001`), and the acceptance criteria should require numeric
targets, not just "loudness target." A background-noise/SNR check is notably absent and matters a
lot for field recordings of endangered languages.

**Contributor attribution + licensing (CC0/CC-BY) — correct and nuanced.** CC0-preferred,
CC-BY/CC-BY-SA to match source/community, SPDX ids, attribution string carried through, code MIT.
One subtlety the plan gets right that competitors get wrong: **you cannot re-CC0 a CC-BY-SA source**
(Lingua Libre is CC-BY-SA 4.0; Tatoeba audio "cannot use CC0"). The additive-only,
license-travels-with-item rule is correct. Recommend stating explicitly that **per-clip license is
authoritative** and the dataset is a *mixed-license* collection (not a single blanket license), so
downstream users filter by `license` field.

**Synthetic-vs-human disclosure — see Gap B above.** The *ethical-use note* and ML-reuse
disclosure are present and good; the *inbound* detection control is the gap.

**Phonetic transcription (IPA) accompaniment — a real strength and a key differentiator.** Pairing
audio with reviewed IPA is exactly what Lingua Libre/Commons/Shtooka do NOT do (their recordings
have no IPA; Wiktionary has IPA but patchy audio). The 4-dimension rubric, phonemic/phonetic
distinction, and "audio–IPA agreement" dimension are excellent. Open question #7 (require narrow
`[…]` or not) should resolve to **phonemic required, phonetic optional** to keep fan-out tasks
cheap — that's the right default and should be promoted from open-question to decision.

**Under-served-language coverage — correctly the headline, but the plan's own design fights it.**
The verified-need framing (coverage near-zero for hundreds of languages) is accurate and
well-cited by the research below. But three tensions:
- The hard channel gate (no work until an upstream channel is secured) plus the CARE community-
  consent gate means low-resource languages — the whole point — are the *hardest* to start and may
  be perpetually deferred behind "easy" high-resource curation. Recommend an explicit **portfolio
  rule**: every milestone must include ≥1 low-resource language *in progress*, not just "≥1 by
  M2."
- Reviewer capacity for low-resource languages is flagged as "High likelihood" bottleneck but has
  no concrete sourcing plan beyond "recruit." This is the real scaling constraint and deserves a
  dedicated M1 task.
- The standalone-dataset fallback is the right hedge, but for a low-resource language with no
  Wiktionary/Lingua Libre presence, the dataset *is* the only channel — the plan should say so
  plainly so low-resource work isn't blocked waiting for a Wikimedia channel that will never have
  that language.

**Scope vs `read-aloud-audio` / `place-name-pronunciation` — clean and correct.** Word/lemma/
short-phrase is the boundary; sentence/paragraph narration is explicitly `read-aloud-audio`;
place names are a sibling. The shared infrastructure (consent, audio-quality, license gate,
adapters) is reusable across all three — see §7. One overlap to adjudicate: a single proper-noun/
place-name *word* is in-scope here AND in `place-name-pronunciation`; the plans should declare
which owns single-token toponyms to avoid double-recording.

**Verdict:** Plan is delivery-ready. The two correctness findings that matter most are **Gap B
(no inbound synthetic-audio detection gate)** and **Gap A (native-speaker authenticity asserted,
not verified)** — both are guardrail-level and should be closed before any Path-B recording.

---

## 2. Competitive landscape (researched, cited)

| Source | Open? | Word-level? | IPA? | Native speaker? | Dialect-labeled? | Low-resource? |
|---|---|---|---|---|---|---|
| **Forvo** | ✗ (NC, restrictive) | ✓ | ✗ | ✓ | partial | broad but closed |
| **Lingua Libre** | ✓ CC-BY-SA | ✓ | ✗ | ✓ | partial | uneven |
| **Wiktionary / Wikidata Lexemes** | ✓ CC-BY-SA | ✓ | ✓ (text) | ✓ | partial | sparse |
| **Shtooka** | ✓ mostly CC-BY | ✓ | ✗ | ✓ | weak | stagnant |
| **Common Voice** | ✓ CC0 | ✗ (sentences) | ✗ | mixed | weak | growing |
| **Tatoeba audio** | ~ (mixed/NC) | ✗ (sentences) | ✗ | ✓ | weak | sparse |
| **RhinoSpike** | ✗ (no clear license) | ✗ (on-demand) | ✗ | ✓ | none | by request |

**Forvo** — the market leader and the one to beat. ~All words, native speakers, but **closed**:
it ran under CC BY-NC-SA 3.0 until ~end-2019, then replaced it with an "ad hoc" license that
restricts copying, modifying and redistributing the audio; reuse is non-commercial only and the
API costs money. Not openly licensed, not reusable in OER/dictionaries without friction.
Weakness: closed, no IPA, no machine-readable open dataset.
Sources: https://forvo.com/license/ , https://en.wikipedia.org/wiki/Forvo

**Lingua Libre (Wikimedia France)** — the closest *open* competitor and the natural upstream
channel. CC-BY-SA 4.0, native speakers, very fast capture (up to ~1,000 words/hour); ~500,000
recordings across 120 languages by June 2021, 310+ languages total. Weaknesses: **no IPA
transcription**, coverage extremely uneven (rich for a few European languages, near-zero for most
low-resource ones), variety labeling inconsistent, and — tellingly — its public documentation
does not describe how speaker consent is managed. This is the gap to fill, not compete with.
Sources: https://en.wikipedia.org/wiki/Lingua_Libre ,
https://meta.wikimedia.org/wiki/Lingua_Libre/Supports

**Wiktionary / Wikidata Lexemes** — open dictionary in ~500 languages with **IPA text** but
patchy audio; Lexemes passed 600k entries (Oct 2021) as structured, machine-readable data.
Strength: huge reach, has the IPA the audio projects lack. Weakness: audio coverage sparse,
AI-assisted bulk edits are policy-sensitive. This is exactly where pairing reviewed audio + IPA
adds value. Sources: https://en.wikipedia.org/wiki/Wiktionary ,
https://diff.wikimedia.org/2022/03/10/building-a-50000-pronunciation-data-repository-in-the-odia-language/

**Shtooka** — pioneer of free native-speaker word audio, mostly CC-BY (per-collection), 75,000+
records, Commons-integrated. Weakness: effectively **dormant/legacy**, narrow language set, no
IPA, weak dialect metadata. A curation source (Path A), not a live competitor.
Sources: https://shtooka.net/ , https://commons.wikimedia.org/wiki/Commons:Shtooka

**Mozilla Common Voice** — CC0, huge (~31,800 hours, 129+ languages, 250+ collecting), strong
consent/validation model. But it is **sentence-level read-aloud for ASR training, not word-level
pronunciation with IPA**, prompts crawled from Wikipedia. Adjacent, not overlapping — closer to
the `read-aloud-audio` sibling. Worth studying for its dual-upvote validation and CC0 pipeline.
Source: https://en.wikipedia.org/wiki/Common_Voice ,
https://www.mozillafoundation.org/en/blog/common-voice-18-dataset-release/

**Tatoeba audio** — 1M+ sentences with audio, native speakers, but **sentence-level** and
**mixed/uncertain licensing** (speaker's choice: CC-BY, CC-BY-SA, CC-BY-NC, or none; audio
"cannot be CC0" as it's derivative of the sentence text). Licensing minefield for reuse; not
word-level. Source: https://en.wikipedia.org/wiki/Tatoeba ,
https://tatoeba.org/en/terms_of_use

**RhinoSpike** — on-demand native-speaker recordings via a reciprocal queue, 75,000+ recordings /
87 languages, but it's a **request-exchange service with no clear open license**, not a reusable
corpus, no IPA, no dialect metadata. Source: https://rhinospike.com/about/

**FreeDict / dictionary apps** — FreeDict is open bilingual dictionary data (text, largely no
audio); commercial dictionary-app audio is licensed to forbid reuse (a hard-excluded source).

---

## 3. Gaps we can fill

The market has *open audio without IPA* (Lingua Libre, Shtooka), *open IPA without audio*
(Wiktionary text), *closed audio* (Forvo), and *sentence audio for ASR* (Common Voice, Tatoeba).
**Nobody ships open, native-speaker, word-level audio *paired with reviewed IPA*, dialect-labeled,
with rigorous consent provenance, deliberately targeting under-served languages.** Concretely:

1. **Audio + reviewed IPA as one unit.** The Lingua-Libre-meets-Wiktionary product nobody
   assembled: a clip and its phonemic transcription, both human-verified to *agree* (rubric
   dimension 2).
2. **Under-served / endangered languages first.** Every open source is rich for a handful of
   languages and empty for hundreds; we start where coverage is zero.
3. **Honest, machine-readable dialect/variety labels** on every clip — descriptive, side-by-side,
   not a single "standard."
4. **Consent + provenance as first-class, auditable data** — the thing Forvo can't offer (closed)
   and Lingua Libre/Tatoeba under-document.
5. **A clean, single mixed-license open dataset** with per-clip license/attribution/provenance —
   so OER authors, lexicographers, and (transparently) speech-tech builders can actually reuse it
   without a licensing minefield.
6. **An open, reusable collection *pipeline*** (gates, rubric, consent workflow) — the durable
   asset competitors never published.

---

## 4. Differentiators to win

**Vs Forvo (closed):** open license is the whole game. Forvo's audio is non-commercial / ad-hoc
restricted and cannot be embedded freely in OER, dictionaries, or downstream tech. Our CC0/CC-BY
corpus + IPA is reusable with zero licensing friction. We also add what Forvo lacks: **IPA**,
**machine-readable provenance**, and **a downloadable dataset**.

**Vs Lingua Libre (open but gappy):** four wedges — (1) **IPA pairing** (they have none); (2)
**under-served-language focus** (they're uneven); (3) **rigorous, documented consent + CARE
sovereignty** (they don't publicly document consent handling); (4) **enforced descriptive variety
labels + accuracy rubric** (their metadata is inconsistent). Critically, Lingua Libre is an
**upstream channel, not a rival** — we contribute audio *to* Commons/Lingua Libre and cross-link
IPA to Wiktionary/Lexemes, so we ride their distribution while filling their gaps.

**The single strongest differentiator:** **open, native-speaker word audio paired with
human-verified IPA and an honest dialect label — for languages that currently have neither —
backed by auditable consent/provenance.** No competitor combines open + audio + reviewed IPA +
dialect + under-served focus; each has at most two of these.

---

## 5. Claude API leverage (and hard limits)

The audio is human; Claude accelerates everything *around* it. The plan's donated lane means
Claude assists the human contributor, never runs headless.

**Where Claude API adds leverage:**
- **Word-list curation & batch design.** Generate/clean per-language frequency or Swadesh/learner-
  core wordlists, deduplicate against existing upstream coverage, and assemble cold-start batches
  that satisfy the M0 diversity criteria (script/tone/variety spread). High value, low risk.
- **Candidate IPA drafting.** Draft phonemic `/…/` (and optional narrow `[…]`) per the style
  guide for the stated variety, with stress/tone/length marks and a cited rationale — always as a
  *draft for human review* (the plan already says this).
- **Metadata & provenance assembly.** Normalize BCP-47/ISO 639-3 tags, SPDX license ids,
  attribution strings, variety labels to the controlled vocabulary; draft the per-item record;
  flag missing fields before submission.
- **QA / triage workflow.** Pre-screen submissions: lint IPA notation validity (phonemic vs
  phonetic slot misuse, invalid symbols), detect probable audio–IPA length/syllable mismatch,
  surface near-duplicates, route CARE/low-resource items to the right reviewer, draft error-
  taxonomy tags for the human to confirm.
- **Reviewer assist & style-guide consistency.** Summarize a batch's error mix, suggest style-
  guide clarifications, draft adapter-formatting payloads (Commons/Lexeme fields).
- **Consent-release & doc drafting.** Draft plain-language consent wording, SOPs, and the
  style guide (human/data-protection-competent review required).

**Where Claude must NOT decide (hard human gates):**
- **Native-speaker authenticity & audio quality** — must be human-verified; a model cannot
  certify a speaker is a native speaker of the labeled variety, nor approve recording quality by
  ear (rubric + reviewer).
- **Consent** — human-governed end to end; the model never asserts, infers, or waives consent,
  and never touches the PII consent registry.
- **IPA final correctness** — model drafts, a competent language reviewer *decides*; nothing
  publishes on a model's say-so (≥3/4 on every rubric dimension, human-scored).
- **Synthetic-vs-human** — the model must never certify audio as human; worse, the model must
  never *generate* the pronunciation audio (no TTS-as-native). Inbound detection assists, human
  attests.
- **License & community sovereignty** — license verification and any CARE/community-owned
  decision are human; the community, not the model, sets license/handling.

**Top 3 Claude-API ideas:** (1) **IPA candidate generator + notation linter** feeding the human
rubric review; (2) **wordlist curator + coverage-gap differ** that builds diversity-balanced
batches and avoids duplicate work; (3) **provenance/metadata normalizer + QA triage** that
assembles the record, validates tags/licenses, and routes items to the correct reviewer.

---

## 6. Ten concrete optimizations

1. **Add an inbound synthetic-audio gate (Gap B).** Mandatory `audioProvenance` attestation tied
   to a real consent release + a reviewer "human/synthetic" check + an optional automated
   synthetic-speech detector flag. Highest-priority guardrail fix.
2. **Add a native-speaker authenticity field & check (Gap A).** Speaker self-declares L1 +
   variety/region; reviewer confirms against audio; store distinct from `l1l2`.
3. **Put numeric thresholds in the audio spec.** Target LUFS, min SNR / max noise floor, min
   sample rate, silence-trim window — and add an **SNR/background-noise check** for field
   recordings.
4. **Controlled vocabulary for `variety` labels** mapped to Glottolog / IETF variant subtags, to
   stop free-text fragmentation and enable side-by-side variety grouping.
5. **Portfolio rule: ≥1 low-resource language in progress every milestone**, not deferred to M2,
   so the under-served mission isn't crowded out by easy curation.
6. **Dedicated low-resource reviewer-sourcing task in M1** (partner with ELDP/OpenSpeaks/
   university field linguists, university heritage-language programs) — reviewer capacity is the
   real scaling limit.
7. **Resolve open-question #7 now:** phonemic `/…/` required, narrow `[…]` optional — cheaper
   fan-out, documented decision.
8. **Adopt Common Voice's dual-independent-validation** for IPA/variety on a sampled fraction
   (you already track Cohen's κ ≥ 0.7) — borrow their proven two-reviewer gate.
9. **Declare per-clip license authoritative** and ship the dataset as an explicitly *mixed-license*
   collection with a `license` filter column; never imply a blanket license over CC-BY-SA sources.
10. **Adjudicate single-token toponym overlap** with `place-name-pronunciation` (who owns
    single-word place names) and publish a shared `audioProvenance`/consent schema so siblings
    don't double-record or diverge.

---

## 7. Parallel & perpendicular spin-offs

- **`place-name-pronunciation` (parallel).** Same pipeline minus most IPA complexity; toponyms are
  single-token pronunciations with the same consent/license/variety needs. Share the gate stack;
  adjudicate single-word overlap (opt. #10).
- **`read-aloud-audio` (perpendicular).** Sentence/paragraph narration — the Common Voice/Tatoeba
  space. Shares consent, audio-quality, license gates, and adapters; differs in unit and IPA. Our
  word corpus is the building block; their sentences are the next layer.
- **`oncology-glossary-multilingual` (perpendicular).** High-stakes medical term pronunciation
  across languages — reuse the audio+IPA+consent pipeline but with **risk-tier escalation and
  expert (clinical/terminology) review**; a direct beneficiary handoff for our IPA/variety method.
- **`literacy-from-zero` (perpendicular).** Letter/grapheme/syllable sounds + IPA for early
  literacy and low-literacy adults — same recording + IPA discipline, different unit (phoneme/
  grapheme), strong accessibility overlap.
- **`sign-glossary` (perpendicular).** Sign-language video glossary — *not* audio, but the same
  **consent/CARE/provenance/dialect-(regional-sign-variant)-labeling** governance transfers
  almost wholesale; a natural home for the consent + sovereignty framework.
- **Reusable `pronunciation-collection-pipeline` (the durable asset).** Factor the gate stack
  (license verify, consent registry, audio-quality + SNR check, dedup/lock, rubric review,
  additive-only adapters) into a shared Elyos package consumed by all the above. This is the
  real moat competitors never published.
- **An MCP server (`pronunciation-pipeline` MCP).** Expose tools — `curate_open_clip`,
  `draft_ipa`, `lint_ipa_notation`, `check_audio_quality`, `find_coverage_gaps`,
  `assemble_provenance_record`, `route_to_reviewer` — so any agent can drive the human-gated
  pipeline. Read/draft tools only; consent, final IPA, license, and CARE decisions stay human.

---

## 8. Open questions

The plan's own open questions (first channel, target languages + low-resource pick, default
license per channel, reviewer competence bar, consent wording, retention/withdrawal, κ threshold,
narrow-IPA requirement) all stand. Additional ones surfaced by this analysis:

1. **Synthetic-audio detection:** is a reviewer ear + attestation sufficient, or do we need an
   automated detector — and what's the false-accept tolerance? (Tied to Gap B.)
2. **Native-speaker definition:** what exactly qualifies someone as a "native speaker of variety
   X," and who adjudicates heritage/semi-speakers (common in endangered-language work)?
3. **Variety-label authority:** which registry (Glottolog? IETF subtags? a project-owned list)
   governs variety names, and who maintains it?
4. **Low-resource channel reality:** for a language with no Wikimedia presence, is the standalone
   dataset the *primary* channel from day one (and is that acceptable for the outcome metric)?
5. **Toponym ownership** vs `place-name-pronunciation` — single source of truth for single-word
   place names.
6. **Reviewer sourcing partnerships** — ELDP, OpenSpeaks, university field-linguistics / heritage-
   language programs: which are realistic, and what do they need in return?
7. **Dataset license heterogeneity** — do downstream reuse partners accept a mixed-license corpus,
   or do we need CC0-only and CC-BY-SA-only sub-distributions?

---

### Summary (for the caller)

`open-pronunciation-audio` targets a real, well-documented gap and the PLAN is delivery-ready and
unusually rigorous on consent and accuracy. **Top 3 competitors:** **Forvo** (market leader,
native-speaker word audio, but closed — CC-BY-NC-SA until ~2019, now an ad-hoc non-commercial,
restrictive license; no IPA; no open dataset); **Lingua Libre** (Wikimedia, genuinely open
CC-BY-SA, ~500k recordings / 120+ languages, but **no IPA**, uneven low-resource coverage, and
undocumented consent handling — best treated as an upstream channel, not a rival); and
**Wiktionary / Wikidata Lexemes** (open, has IPA *text* across ~500 languages but patchy audio).
Adjacent-but-not-overlapping: Common Voice and Tatoeba are sentence-level read-aloud for ASR, not
word+IPA; Shtooka is a dormant CC-BY curation source; RhinoSpike is an on-demand exchange with no
open license.

**Single strongest differentiator:** open, native-speaker, word-level audio **paired with
human-verified IPA and an honest dialect label, for under-served languages that have neither** —
backed by auditable consent/CARE provenance. No competitor holds more than two of {open, audio,
reviewed IPA, dialect-labeled, low-resource}.

**Top 3 Claude-API ideas:** (1) IPA candidate generator + notation linter feeding the human
rubric; (2) wordlist curator + coverage-gap differ that builds diversity-balanced batches and
avoids duplicate work; (3) provenance/metadata normalizer + QA triage that validates tags/
licenses and routes items to the right reviewer. In all three Claude drafts/triages; humans decide
native-speaker authenticity, audio quality, consent, final IPA, synthetic-vs-human, and CARE/
license.

**Two most important plan-correctness findings:** (A) **no inbound synthetic-audio detection
gate** — the "no synthetic-passed-as-native" guardrail is stated but unenforced on Path B
submissions; add an `audioProvenance` attestation + reviewer human/synthetic check. (B)
**native-speaker authenticity is asserted, not verified** — add an explicit speaker-authenticity
attestation cross-checked by the language reviewer, stored distinct from self-described `l1l2`.
Both are guardrail-level and should be closed before any new recording.
