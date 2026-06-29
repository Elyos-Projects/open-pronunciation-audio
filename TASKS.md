# TASKS — open-pronunciation-audio

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

Backlog for the `open-pronunciation-audio` good-deed project. Read alongside `PLAN.md` (same
directory).

## How these tasks map to Elyos

Every task here becomes a **Task JSON** validated by `packages/schema/src/schemas.ts`. Field
mapping:

- `id` — stable, e.g. `opa-consent-001` (the table's ID column).
- `title` — the task title.
- `project` — `"open-pronunciation-audio"` for all tasks.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (see each task).
- `lane` — `"donated"` for all tasks (the human runs their own agent and, for recordings,
  coordinates a consenting speaker; the CLI preps the workspace + PR). No `funded` tasks, so
  `fundedBudgetUsd` is not required.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["language","education","open-culture","accessibility","privacy"]`.
- `riskTier` — `low | medium | high`. **Baseline low**; **medium** for newly-recorded audio
  (consent/PII), IPA for languages needing specialist/native review, tonal/complex-script, and
  any community-owned (CARE) material. No `high` tasks (clinical/forensic voice uses are declined,
  not escalated).
- `urgent` — boolean (default `false`; nothing here is time-critical).
- `deliverable` — `pr | dataset | document | translation`. Style guide / consent framework / SOPs
  → `document`; record schema, batches, provenance records, the published corpus → `dataset`;
  adapters / pipeline code & upstream contributions → `pr`. (Audio+IPA contributions map to
  `pr`/`dataset`; there is no `audio` enum, so we use the closest deliverable and describe the
  artifact in `output`.)
- `tokenEstimate` — `small | medium | large` (the table's Size column).
- `status` — `open | in-progress | review | delivered | done` (all start `open`).
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — task body fields.
- `requestor` — proposer/maintainer; `verifiedNeed` — **`false`** for delivery- and
  community-dependent tasks until an upstream channel and/or speaker community is secured (see
  PLAN "partner gap"); **`true`** only where the need is self-evident and not channel/community
  dependent (the foundational documents).
- `outputLicense` — MIT for code; **CC0-1.0** preferred for audio/IPA content (max reuse),
  **CC-BY-4.0 / CC-BY-SA-4.0** to match the source/community; **CC-BY-4.0** for guides/docs.

At production scale this project fans out: **one (word × language × variety) = one task**, drawn
from a milestone-scoped, **wordlist-bounded** batch (the `*-batch-*` and cold-start `data` tasks
are the templates for that fan-out).

---

## Milestone M0 — Foundation & cold-start

Prove one pronunciation entry (audio + IPA) can travel from source-or-recording to *live upstream
or published*, with full consent and provenance.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| opa-consent-001 | Author speaker consent & release framework v1 (incl. irrevocability, ML-reuse, pseudonymity, minors) | writing | medium | medium | document | — | Maintainer + Consent steward |
| opa-style-001 | Author IPA-transcription & variety-labeling style guide v1 | writing | medium | low | document | — | Maintainer + Language reviewer |
| opa-data-001 | Define per-item record: license + provenance + consent + audio-quality schema | data | small | low | dataset | consent-001, style-001 | Maintainer |
| opa-research-001 | Secure ≥1 upstream channel (Lingua Libre/Commons, Wiktionary/Lexemes, community partner, or validated standalone dataset) | research | medium | medium | document | — | Maintainer + Steward |
| opa-research-002 | License + consent-provenance verification rules for curated sources (Lingua Libre/Commons/Shtooka); exclusion list | research | small | medium | document | — | Steward |
| opa-data-002 | Cold-start batch — ≥30 entries (audio + IPA) across ≥2 languages incl ≥1 low-resource, both source paths, covering script/tone/variety | data | medium | medium | dataset | research-001 (channel gate), consent-001, style-001, data-001, research-002 | Language reviewer(s) |
| opa-pr-001 | Publish/merge cold-start batch upstream (or to standalone dataset) + measure baseline coverage | maintenance | medium | medium | pr | research-001, data-002 | Steward |

**Sequencing — channel + consent are hard gates.** `research-001` blocks `data-002` and `pr-001`:
no entry is recorded, curated, or submitted until ≥ 1 delivery path is confirmed (a host channel
or the validated standalone-dataset path). For any **community-owned** language, the community's
**CARE consent** also gates `data-002`. If the time-boxed channel-securing effort confirms no
viable path, the project hits its **kill/pivot criteria** (next candidate channel, or standalone
dataset, or stop) rather than producing unpublishable — or worse, unshippable recorded —
material.

**Key task acceptance criteria**

- **opa-consent-001** (consent & release framework v1)
  - [ ] Provides a plain-language **speaker release** stating: open license (CC0 or CC-BY),
        worldwide redistribution + **derivatives**, that a **voice recording is identifying
        personal data**, and that an open license is **effectively irrevocable** (downstream
        copies cannot be universally recalled).
  - [ ] Discloses **ML/speech-technology reuse** is permitted (transparency; no per-use veto) and
        includes the project's **ethical-use note** (no impersonation of identifiable
        individuals; no deceptive synthetic-voice deepfakes).
  - [ ] Offers a **pseudonymity option** and states the voice itself remains identifying.
  - [ ] Requires verified **guardian consent** for minors; default treats minors as out of scope
        unless a community partner governs it.
  - [ ] Defines the **consent registry** (where signed releases are archived, keyed by
        `consentRef`) and mandates PII is stored **outside the open repo** and never logged.
  - [ ] Defines a **withdrawal procedure** (cease our distribution + remove from our dataset; note
        downstream limits) and a retention policy.
  - [ ] Published, versioned, licensed CC-BY-4.0. *(Process guidance, **not legal advice** to
        speakers; wording to be reviewed by a data-protection/open-licensing-competent person.)*

- **opa-style-001** (IPA & variety style guide v1)
  - [ ] Defines **phonemic `/…/` vs phonetic `[…]`** usage and when each is required.
  - [ ] Requires an explicit **variety/accent label** on every entry and a **descriptive,
        non-prescriptive** rule (no "correct" accent; no ranking; add varieties side-by-side).
  - [ ] Specifies **stress, tone, and length** marking and handling of **non-Latin scripts**.
  - [ ] Mandates **BCP-47 + ISO 639-3** language tagging.
  - [ ] Specifies the **audio spec** (Opus/Ogg delivery; sample rate; loudness target; no
        clipping; trimmed silence; single-word/short-phrase content).
  - [ ] Defines the **4-dimension IPA rubric** + **error taxonomy** reviewers will use.
  - [ ] Published, versioned, licensed CC-BY-4.0.

- **opa-research-001** (secure ≥1 channel)
  - [ ] Evaluates candidate channels in priority order (Lingua Libre/Commons → Wiktionary/Lexemes
        → named community partner → standalone dataset) with each one's acceptance posture, and
        applies the PLAN decision tree to pick the first to pursue.
  - [ ] Produces a **pre-engagement exit artifact** for the chosen channel: confirmed recording-
        release/upload workflow (Lingua Libre/Commons) or talk-page intent + bot/edit agreement +
        volume ramp (Wiktionary), **or** a CARE agreement (community partner), **or** a validated
        standalone-dataset publish path.
  - [ ] Records the channel's required **output license** and attribution format.
  - [ ] Documents the host's bot/automation policy and rate limits.
  - [ ] States the time-box and the **kill/pivot criteria** if no channel is secured.
  - [ ] On success, flips `verifiedNeed=true` for tasks targeting that channel.

- **opa-data-002** (cold-start batch of ≥30)
  - [ ] Batch spans **≥2 languages incl ≥1 low-resource/endangered**, **both source paths**
        (some curated-open + some newly recorded), **≥1 non-Latin script**, **≥1 tone/length**
        item, and **≥1 two-variety** example — not 30 easy high-resource words.
  - [ ] Every curated clip verified **open license + upstream consent provenance**; every recorded
        clip has an **archived signed release** linked by `consentRef`.
  - [ ] Every entry passes the **audio-quality check** and conforms to the style guide; IPA scored
        on the 4-dimension rubric by a competent **language reviewer**; nothing < 3/4 kept.
  - [ ] Any **community-owned** item cleared under the **CARE** protocol before inclusion.
  - [ ] No private-attribute inference about any speaker; minimal metadata only.

**M0 Definition of Done:** consent framework v1 + IPA/variety style guide v1 + record schema
published; ≥ 1 upstream channel (or standalone-dataset path) confirmed; **≥ 30 entries (audio +
IPA) live upstream or published** with full license + attribution + provenance; **100% of
recorded clips have an archived consent release**; baseline coverage measured for one language;
**zero license or consent violations**.

---

## Milestone M1 — Pipeline & adapters

Turn the manual slice into a repeatable, policy-compliant pipeline.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| opa-adapter-001 | Lingua Libre / Wikimedia Commons audio-upload + metadata adapter | code | large | medium | pr | research-001, data-001 | Maintainer |
| opa-adapter-002 | Wiktionary / Wikidata-Lexeme audio + IPA contribution adapter | code | large | medium | pr | research-001, data-001 | Maintainer |
| opa-adapter-003 | Standalone open-dataset emitter (schema-validated, CC0/CC-BY) | code | medium | low | dataset | data-001 | Maintainer |
| opa-pipeline-001 | License + consent pre-ingest gate (hard exclude unknown/no-consent) + consent registry | code | medium | medium | pr | research-002, data-001, consent-001 | Steward + Consent steward |
| opa-pipeline-002 | Audio-quality check + near-duplicate detection + per-item claim/lock | code | large | medium | pr | data-001, style-001 | Maintainer |
| opa-pipeline-003 | IPA candidate generation + variety/sovereignty flagging pass | code | medium | medium | pr | style-001, data-001 | Maintainer |
| opa-doc-001 | Reviewer workflow + IPA rubric + error taxonomy + recording/consent SOP | writing | small | low | document | style-001, consent-001 | Maintainer + Language reviewer |
| opa-batch-001 | First repeatable run — ≥200 entries across ≥2 languages, merged/published | data | large | medium | pr | adapter-001 OR adapter-002 OR adapter-003, pipeline-001, pipeline-002, doc-001 | Reviewer rotation |

**Key task acceptance criteria**

- **opa-pipeline-001** (license + consent gate)
  - [ ] Rejects any clip whose license is not verifiably CC-* / CC0 / PD (**unknown = excluded**;
        Forvo/scraped sources hard-blocked by the exclusion list).
  - [ ] Requires a valid `consentRef` for every `audioSource=recorded` item and a verified
        `consentProvenance` for every `audioSource=curated-open` item; blocks otherwise.
  - [ ] Records SPDX-style license id + attribution + source/recording metadata per accepted item;
        emits an auditable decision log with **no PII and no secrets**.
  - [ ] Consent registry stores releases **outside the open repo**, access-controlled.

- **opa-pipeline-002** (audio quality + dedup + lock)
  - [ ] Audio-quality check enforces format/sample-rate/loudness/no-clipping/trim/single-word;
        emits pass/fail + reasons; failures route to redo.
  - [ ] Near-duplicate detection on `(lemma, language, variety, speaker)` routes collisions to
        human confirmation (no silent auto-skip).
  - [ ] Per-item claim/lock (TTL + owner stamp) prevents two parallel sessions double-recording or
        double-submitting the same item.

- **opa-adapter-001 / opa-adapter-002** (upstream adapters)
  - [ ] Submit human-reviewed audio (+ IPA, for Lexeme/Wiktionary) via the host API with correct
        license + attribution, honoring bot policy and rate limits.
  - [ ] **Additive-only:** never overwrite an existing non-empty audio/IPA/variety; a clip per
        variety is allowed; on edit conflict, re-fetch and re-evaluate rather than clobber.
  - [ ] Tokens supplied by the human's environment; never logged or committed.
  - [ ] Record the resulting edit/revision/PR id as `upstreamRef`.

- **opa-batch-001** (first ~200 run)
  - [ ] ≥ 200 entries merged/published across ≥ 2 languages; acceptance + accuracy measured.
  - [ ] All passed license+consent gate → audio-quality check → flagging → human language review →
        adapter/dataset submission; **0 consent/license violations.**

**M1 Definition of Done:** ≥ 1 adapter (or dataset emitter) live and policy-compliant; license +
consent gate, audio-quality check, and flagging/dedup operational; consent registry in use;
reviewer workflow documented with **≥ 2 language reviewers** onboarded (≥ 1 low-resource); ≥ 200
entries merged/published with measured acceptance + accuracy; zero violations.

---

## Milestone M2 — Controlled scale

Go deep in a few languages, with quality and consent held.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| opa-batch-002 | Scale to ≥1,000 entries across ≥3 languages incl ≥1 low-resource | data | large | medium | pr | batch-001 | Reviewer rotation |
| opa-doc-002 | Indigenous/endangered language **data-sovereignty (CARE)** protocol | writing | medium | medium | document | consent-001 | Community liaison + Maintainer |
| opa-research-003 | Beneficiary validation with learners / native speakers / educators | research | small | low | document | batch-001 | Maintainer |
| opa-data-003 | Quality metrics dataset (IPA accuracy, audio quality, consent completeness, reviewer agreement, error mix) | data | small | low | dataset | batch-001 | Maintainer |

**Key task acceptance criteria**

- **opa-batch-002** (scale run)
  - [ ] ≥ 1,000 entries live upstream/published across ≥ 3 languages (≥ 1 low-resource/endangered).
  - [ ] IPA accuracy ≥ 3.5/4 mean; reviewer-corrected ≤ 25% trending down; audio-quality pass
        ≥ 95%.
  - [ ] **0** consent/license violations; **100%** of community-owned items CARE-cleared.

- **opa-doc-002** (CARE protocol)
  - [ ] Defines how community **authority** is secured (who consents, on what terms) and how
        license/handling/decline decisions are recorded and honored.
  - [ ] Specifies the `sovereigntyFlag` routing and the community-liaison sign-off gate before any
        publication of community-owned material.
  - [ ] States that CARE **overrides** the default open-reuse preference.

- **opa-research-003** (beneficiary validation)
  - [ ] ≥ 1 learner / native speaker / educator reviews a published batch per language and confirms
        the audio + IPA + variety label are accurate and usable.
  - [ ] Findings feed back into the style guide / reviewer checklist.

**M2 Definition of Done:** ≥ 1,000 entries across ≥ 3 languages (≥ 1 low-resource) at the quality
bar; CARE protocol published and applied; ≥ 1 beneficiary validation per language; quality metrics
dashboarded; zero consent/license violations.

---

## Milestone M3 — Broaden & sustain

Add languages and a durable maintenance + community-stewardship model.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| opa-pipeline-004 | Outcome-tracking dashboard (live/published + coverage per language) | code | medium | low | pr | batch-002, data-003 | Maintainer |
| opa-maint-001 | Adapter/policy maintenance vs host API & bot-policy changes | maintenance | medium | low | pr | adapter-001, adapter-002 | Maintainer |
| opa-doc-003 | Consent-registry retention + withdrawal-handling runbook | writing | small | medium | document | consent-001, pipeline-001 | Consent steward |

**M3 Definition of Done:** ≥ 2 additional languages onboarded with confirmed channels / CARE
agreements; dashboard maintained; maintainer + reviewer rotation documented; consent retention +
withdrawal runbook live; sustainability plan in effect.

---

## Backlog / future (sized, unscheduled)

| ID | Title | Type | Size | Risk | Deliverable | Notes |
|---|---|---|---|---|---|---|
| opa-adapter-004 | Additional source/host adapter (community dictionary platform) | code | large | medium | pr | New channel class; needs partner |
| opa-research-004 | Ethical-reuse / fairness note for TTS-ASR builders using the corpus | research | small | low | document | Expands the reuse-ethics note |
| opa-data-004 | Frequency/core-wordlist packs per language (drives batch selection) | data | medium | low | dataset | Wordlist provenance must be open |
| opa-pipeline-005 | Forced-alignment QA (audio↔IPA agreement automation, advisory only) | code | large | medium | pr | Assists, never replaces, human review |
| opa-link-001 | Cross-link with read-aloud-audio / decodable-readers (sibling projects) | design-spec | small | low | document | Out of current scope; high reuse value |
| opa-translate-001 | Glossary/label localization of the dataset UI/metadata | writing | medium | low | translation | Metadata only; not the words themselves |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 task. `verifiedNeed` is **`true`** here: a
published, openly-licensed **speaker consent & release framework** is a self-evidently necessary
foundational artifact and does not depend on an upstream channel or speaker community being
secured (unlike the `data-002`/`pr-001` tasks, which are `false` until M0's channel/community is
confirmed).

```json
{
  "id": "opa-consent-001",
  "title": "Author speaker consent & release framework v1",
  "project": "open-pronunciation-audio",
  "type": "writing",
  "lane": "donated",
  "priority": "high",
  "domain": ["language", "education", "open-culture", "privacy", "accessibility"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "open-pronunciation-audio records and curates word-level pronunciation audio paired with IPA across many languages. A human voice recording is identifying personal data, and an open license (CC0/CC-BY) is effectively irrevocable once distributed. Before any audio is recorded or curated, the project needs a single, versioned consent & release framework so that every recorded clip carries an informed, archived speaker release and every curated clip carries verified upstream consent provenance. This is the project's headline guardrail and gates all record/publish work.",
  "objective": "Write and publish version 1 of the speaker consent & release framework, including the plain-language release, the consent registry design, minors and pseudonymity handling, the ML-reuse and ethical-use disclosures, and the withdrawal/retention procedure.",
  "acceptanceCriteria": [
    "Provides a plain-language speaker release covering open license (CC0/CC-BY), worldwide redistribution and derivatives, that a voice recording is identifying personal data, and that an open license is effectively irrevocable (downstream copies cannot be universally recalled).",
    "Discloses that ML/speech-technology reuse is permitted (transparency, no per-use veto) and includes an ethical-use note prohibiting impersonation of identifiable individuals and deceptive synthetic-voice deepfakes.",
    "Offers a pseudonymity option and states the voice itself remains identifying; collects only minimal self-described speaker metadata (variety, region, L1/L2, age band).",
    "Requires verified guardian consent for minors and treats minors as out of scope unless a community partner explicitly governs it.",
    "Defines the consent registry (signed releases archived and keyed by consentRef, stored outside the open repo and never logged) and a documented withdrawal + retention procedure with downstream-limit disclosure.",
    "Published, versioned, and licensed CC-BY-4.0; wording flagged for review by a data-protection/open-licensing-competent person and framed as process guidance, not legal advice to speakers."
  ],
  "resources": [
    "C:\\code\\elyos\\planning\\projects\\open-pronunciation-audio\\PLAN.md",
    "C:\\code\\elyos\\CLAUDE.md",
    "C:\\code\\elyos\\docs\\good-deed-definition.md",
    "Creative Commons CC0-1.0 and CC-BY-4.0 license deeds",
    "CARE Principles for Indigenous Data Governance"
  ],
  "output": "A versioned consent framework document (consent/consent-framework.md) plus a reusable speaker-release template, CC-BY-4.0, covering informed open-license consent, irrevocability disclosure, ML-reuse and ethical-use notes, pseudonymity, minors, the consent registry, and withdrawal/retention.",
  "requestor": "jdev1977",
  "verifiedNeed": true,
  "outputLicense": "CC-BY-4.0"
}
```
