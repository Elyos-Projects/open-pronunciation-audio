# PLAN — open-pronunciation-audio

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## Executive summary

For most of the world's languages there is no free, reliable way to hear how a word is
pronounced or to read its phonetic transcription. Major commercial pronunciation sources are
closed (Forvo's audio is not openly licensed; dictionary apps lock audio behind terms that
forbid reuse), and the openly-licensed audio that does exist — Wikimedia's Lingua Libre,
Wiktionary/Commons recordings, Shtooka — is uneven across languages and almost absent for
low-resource and endangered ones. The result: a learner of Spanish has thousands of clips; a
learner of, say, a Celtic, West African, or Indigenous American language often has none, and no
IPA either.

`open-pronunciation-audio` builds an **openly-licensed corpus of word-level pronunciation audio
paired with IPA transcriptions, across many languages**, where every clip carries **verifiable
provenance, an open license, and — for newly recorded audio — an archived, informed speaker
consent release**. The work fans out as small reviewable units: **one (word × language ×
variety) = one task**. Two source paths feed it: (A) **curate** existing openly-licensed audio
(Lingua Libre, Commons, Shtooka) after verifying its license *and* its consent provenance; and
(B) **record** new audio with volunteer native speakers under a full consent workflow — the only
practical way to cover languages that have nothing today.

Success is **not** "clips produced." It is **word entries that are live and usable** — merged
into an upstream open resource (Wiktionary / Wikidata Lexemes / Lingua Libre / Commons) or
published as an Elyos open dataset — so learners, educators, lexicographers, and revitalization
communities can actually hear and read the pronunciation, with the language **variety honestly
labeled** (descriptive, never "the one correct accent").

The project is **low risk** overall (open educational content), but it has two non-obvious
hazards that drive the whole design: **(1) speaker consent and privacy** — a human voice is
personal, identifying data, and an open license is effectively **irrevocable**, so consent must
be explicit, informed, and honest about that permanence; and **(2) accuracy** — a wrong IPA or a
mislabeled accent is worse than nothing, and IPA for many languages needs native-speaker or
phonetician review. Both are handled by hard gates, not by trusting a model. The binding
guardrails are **open license only** (no Forvo, no scraped dictionary audio), **consent on every
recorded clip**, and **data sovereignty (CARE)** for Indigenous and endangered-language
material.

This document is honest about what is not yet in place: **no upstream partner or language
community is secured**, and the Wiktionary/Lingua Libre contribution pathways and any
revitalization partnership must be validated before scaling. M0 is a thin, manual, end-to-end
slice that proves one word can travel from source-or-recording to *live upstream* with full
consent and provenance, and earns the right to scale.

## Problem & beneficiaries

**Who is helped (primary beneficiaries):**

- **Language learners**, especially of **low-resource and endangered languages** for which no
  free pronunciation audio or IPA exists today. Hearing a native speaker and reading the IPA is
  one of the highest-leverage aids for acquiring a new language's sound system.
- **Educators, tutors, and OER authors** who need reusable, openly-licensed pronunciation models
  to embed in lessons, flashcards, and dictionaries without licensing friction.
- **Language-revitalization communities** working to teach and transmit their own languages, who
  benefit most from a free, community-owned audio + IPA record — *and whose ownership of that
  material this project is bound to respect* (see CARE, below).

**Secondary beneficiaries:**

- **Lexicography and dictionary projects** (Wiktionary, Wikidata Lexemes, community
  dictionaries) whose entries become richer and more usable.
- **Accessibility uses** — read-aloud support, low-literacy and dyslexia aids — where hearing a
  word matters more than reading it.
- **The open commons and, downstream, equitable speech technology.** Open pronunciation data can
  help build TTS/ASR for under-served languages that the market ignores. This benefit is
  **public and shared** (CC0/CC-BY), not a hand-off to one company; see the reuse-ethics note in
  *Data, licensing & compliance* and the for-profit guardrail below.

**The verified need.** The underlying need is well documented and not in dispute: the coverage
gap in free pronunciation resources for most languages is visible directly in Lingua
Libre/Wiktionary coverage statistics (rich for a handful of languages, near-zero for hundreds),
and pronunciation is a recognized core component of language learning and lexicography. The
proposal marks the *underlying* need as real (low risk, clear public benefit).

**The partner gap (honest).** A documented need is **not** a secured delivery channel or a
consenting speaker community. No specific upstream maintainer (a Wiktionary/Lingua Libre
community workflow owner), and no specific **language community or revitalization organization**,
has yet agreed to receive contributions, supply or vet speakers, or co-own low-resource-language
material. **Partner / requestor: TO BE SECURED.** Until at least one upstream channel *and*, for
any community-owned language, that community's consent are confirmed, task-level `verifiedNeed`
is set conservatively to `false` for delivery- and community-dependent tasks. M0 includes
securing a channel as a hard exit criterion.

## Goals and non-goals

**Goals**

- Produce **accurate, variety-labeled** word-level pronunciation **audio + IPA** for many
  languages, prioritizing those with little or no free coverage today.
- Attach to every item a **verifiable open license, full provenance, and (for recorded audio) an
  archived informed-consent release**.
- Contribute items **upstream** (Wiktionary / Wikidata Lexemes / Lingua Libre / Commons) and/or
  publish a clean **Elyos open dataset**, with attribution and provenance intact.
- Make the work safely parallelizable: a clear transcription + recording style guide, a
  self-contained per-item task unit, consent and license gates, and a human accuracy-review gate.
- Be **descriptive, not prescriptive** — label *which* variety/accent a clip represents and
  never present one as "correct" or another as "wrong."
- Respect **community ownership** of Indigenous/endangered-language material (CARE principles).

**Non-goals**

- **Not** ingesting, hosting, transcribing, or even caching **non-open / unclear-license** audio
  (explicitly excludes Forvo audio, scraped dictionary/app audio, YouTube rips, etc.).
- **Not** recording or republishing any voice **without a documented, informed consent release**.
- **Not** building a hosted SaaS, a public "upload your voice" service, or a general TTS product.
- **Not** producing **voice clones, impersonations, or speaker-identification** tooling, or any
  dataset designed to imitate a *specific identifiable person's* voice (see refusal guardrails).
- **Not** sentence- or paragraph-level read-aloud / narration (that is `read-aloud-audio`, a
  sibling project) — scope here is **word/lemma/short-phrase** pronunciation.
- **Not** prescriptive "standard/correct pronunciation" judgments, accent ranking, or
  dialect-policing.
- **Not** medical/clinical speech assessment, forensic voice analysis, or any high-stakes use of
  voice data.
- **Not** etymology, definitions, or full dictionary authoring (we add pronunciation + IPA to
  existing entries; we do not write the dictionary).

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Vanity counts ("clips generated") are explicitly **not**
the headline; **live-and-usable upstream/published** is.

| Metric | Baseline | Target (first 2 quarters post-M0) |
|---|---|---|
| Word entries with audio + IPA **live upstream or in a published open dataset** | 0 | ≥ 1,000 across ≥ 3 languages |
| **Languages covered**, of which **low-resource/endangered** | 0 | ≥ 3 covered, **≥ 1** low-resource/endangered |
| **Consent compliance** on newly recorded clips (signed release archived & linked) | n/a | **100%** (hard gate; any uncovered clip is a stop-the-line incident) |
| **License-compliance violations** (non-open audio touched/ingested) | 0 | **0** (hard gate) |
| **Provenance completeness** (license + source + variety + speaker-or-source metadata recorded) | n/a | 100% of published items |
| **IPA accuracy** (mean score on the 4-dimension rubric below) | n/a | ≥ 3.5 / 4 mean; nothing published below 3/4 on any dimension |
| **Audio quality pass rate** (meets the audio spec: format, loudness, no clipping, trimmed) | n/a | ≥ 95% of published clips |
| **Reviewer-corrected rate** (IPA/variety edited before publish) | n/a | ≤ 25%, trending down |
| **Inter-reviewer agreement** on a double-reviewed sample | n/a | ≥ 0.7 (Cohen's κ or % exact-match on the rubric); recalibrate rubric if below |
| **Variety-labeling completeness** (every clip names its language + variety/accent) | n/a | 100% |
| **Beneficiary validation** (a learner / native speaker / educator confirms a batch is usable) | none | ≥ 1 qualitative review per language |

**IPA accuracy rubric.** Every transcription is scored 1–4 on four independent dimensions:

1. **Phonemic correctness** — the broad `/…/` transcription matches the word's phonology in the
   stated variety.
2. **Phonetic/audio agreement** — the transcription matches what the linked audio actually says
   (audio and IPA describe the same realization, or any difference is intentional and noted).
3. **Notation validity** — valid IPA, correct stress/length/tone marks, phonemic `/…/` vs
   phonetic `[…]` used correctly.
4. **Variety fidelity** — the labeled variety/accent is accurate and the transcription is
   consistent with it (no silent mixing of accents).

**Error taxonomy** (tag every correction/rejection): **wrong phoneme/symbol**, **wrong
stress/tone/length**, **audio–IPA mismatch**, **wrong or missing variety label**, **notation
error** (e.g. phonetic detail in a phonemic slot). The taxonomy mix is reported alongside the
reviewer-corrected rate to tune the style guide.

**Counting "coverage" (reproducible).** Coverage is `entries-with-pronunciation ÷ in-scope
entries` for a **named wordlist snapshot per language at a recorded date** (e.g. a frequency
list, a Swadesh/learner core list, or a partner-supplied list). "Has pronunciation" is detected
per host (a non-empty audio + IPA on the Wiktionary/Lexeme entry, or a record in the published
dataset). Baseline and every delta are computed against the same snapshot so they are comparable
over time and across contributors. Percentage-point coverage targets are deferred until M0
measures a real baseline per chosen language — inventing one now would be dishonest.

## Scope

**In scope**

- **Word / lemma / short-phrase** pronunciation **audio** (curated-open or newly recorded with
  consent) and **IPA** transcription (phonemic, plus phonetic where useful), **variety-labeled**.
- Per-item **license + attribution + provenance** capture; per recorded clip, a linked
  **consent-release record**.
- A flagging/dedup pass (license/consent gate, audio-quality check, near-duplicate detection,
  community-sovereignty flag) routing items to the right review.
- Upstream contribution adapters (Wiktionary/Wikidata Lexemes, Lingua Libre/Commons) and/or a
  published Elyos open dataset with a documented schema.
- Published, versioned **IPA-transcription & variety-labeling style guide** and **recording +
  consent workflow**.

**Out of scope**

- Non-open / unknown-license audio (hard exclusion, incl. Forvo and scraped sources).
- Any recording **without** a documented consent release; any voice of an **identifiable
  individual** without their informed consent.
- Sentence/paragraph narration, audiobooks, TTS model training/serving, or hosted voice services.
- Voice cloning, impersonation, speaker-ID/biometric matching, or covert/surveillance recording.
- Definitions, etymologies, full dictionary authoring, or translation of the words themselves.
- Prescriptive "correct accent" rulings; accent ranking; any discriminatory framing of varieties.
- Clinical/forensic speech analysis or any high-stakes voice use.

## Solution approach & architecture

A **content/data pipeline** project with supporting **adapter code**, run in the **donated lane**:
a human runs their own agent interactively (and, for path B, coordinates a consenting speaker);
Elyos prepares the per-item task workspace and opens the upstream PR/edit. The CLI never runs an
agent headless and never authenticates a coding agent.

**Two source paths (different risk handling):**

- **Path A — Curate existing open audio.** Pull a clip from an approved openly-licensed source
  (Lingua Libre, Commons, Shtooka). Verify **both** the license *and* the upstream **consent
  provenance** (these platforms record speaker release/recording terms). Re-pair or verify the
  IPA. Lower consent risk because the original platform obtained the release; we carry that
  provenance forward, we do not re-assert it.
- **Path B — Record new audio.** Coordinate a **volunteer native speaker**, capture audio to the
  audio spec, and obtain a **signed, informed consent release** (the only way to cover languages
  with nothing today). Higher consent responsibility; the consent gate is mandatory.

**Pipeline (per item)**

1. **Select item & variety** — choose a `(lemma, language, variety)` from a named wordlist; tag
   language with **BCP-47 / ISO 639-3** and an explicit **variety/accent** label.
2. **Source the audio** — Path A: verify license + consent provenance, capture canonical URL,
   license, attribution, speaker terms. Path B: confirm speaker consent (below), then record.
   *If license is not clearly open, or (recorded) consent is not documented → drop; do not
   ingest, cache, or publish.*
3. **Consent capture (Path B, mandatory).** Speaker gives **informed, written consent** that
   covers: open license (CC0 or CC-BY), worldwide redistribution and **derivatives**, that
   **a voice recording is identifying personal data**, that **an open license is effectively
   irrevocable** (a published CC0/CC-BY clip cannot be universally recalled), the **pseudonymity
   option** (attributed name *or* pseudonym; the voice itself remains identifying), and that the
   data may be reused by anyone, **including for machine learning** (transparency, no per-use
   veto). **Minors** require verified guardian consent. The release is archived and linked by
   `consentRef`; only **minimal, self-described** speaker metadata is stored (variety, region,
   L1/L2, **age band** not exact age, optional self-described attributes).
4. **Pre-flight flagging & dedup** — automated checks: license/consent gate; **audio-quality
   check** (format, sample rate, loudness, clipping, leading/trailing silence, single-word
   content); **near-duplicate** detection (same lemma+language+variety+speaker already present →
   route to human, don't auto-skip); **community-sovereignty flag** (Indigenous/endangered
   language → CARE protocol).
5. **Claim & lock** — before work, the session **claims** the item with a lock keyed on
   `(language, lemma, variety)` (TTL + owner stamp) in the shared `records/` index, so parallel
   sessions cannot double-record/double-submit. Released on completion, rejection, or TTL expiry.
6. **IPA transcription** — produce phonemic `/…/` (and phonetic `[…]` where useful) per the
   style guide for the **stated variety**, with stress/tone/length marked. Candidate IPA may be
   model-drafted or carried from a sourced reference (e.g. Wiktionary), but is **always** subject
   to review (next step). Cite the IPA's source.
7. **Human accuracy review** — a reviewer with the relevant language skill scores the item on the
   4-dimension IPA rubric and tags any failure with the error taxonomy, **listens to the audio
   against the IPA**, and confirms the variety label. Nothing below 3/4 on any dimension is kept.
8. **Upstream contribution / dataset publish (additive only)** — an adapter formats the accepted
   item for the host (Lingua Libre/Commons upload + Wiktionary/Lexeme audio & IPA fields) or adds
   it to the open dataset. The adapter **must not overwrite** an existing non-empty
   audio/IPA/variety — it adds (a clip per variety is legitimate) or routes a proposed
   improvement to human review; it detects edit conflicts and re-fetches rather than clobbering.
   License, attribution, provenance, and (recorded) `consentRef` travel with the contribution.
9. **Track to live** — record merge/publish status; a deed is "done" only when **live and
   usable** upstream or in the published dataset.

**Components**

- `style-guide/` — IPA transcription & variety-labeling conventions; recording spec; consent
  workflow and release template.
- `pipeline/` — selection, license + consent verification, audio-quality check, flagging/dedup,
  and the per-item task-record schema.
- `adapters/` (Elyos-conformant; all host-specific logic lives here):
  - `adapters/lingualibre/` — Lingua Libre / Wikimedia Commons audio upload + metadata.
  - `adapters/wiktionary-lexeme/` — Wiktionary / Wikidata Lexeme audio & IPA contribution.
  - `adapters/dataset/` — emit the clean, schema-validated Elyos open dataset (CC0/CC-BY).
- `records/` — per-item provenance records (the dataset/audit artifact).
- `consent/` — the **consent registry**: archived release references keyed by `consentRef`
  (release artifacts stored securely; never committed in the open repo with PII).

**Tech stack & conventions.** TypeScript, ESM, pnpm workspaces. Adapter/pipeline **code** is
MIT. **Audio + IPA content** is licensed to match the host / chosen output license (CC0-1.0
preferred for maximal reuse; CC-BY-4.0 or CC-BY-SA-4.0 where the source/community requires).
**Audio format:** Opus/Ogg Vorbis (Commons-compatible) for delivery; WAV/FLAC accepted as
recording master. DCO sign-off (`git commit -s`) on all code and PRs.

**Per-item task record (data model, draft)**

```
entryId            stable id within the project
lemma              headword / short phrase being pronounced
language           BCP-47 tag + ISO 639-3 (e.g. "ga" / "gle", "es-MX")
variety            explicit accent/variety label (e.g. "Munster Irish", "Mexican Spanish")
ipaPhonemic        broad transcription /…/
ipaPhonetic        narrow transcription […] (nullable)
ipaSource          wiktionary | reference-dict | model-drafted-reviewed | phonetician
audio              { fileUrl, format, durationMs, sampleRate, loudnessLufs }
audioSource        curated-open | recorded
license            SPDX-style id: "CC0-1.0" | "CC-BY-4.0" | "CC-BY-SA-4.0" | "PD"
attribution        required attribution string (speaker/author + source)
speaker            { displayName | pseudonym, varietySpoken, l1l2, ageBand, region } (minimal)
consentRef         id into the consent registry (REQUIRED if audioSource=recorded; null if curated-open)
consentProvenance  for curated-open: how the upstream consent/release was verified
sovereigntyFlag    none | community-owned (CARE) — gates community-governed handling
existingUpstream   whether host already has audio/IPA for this lemma+variety (add/route, never overwrite)
audioQuality       pass | fail (+ reasons)
reviewStatus       drafted | needs-language-reviewer | reviewed | rejected
upstreamRef        Commons/Lexeme/PR id once submitted
publishStatus      submitted | merged | published | declined
provenance         tool, model, date, reviewer (+ reviewer language/credential)
```

**Key decisions**

- *Consent is a hard gate, not a checkbox.* No recorded clip is published without a linked,
  archived release; curated clips must carry verified upstream consent provenance.
- *Descriptive, not prescriptive.* Every clip names its variety; we add varieties side by side
  and never rank or "correct" them.
- *Upstream-first, with a clean dataset as the durable fallback.* We contribute into the host;
  the published Elyos open dataset is the provenance-rich record and the deliverable when no
  single host fits (e.g. a community that wants to self-host).
- *Human language reviewer is non-optional.* No IPA/variety publishes without review by someone
  with the relevant language competence.

## Data, licensing & compliance

**This is the critical, conservative section.**

**Allowed sources (only these classes).**

- **Lingua Libre** recordings (CC-BY-SA / CC0 as recorded) — designed for open pronunciation
  audio with speaker release.
- **Wikimedia Commons** audio under CC-BY, CC-BY-SA, CC0, or Public Domain.
- **Shtooka** and similar **explicitly openly-licensed** pronunciation collections.
- **Newly recorded** audio from consenting volunteer speakers (Path B).
- **IPA** carried from openly-licensed/again-citable references (e.g. Wiktionary IPA under
  CC-BY-SA) **or** model-drafted **and reviewed** — IPA notation itself is factual, but the
  *source* and review are recorded.

**Explicitly excluded.** **Forvo** audio (proprietary terms), dictionary/app audio, scraped
streaming/video audio, any clip whose license is unknown or "all rights reserved," and any
recording lacking documented consent. **Unknown = excluded.** No ingestion, no caching.

**Hard license + consent gate (before any ingestion, recording, or publication):**

1. Audio license is verified CC-* / CC0 / PD (Path A) — or the clip is newly recorded (Path B).
2. **Recorded audio:** a signed, informed **consent release** exists and is linked by
   `consentRef` (see consent model below). **Curated audio:** upstream **consent provenance** is
   verified and recorded (`consentProvenance`).
3. The license id and required **attribution** string are recorded.
4. Provenance is recorded: source URL/recording session, language + variety, retrieval/recording
   date, IPA source, tool/model used, reviewer (+ reviewer language competence).

**Speaker consent model (Path B).** Informed, written, and **honest about permanence**. The
release explicitly states:

- The recording will be released under an **open license** (CC0 or CC-BY) permitting **anyone**,
  **worldwide**, to use, **redistribute, and make derivatives**, **including for machine
  learning / speech technology** (transparency; no per-use veto, because open licenses do not
  allow one).
- A **voice recording is identifying personal data**; releasing it is a meaningful choice.
- An open license is **effectively irrevocable** — once distributed under CC0/CC-BY, copies
  cannot be universally recalled. The speaker may request we cease *our* further distribution and
  remove items from *our* dataset, but cannot guarantee removal of downstream copies. This is
  stated up front.
- **Pseudonymity option:** the speaker may be credited by name or by a pseudonym; the **voice
  itself remains identifying** regardless.
- **Minors:** verified parental/guardian consent is required; default is to prefer adult speakers
  and treat minors' recordings as out of scope unless a community partner explicitly governs it.
- Only **minimal, self-described** speaker metadata is stored (variety, region, L1/L2, **age
  band**, optional self-described attributes) — enough to make the clip useful, no more.

**Indigenous & endangered languages — data sovereignty (CARE).** For any community-owned
language, the project follows the **CARE principles** (Collective benefit, Authority to control,
Responsibility, Ethics) alongside FAIR. Community-owned material requires the **community's**
authority over whether and how it is recorded, licensed, and published; the community may set the
license, request restricted handling, or decline — and **the project complies**. `sovereigntyFlag
= community-owned` routes the item to the CARE protocol before any publication. This guardrail
**overrides** the default "maximize open reuse" preference.

**Output licensing.** Default **CC0-1.0** for maximal reuse where the source/community allows;
**CC-BY-4.0** / **CC-BY-SA-4.0** to match the source (e.g. Lingua Libre CC-BY-SA) or community
preference. Adapter/pipeline **code** is MIT.

**Privacy / PII stance.** Voice is personal data and (in several jurisdictions) potentially
biometric/special-category. The pipeline: collects the **minimum** speaker metadata; never infers
or asserts private attributes; offers pseudonymity; stores consent releases in a secure
**consent registry** (PII **never** committed to the open repo or written to logs); and excludes
any voice of an identifiable individual without their consent.

**Reuse-ethics note (anti-misuse, transparency).** Because the corpus is open, it **can** be used
to train TTS/ASR — a deliberate public good for under-served languages. The project publishes an
**ethical-use note** (no impersonation of identifiable individuals; no deceptive synthetic-voice
deepfakes; attribution where required) and discloses ML reuse to speakers up front. We **do not**
build, and **refuse to produce**, datasets engineered to clone a *specific person's* voice.

**Robots / automation policy.** Selection and contribution honor each host's API terms, rate
limits, and bot/automation policies (Commons/Lingua Libre/Wikidata). No scraping around access
controls.

## Quality, review & risk gates

**Risk tier: low (baseline), with medium escalations.** Per the proposal and the good-deed
definition, open pronunciation content is **low** risk. Specific paths escalate to **medium**:

- **low** — curating an already-consented, openly-licensed clip and verifying its IPA in a
  well-resourced language with a competent reviewer.
- **medium** — **newly recorded audio** (consent + PII handling), **IPA for languages requiring
  specialist/native review**, **tonal/complex-script** languages, and any **community-owned**
  (CARE-flagged) material.
- **high (out of default flow)** — none expected; any use touching clinical/forensic voice
  analysis, or recording of identifiable individuals in sensitive contexts, is **declined**
  (these are non-goals/refusals, not an escalation path).

**Required review before a deed is "done":**

1. **License + consent check passed** (recorded license/attribution; `consentRef` for recorded
   clips, or verified `consentProvenance` for curated clips; CARE compliance if flagged).
2. **Audio-quality check passed** (format, loudness, no clipping, trimmed, single-word content).
3. **IPA accuracy review** by a human with the relevant language competence — mandatory for every
   item, scored on the 4-dimension rubric, audio listened against the IPA, variety confirmed;
   nothing publishes below 3/4 on any dimension.
4. **Style-guide conformance** (phonemic vs phonetic notation, stress/tone/length, variety label
   present and accurate).

**Definition of Shipped (project-level).** A reviewed item is **live and usable** — merged into
the upstream host (audio on Commons/Lingua Libre + audio/IPA on the Wiktionary/Lexeme entry) or
**published in the Elyos open dataset** — with license, attribution, provenance, variety label,
and (recorded) consent reference recorded. Generated-but-not-published ≠ shipped.

## Roadmap & milestones

Phased; each milestone has measurable exit criteria. M0 is a deliberately thin, manual,
end-to-end slice — it must prove one item can travel from source/recording to **live upstream or
published**, with full consent and provenance, before anything scales.

**M0 — Foundation & cold-start (prove one end-to-end deed).**
Goal: publish the consent framework, the IPA/variety style guide, and the record schema; secure
≥ 1 upstream channel; and ship a tiny hand-curated + hand-recorded batch end-to-end.

**Sequencing — securing a channel (and, for community languages, community consent) is a hard
gate.** No record/curate/publish task starts until ≥ 1 upstream channel is confirmed, and no
community-owned-language work starts without that community's consent. **Kill/pivot criteria:**
if, after the time-boxed channel-securing effort, no candidate channel will accept
human-reviewed, openly-licensed, consented contributions, the project **pivots** (next candidate
channel / publish as standalone open dataset) or **stops** — it does not accumulate
unpublishable clips or, worse, recordings it cannot ethically ship.

**Candidate upstream channels (priority order, with acceptance posture):**
1. **Lingua Libre + Wikimedia Commons** — purpose-built for open pronunciation audio with a
   speaker-release flow; posture: *welcoming, but bot/edit and license norms apply — confirm the
   recording-release and upload workflow before volume.*
2. **Wiktionary / Wikidata Lexemes** (audio + IPA fields) — large reach; posture: *additive
   audio/IPA generally welcomed; AI-assisted edits at volume are policy-sensitive — pre-engage.*
3. **A named language community / revitalization partner** (esp. low-resource) — highest value;
   posture: *requires community authority + CARE agreement; TO BE SECURED.*
4. **Standalone Elyos open dataset** — always-available fallback so the metric is **decoupled
   from any single host**.

**Decision tree (which channel first):** Lingua Libre/Commons workflow confirmed for the target
language? → start there + cross-link to Wiktionary/Lexeme. Else a community partner with a CARE
agreement? → start there. Else → publish the cold-start batch as the standalone open dataset.
Else (nothing viable) → kill/pivot.

**M0 batch-selection criteria (must exercise every path, not 30 easy English words).** The
hand-curated/recorded cold-start batch is spread to prove the hard cases: it covers **both source
paths** (≥ some **curated-open** from Lingua Libre/Commons and ≥ some **newly recorded** with a
real consent release); **≥ 2 languages** including **≥ 1 low-resource/endangered**; at least one
item with a **non-Latin script** and one with **tone or vowel length**; and at least one item
exercising the **variety label** (two accents of the same word). A batch of 30 trivially-easy
high-resource words is explicitly disallowed.

Exit criteria:
- Speaker **consent & release framework v1** published (Path B) + curated-consent-provenance
  verification rules (Path A).
- IPA-transcription & variety-labeling **style guide v1** published.
- License + consent + provenance **record schema** finalized and applied to the batch.
- ≥ 1 upstream channel **confirmed** (or the standalone open-dataset path validated) — closes the
  partner gap for at least one language; **gates** all record/publish work.
- Cold-start batch selected per the criteria above and **≥ 30 entries (audio + IPA) live upstream
  or published**, each with license + attribution + provenance, and **100% of recorded clips with
  an archived consent release**.
- **Baseline coverage** measured for one chosen language against a named wordlist snapshot.

**M1 — Pipeline & adapters (make it repeatable).**
Goal: turn the manual slice into a repeatable, policy-compliant pipeline.
Exit criteria:
- Lingua Libre/Commons ingestion adapter **and/or** Wiktionary/Lexeme contribution adapter
  working and policy-compliant; dataset emitter working.
- License + consent pre-ingest gate, audio-quality check, and flagging/dedup operational and
  routing to review; consent registry in use.
- Reviewer workflow + IPA rubric + recording/consent SOP documented; **≥ 2 language reviewers**
  onboarded (covering ≥ 1 low-resource language).
- ≥ 200 entries live upstream/published across ≥ 2 languages; acceptance & accuracy measured;
  **0 consent/license violations.**

**M2 — Controlled scale (go deep in a few languages).**
Goal: meaningfully move coverage in ≥ 3 languages incl. ≥ 1 low-resource, quality held.
Exit criteria:
- ≥ 1,000 entries live upstream/published across ≥ 3 languages (≥ 1 low-resource/endangered).
- IPA accuracy ≥ 3.5/4 mean; reviewer-corrected ≤ 25% trending down; audio-quality pass ≥ 95%.
- ≥ 1 beneficiary validation (learner / native speaker / educator) per language.
- **0** consent/license violations; CARE compliance for any community-owned material.

**M3 — Broaden & sustain (more languages, durable ops, community ownership).**
Goal: add languages and a maintenance + community-stewardship model.
Exit criteria:
- ≥ 2 additional languages onboarded with confirmed channels and (where applicable) CARE
  agreements.
- Outcome-tracking dashboard (live/published counts + coverage per language) maintained.
- Documented maintainer + reviewer rotation; consent-registry retention/withdrawal procedure
  live; sustainability plan in effect.

## Work breakdown

The itemized, schema-mapped backlog lives in **`TASKS.md`** (same directory), organized by the
milestones above, with one task per work unit, stable IDs (`opa-<area>-NNN`), a
size/risk/reviewer table per milestone, acceptance criteria for the key tasks, and a complete
example Task JSON for the first M0 task. The defining characteristic of the production backlog is
fan-out: at scale, **one (word × language × variety) = one task**, drawn from a milestone-scoped,
wordlist-bounded batch.

## Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — owns the style guide, consent framework, schema, pipeline,
  adapters, and the quality bar.
- **Language reviewers / rotation:** a pool with native/specialist competence per target
  language; at least one able to review tonal/complex-script languages and one for each
  low-resource language in scope. Rotation documented in M1.
- **Steward (last-mile owner):** owns each upstream-channel relationship and shepherds items to
  *live/published* — accountable for "delivered, not merged."
- **Consent steward / data-protection contact:** owns the consent registry, the release template,
  retention, and any withdrawal requests; the point of contact for speaker concerns.
- **Community liaison (CARE):** for any community-owned language, secures and maintains the
  community's authority/agreement and ensures CARE compliance.
- **Partner / requestor:** TO BE SECURED — the upstream collection(s), speaker communities,
  and/or revitalization organizations.
- **Expert reviewers (risk tiers):** specialist/native reviewer for medium items (new recordings,
  tonal/complex-script, low-resource, CARE). No high-risk path in scope (such uses are declined).
- **Beneficiary validators:** learners, native speakers, and educators who confirm batches are
  usable.

## Dependencies & integrations

- **Lingua Libre / Wikimedia Commons (MediaWiki + Wikibase API)** — open pronunciation audio
  upload + metadata + speaker-release workflow; subject to community norms, bot policy, rate
  limits. Pre-engagement (confirmed recording-release + upload workflow) is a required exit
  artifact of the channel-securing task.
- **Wiktionary / Wikidata Lexemes** — audio + IPA contribution; additive a11y/lexeme edits;
  pre-engagement for AI-assisted edits at volume.
- **Reference IPA sources** — Wiktionary IPA (CC-BY-SA) and openly-licensed dictionaries, with
  citation; model-drafted IPA always reviewed.
- **Audio tooling** — open codecs/tools (Opus/Ogg, FLAC, ffmpeg/loudness tooling) for the
  audio-quality check; no proprietary dependency.
- **License/lang metadata** — SPDX identifiers, CC license deeds, BCP-47 / ISO 639-3 registries.
- **Elyos platform pieces** — `packages/cli` (task workspace + PR prep, donated lane), the Task
  schema (`packages/schema`), and `adapters/` for all host-specific code. The CLI never runs an
  agent headless and never authenticates a coding agent. The **funded lane is not used** here
  (no metered API execution required); if ever used, a hard per-task budget cap applies.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Recorded clip published without valid/informed consent | Low | High | Hard consent gate; `consentRef` required for every recorded clip; archived signed release; consent steward sign-off; stop-the-line on any uncovered clip | Consent steward |
| Non-open / unclear-license audio ingested (e.g. Forvo, scraped) | Low | High | Hard pre-ingest license gate; explicit exclusion list; "unknown = excluded"; record license+attribution; stop-the-line on violation | Steward |
| Wrong IPA / wrong variety label published | Medium | High | Mandatory human language-reviewer; 4-dimension rubric + error taxonomy; audio listened against IPA; specialist for tonal/complex-script; nothing < 3/4 merged | Maintainer / Reviewers |
| Prescriptivism / accent discrimination (one accent as "correct") | Medium | Medium | Descriptive-only style guide; mandatory variety label; add varieties side-by-side; no ranking language; review checks framing | Maintainer / Reviewers |
| Indigenous/endangered material handled without community authority | Medium | High | CARE protocol; `sovereigntyFlag` routes to community liaison; community sets license/handling or declines; overrides default open-reuse | Community liaison |
| Speaker later wants withdrawal but open license is irrevocable | Medium | Medium | Disclose irrevocability **up front** in the release; honor cessation of *our* distribution + dataset removal; document downstream limits | Consent steward |
| Misuse of open voice data for impersonation / deepfakes | Medium | Medium | Refuse to build voice-clone/impersonation datasets; publish ethical-use note; disclose ML reuse to speakers; no identifiable-person targeting | Maintainer |
| Minor recorded without proper guardian consent | Low | High | Prefer adult speakers; minors out of scope unless a community partner governs it with verified guardian consent | Consent steward |
| No upstream channel / community partner secured → nothing ships | Medium | High | M0 exit = confirmed channel or validated standalone-dataset path; `verifiedNeed=false` until secured; metric decoupled from any single host | Maintainer / Steward |
| Host community rejects AI-assisted contributions at volume | Medium | Medium | Honor bot policy; concrete pre-engagement artifact; start small; human-reviewed not spam; dataset fallback so one rejection isn't fatal | Steward |
| Poor audio quality (clipping, noise, multiple words) shipped | Medium | Medium | Automated audio-quality check (format, loudness, clipping, trim, single-word); ≥95% pass target; reject-and-redo | Maintainer |
| PII leak (consent releases / speaker data committed or logged) | Low | High | Consent registry stores PII outside the open repo; never in logs/receipts/commits; minimal metadata; pseudonymity option | Consent steward |
| Reviewer capacity (esp. low-resource langs) becomes the bottleneck | High | Medium | Reviewer rotation; batch sizing; recruit native reviewers per language; only scale fan-out to review capacity | Maintainer |
| Duplicate/redundant clips across sessions | Medium | Low | Near-duplicate detection (lemma+lang+variety+speaker); TTL'd per-item claim/lock; wordlist-bounded batches | Maintainer |

## Security & privacy

- **Threat surface.** Primarily content-integrity and **privacy**: consent failures, PII leakage
  of speakers, license violations, wrong transcriptions, and downstream misuse (impersonation).
  Adapters also touch external write APIs (Commons/Lingua Libre/Wikidata edits, repo PRs).
- **Secrets handling.** Host API tokens (MediaWiki/Wikidata, GitHub) are supplied via the human's
  own environment, **never** written to logs, receipts, records, or committed files (per
  CLAUDE.md). The donated lane means the human authenticates with their own account; Elyos stores
  no agent credentials.
- **PII / privacy.** Voice is personal/identifying (potentially biometric/special-category) data.
  Consent releases live in a **consent registry outside the open repo**; only minimal,
  self-described speaker metadata is stored; no inference of private attributes; pseudonymity
  offered; a documented retention + **withdrawal** procedure (cease our distribution + remove from
  our dataset, with downstream limits disclosed).
- **Abuse / misuse prevention.** No covert/surveillance recording; no recording of identifiable
  individuals without consent; no voice-clone/impersonation datasets; refuse and flag (per Elyos
  guardrails) any request to ingest non-open audio, to target/deceive, to build surveillance or
  impersonation tooling, or to override community data sovereignty. Honor host rate limits/bot
  policy; every publish is human-reviewed.

## Sustainability & maintenance

- **Post-delivery ownership.** Items contributed upstream (Commons/Lingua Libre/Wiktionary) are
  maintained by those communities once live — the durable, non-Elyos-dependent outcome. The
  standalone open dataset is versioned and mirrored for longevity.
- **Additive contributions only.** We add audio/IPA/varieties; we never overwrite existing host
  data. A clip per variety is legitimate; conflicts are resolved by re-fetch-and-re-evaluate,
  never clobbering. Provenance records original contribution and final disposition.
- **Consent archive.** The consent registry is retained for the life of the dataset (so any item
  can be traced to its release) with a documented withdrawal procedure; PII is access-controlled.
- **Project maintenance.** Maintainer keeps the style guide, consent template, and adapters
  current with host API/policy changes; reviewer rotation keeps language-review capacity alive.
- **Outcome tracking.** A lightweight dashboard tracks live/published counts and coverage per
  language over time, so impact is visible after the active push ends.
- **Wind-down.** If a channel closes, in-flight items are completed or cleanly withdrawn; the
  standalone dataset preserves delivered work; the consent archive and provenance log record
  final disposition.

## Open questions

- Which upstream channel is secured first — Lingua Libre/Commons, Wiktionary/Lexemes, a named
  community partner, or the standalone dataset? (Blocks M0 exit.)
- Which **languages** does M0 target, and which **low-resource/endangered** one — and is there a
  willing **speaker community** with CARE authority for it?
- Default **output license** per channel/community — CC0 (max reuse) vs CC-BY/CC-BY-SA (source or
  community requirement)? Confirm before volume.
- Exact **credential/competence bar** for a language reviewer per language, and how
  tonal/complex-script items are reviewed when no specialist is available.
- The precise **consent-release wording** (irrevocability, ML-reuse, pseudonymity, minors) —
  reviewed by someone competent in data protection / open licensing (this is process guidance,
  **not legal advice** to speakers).
- Retention period and **withdrawal** mechanics for the consent registry; downstream-copy limits.
- Inter-reviewer agreement threshold + sampling rate to keep the IPA rubric calibrated.
- Should phonetic `[…]` (narrow) transcription be required, or phonemic `/…/` only by default?

## References

- `C:\code\elyos\CLAUDE.md` — Elyos work rules, lanes, quality bar, refusal guardrails.
- `C:\code\elyos\docs\good-deed-definition.md` — good-deed criteria and risk tiers.
- `C:\code\elyos\packages\schema\src\schemas.ts` — Task JSON schema.
- `C:\code\elyos\planning\ROADMAP.md` — portfolio roadmap (Track 4 entry for this project).
- Lingua Libre — Wikimedia open pronunciation-audio project + speaker-release workflow.
- Wikimedia Commons structured data + MediaWiki/Wikibase API + bot policy.
- Wiktionary / Wikidata Lexemes — audio + IPA fields.
- Creative Commons license suite (CC0-1.0, CC-BY-4.0, CC-BY-SA-4.0) and Public Domain.
- International Phonetic Alphabet (IPA); BCP-47 and ISO 639-3 language tagging.
- CARE Principles for Indigenous Data Governance (Collective benefit, Authority to control,
  Responsibility, Ethics), alongside FAIR.

---

## Appendix A — Improvements applied

The following 25 specific improvements were made to the first draft and are reflected throughout
the document above (and in `TASKS.md`):

1. **Speaker consent promoted to a headline guardrail** with an explicit **irrevocability
   disclosure** (an open license cannot be universally recalled) — Executive summary, Data &
   compliance, and task `opa-consent-001`.
2. **Phonemic `/…/` vs phonetic `[…]`** distinction made mandatory, and **variety/accent
   labeling** required on every clip — style guide, rubric dimension 3/4, record schema.
3. **BCP-47 / ISO 639-3** language tagging required on every record (not free-text language).
4. **CARE principles + Indigenous/endangered data sovereignty** added as a binding,
   default-overriding guardrail with a `sovereigntyFlag`, community liaison role, and task
   `opa-doc-002`.
5. **Minors' consent** (verified guardian, prefer adults, otherwise out of scope) addressed
   explicitly.
6. **Non-open sources excluded by name** (Forvo, scraped dictionary/app/video audio) and curated
   audio requires verified **upstream consent provenance**, not just an open license.
7. **Two distinct source paths** (curate-open vs newly-recorded) with different risk handling and
   different consent treatment, instead of one undifferentiated pipeline.
8. **Audio-quality spec + automated check** (format Opus/Ogg, sample rate, loudness/LUFS, no
   clipping, trimmed silence, single-word content) added as a gate and a metric.
9. **IPA accuracy rubric (4 dimensions) + error taxonomy** replace a binary "accuracy passed,"
   mirroring the project's review rigor.
10. **Per-item claim/lock (TTL + owner) + near-duplicate detection** so parallel donated sessions
    cannot double-record/double-submit.
11. **Additive-only contribution** rule (never overwrite host audio/IPA; a clip per variety is
    legitimate; conflict → re-fetch, not clobber).
12. **Reproducible "coverage" metric** defined per named wordlist snapshot per language, so
    baselines and deltas are comparable.
13. **Kill/pivot criteria + a decision tree** for channel securing, with a **standalone open
    dataset** fallback so the metric is decoupled from any single host.
14. **Cold-start batch-selection criteria** that force every path (both source paths, ≥1
    low-resource, non-Latin script, tone/length, two-variety example) — no "30 easy English
    words."
15. **Pseudonymity option** + "voice itself is identifying" disclosure + **minimal speaker
    metadata** (age band not exact age) for privacy.
16. **Secrets handling** spelled out (host tokens via the human's env; never logged/committed).
17. **Consent registry/ledger** linking each recorded clip to an archived signed release, stored
    **outside the open repo** (no PII in commits/logs); target **100% consent compliance**.
18. **Outcome-based metrics** (live/usable entries) made the headline; vanity "clips generated"
    explicitly demoted.
19. **Beneficiary validation** by learners/native speakers/educators added as a metric and task
    (`opa-research-003`).
20. **Anti-prescriptivism / non-discrimination** stance added (no "correct" accent, no ranking,
    descriptive-only) — Goals, style guide, risk table.
21. **Reuse-ethics note for ML** (open data may train TTS/ASR — a public good for under-served
    languages — disclosed to speakers; no impersonation/deepfake datasets).
22. **Risk table given owners + likelihood/impact** for every row, including consent, PII, CARE,
    and misuse rows specific to voice data.
23. **Sustainability** expanded with consent-archive retention + a documented **withdrawal**
    procedure and downstream-limit disclosure.
24. **Schema-valid example Task JSON** added with **honest `verifiedNeed`** (true only for the
    self-evident foundational artifact; false for delivery/community-dependent tasks).
25. **Explicit refusals** enumerated (covert/surveillance recording, recording identifiable
    individuals without consent, voice-clone/impersonation/speaker-ID tooling) per Elyos
    refusal guardrails.

---

## Review sign-off

**Reviewer pass (senior staff engineer + TPM), 2026-06-28.** Checked against the PLAN spec, the
good-deed definition, and CLAUDE.md guardrails. Findings and resolutions:

- **Measurable metrics?** Yes — every success metric has a baseline (0 or n/a) and a target, with
  reproducible counting definitions for coverage and accuracy. Percentage-point coverage targets
  honestly deferred to a real M0 baseline rather than invented. ✔
- **Enforceable gates?** Yes — license gate, consent gate (`consentRef`/`consentProvenance`),
  audio-quality check, IPA rubric (≥3/4 on every dimension), CARE flag, and "live/published =
  shipped." Each gate names an owner. ✔
- **Risks with owners + mitigations?** Yes — 14-row table; the highest-impact rows (consent,
  license, CARE, IPA accuracy, PII, misuse) each have a concrete, testable mitigation. ✔
- **License / PII / consent / expert-review guardrails?** Yes — open-only sourcing with named
  exclusions; consent registry outside the repo; CARE override for community-owned material;
  native/specialist review for medium items; no high-risk path (clinical/forensic declined). ✔
- **Sequencing sound?** Yes — consent + style + schema + channel precede any
  record/curate/publish; channel-securing is a hard gate with kill/pivot and a dataset fallback;
  community consent gates community-owned work. ✔
- **Schema-valid tasks?** Verified — `TASKS.md` maps every field to `packages/schema`; the example
  JSON includes all required fields with valid enums and honest `verifiedNeed`. ✔
- **Issues fixed during review:** (a) clarified that **curated** clips need consent *provenance*
  even when the file license is open (consent ≠ license); (b) made the **standalone dataset** an
  explicit always-available delivery path so "no host" never blocks delivery *and* so the project
  never holds recordings it cannot ship; (c) tightened the irrevocability + withdrawal language so
  speakers are told the truth up front; (d) made `verifiedNeed=false` the default for
  delivery/community-dependent tasks, `true` only for the foundational consent framework.
- **Residual human decisions (see Open questions):** target languages + low-resource pick +
  willing speaker community; first secured channel; default output license per channel;
  consent-release wording reviewed by a data-protection/open-licensing competent person (process
  guidance, **not legal advice**).

**Status:** Ready for maintainer review and channel/community securing. No blocking
inconsistencies found.
