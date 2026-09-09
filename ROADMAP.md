<!-- status-audit-2026-09-09 -->
> ## Status audit — 2026-09-09
>
> **This repository is still a scaffold:** 5 tracked files, no Hugo shell yet, no `content/` pages, no build output committed.
> The build plan below stands and is not superseded. This box records what the platform
> now requires that the plan predates, and what the first moves are.
>
> **Declared scope (from this plan):** CEFR A1 to C1 at launch-quality, with A1–A2 authored first and B2–C1 completing the arc. C2 is explicitly out of near-term scope.
>
> ### Platform requirements added since this plan was written
>
> - **`conformance.yml` at the repo root is mandatory** to declare any conformance level.
>   The framework (`curriculum` v1.1.0) defines `core` (A1–B1), `full` (A1–C1) and
>   `complete` (Pre-A1–C2). **No course in the organisation has this file yet** — not even
>   `efl`, which has the descriptor IDs in place. Absence of the manifest, not absence of
>   content, is what currently blocks every conformance claim in the suite.
> - **Descriptor IDs take the form `A1.REC.overall-oral-comprehension.01`.** Every
>   `implements_id` must resolve against `curriculum/levels/`; `conformance_audit.py
>   resolve` is the gate.
> - **Shortcodes, CSS tokens and layout chrome come from the shared `kit` Hugo module.**
>   A course repo holding its own copies is drift.
> - **CI is shared** and lives in `boulingua/.github`: `course-build.yml`,
>   `kit-drift.yml`, `link-check.yml`, `materials-latex.yml`, `org-audit.yml`.
> - **`[taxonomies]` is declared in no course `hugo.toml` today** — including the three
>   mature ones. Declare it here from the start rather than inheriting the omission.
>
> ### First moves, in this order
>
> 1. **Instantiate from `pagegen`.** Copy the template's `hugo.toml`, `go.mod` and CI entry point; do not copy layouts, shortcodes or CSS — those arrive from the shared `kit` module, and a local copy is drift that `kit-drift.yml` will flag.
> 2. **Write `conformance.yml` before the first unit.** Declare `framework: boulingua-curriculum`, `framework_version: 1.1.0`, the target from the scope above, and `conformance_status: in-progress`. Authoring units before the manifest exists is how `daf`, `efl` and `fle` each ended up with descriptor IDs in front matter that no machine can audit.
> 3. **Author unit 01 as a complete vertical slice** — page bundle, exam sibling bundle, slide deck, worksheet, audio and descriptor IDs — and only then scale out. One finished unit settles every convention question the remaining units would otherwise re-open.
>
> ### Where this sits in the suite
>
> Sequencing across all 18 courses, and the reasoning behind the order, live in
> [`boulingua/.github/ROADMAP.md`](https://github.com/boulingua/.github/blob/main/ROADMAP.md).
> The reference implementation to copy from is **`efl`** (180 unit bundles + 180 exam
> bundles, `unitNN-slug` naming, `page_type` and a `curriculum:` block on every unit).

---
# ELE — Spanish course roadmap

**Repository:** `boulingua/ele` (Español como Lengua Extranjera) · **Code:** `ele` · **Signature accent:** `#198D81` (light) / `#7EE7DC` (dark), pentagon mark · **Status:** scaffold → *coming soon*.

This roadmap turns the current empty scaffold (LICENSE + README + brand icons) into a full boulingua Spanish course — content **and** website — built on the `kit` platform, mapped to the `curriculum` framework, and shipped under the shared gate battery. It is written to be executed top-to-bottom.

---

## 1. Overview

**What it will be.** A free, openly-licensed Spanish course for the German *Gesamtschule* system and for independent learners worldwide, built on the boulingua five-step unit model (Activate → Input → Practise → Apply → Reflect). Each unit ships differentiated exercises with answer keys and teacher notes, committed slide decks + worksheets, native-voice audio, and cited public-domain / openly-licensed context.

**Target learners.** (a) German secondary learners taking Spanish as a second/third foreign language; (b) self-directed adult beginners and improvers, the largest single audience of any boulingua language. The course is organised by **CEFR level**, not by German school year (unlike EFL/FLE which use the Bildungsplan track), because the primary demand for Spanish is level-based self-study.

**Realistic CEFR scope.** A1 → C1 at launch-quality, with A1–A2 authored first and B2–C1 completing the arc. C2 is explicitly **out of near-term scope** (see §4) — freely-available material rarely reaches C2 and the curriculum framework expects that gap to be *declared, not hidden*.

**Defining challenges (from the language-specific context):**
1. **Variant is a launch decision, not a footnote.** European (Castilian) vs Latin-American Spanish touches vocabulary, *vosotros/ustedes*, *seseo/ceceo*, and the TTS voice. This must be decided **before** the first unit is written (§2).
2. **Huge, heterogeneous learner base.** Wider ability spread and more prior exposure than DaF/FLE; differentiation and a clean self-study path matter more than for the school-bound sister sites.
3. **Latin script, low onboarding cost — but not zero.** No new writing system, yet Spanish needs a genuine level-0 pass on the alphabet's Spanish values, `ñ`, digraphs, accents/tildes, `¿`/`¡`, and stress rules. This is a short onboarding stage, not a script course.
4. **Strong, licence-clear Piper voices available — use them as a first-class asset.** Spanish is the best-served language on the boulingua map for openly-licensed TTS, including a two-speaker model; audio should be a headline feature, including two-voice dialogues, which raises the bar for audio coverage and QA.
5. **Pronunciation regularity is a teaching advantage.** Spelling→sound is highly regular; the course should lean on that (explicit rules early) rather than treat pronunciation as incidental.

---

## 2. Language & localisation decisions

Each item states a concrete, opinionated recommendation.

- **Script / writing system.** Latin, no transliteration layer needed. **Decision: no special handling** beyond correct locale (`es`) collation and full support for `á é í ó ú ü ñ ¿ ¡`. Ensure editor + build treat files as UTF-8 (already the platform standard).
- **Variant / dialect.** **Recommendation: European (Castilian) Spanish as the taught norm**, with a standing "Latin-American variation" callout box wherever it is pedagogically useful (vocabulary pairs, *vosotros* vs *ustedes*, *vale/¿mande?*, `vos` note at B1+). Rationale: the German school audience is taught Peninsular Spanish; a single consistent norm keeps audio, keys, and exams coherent. LatAm is surfaced as *awareness*, never as a competing answer key. This matches the existing README ("Standard (Castilian) Spanish, with attention to Latin-American variation").
- **RTL.** Not applicable.
- **Web fonts / script coverage.** No extra coverage needed — the theme's Latin fonts and the kit's bundled Source Sans 3 / JetBrains Mono already cover all Spanish glyphs and punctuation. **Decision: use the shared font stack unchanged**; add a rendering check for `¿¡ñ` + combining accents to the deck/worksheet QA.
- **Native-voice / Piper TTS.** **Decision: `es_ES-davefx-medium` (CC0) as the primary voice, `es_ES-sharvard-medium` (CC BY 3.0) where a dialogue needs two speakers.** Both licences are on the org allow-list, so Spanish is unconstrained where most boulingua languages are not. `davefx` is CC0 and carries no obligation at all; `sharvard` costs one attribution row in `NOTICE.md`, and buys something the rest of the map does not have — it is a **two-speaker model**, so alternating dialogue voices come from a single `.onnx` rather than needing a second download and a second QA pass. Audio is a launch feature, not a phase-2 nice-to-have.
- **Voice IDs are never hand-typed.** The IDs, tiers and licences above are read from `kit/audio/voices.yml`, which is the single source of truth and must never be hand-copied into prose; adding a voice means adding a row there, not editing a table or a download script by hand. A voice ID retyped from memory is how a 404 gets written into a build.
- **Level-0 onboarding stage.** **Recommendation: ship a short "El alfabeto y los sonidos" onboarding bundle** (2–3 units) before A1 proper: alphabet with Spanish sound values, `ñ` and digraphs (`ch`, `ll`, `rr`), written accents + stress rules, `¿?`/`¡!`, and greetings. Modelled on the pre-A1 tier in `curriculum/levels/pre-a1.md`; it seeds pronunciation habits the regular orthography then reinforces.

---

## 3. Instantiation from the kit

Stand up the buildable site first; author content into it after. Nothing shared is copied — `ele` imports it.

1. **Create the repo around the kit.** `boulingua/ele` holds a short `hugo.toml` — baseURL, title, languageCode, `[params]`, menus, and a `[module]` block with `[[module.imports]] path = "github.com/boulingua/kit"` — plus `go.mod`/`go.sum` requiring `github.com/boulingua/kit` at the pinned tag (`v1.0.0`), `boulingua.yml` (the per-course config the gate battery reads), the twelve-line `.github/workflows/deploy.yml` that calls the org's reusable workflow, an empty `content/` skeleton, the three legal pages + `about/`, `lychee.toml`, `.gitattributes`, `.gitignore`, `.nojekyll`. `layouts/`, `assets/`, `i18n/`, `archetypes/` and `scripts/` are **not** copied and must never appear in this repo: they are the drift surface, and a file that is not here cannot fork. Hugo resolves the first four from the module; CI checks the kit out at the pinned tag so `scripts/` and the gate battery are on disk before Hugo runs. Keep `public/` untracked.
2. **Vendor `_materials/`.** `kit materials sync` assembles the kit's `latex/`, `fonts/` and `brand/icons/` into the flat `_materials/` tree the LaTeX build expects; commit the result. It is the only vendored surface — XeLaTeX cannot read a Hugo module — and every file in it is checked against the digests in `kit.lock`, so a local edit fails the build rather than forking silently. (Earlier drafts said to copy `_materials/` from the template. `pagegen` never had one.)
3. **Edit only the marked `hugo.toml` values:**
   - `baseURL = "https://boulingua.github.io/ele/"`
   - `title = "Español (ELE) — S. Le Boulanger"`
   - `languageCode = "es"` · `defaultContentLanguage = "es"`
   - `[params].navTitle = "Español"` · `description` (Spanish course blurb) · `keywords = "Spanish,español,ELE,CEFR,curriculum,OER,Castilian"`
   - `[params].code = "ele"` — selects the accent from the kit's `data/accents.yaml`; do **not** touch CSS. This one is load-bearing: the kit's default is the neutral graphite `template` accent, so a course that leaves it unset looks obviously unconfigured instead of looking like DaF.
   - `[params.plausible].domain = "boulingua.github.io/ele"` — keep this sub-table **last** under `[params]`; TOML scopes every key after a header to that header, so opening it early silently moves the bare `[params]` keys under `params.plausible` and leaves the site unstyled.
   - `[[params.social]].url = "https://github.com/boulingua/ele"`
   - `[[menu.main]]` sections → mirror the real content sections: `onboarding`, `level-a1 … level-c1`, `materials`, `about`, `legal`.
4. **Confirm the accent + regenerate the icon.** The kit's `data/accents.yaml` already carries `code: ele → #198D81 / hover #12665D` and arrives with the module — verify, then run the kit's `brand/make_icon.py` to regenerate this repo's pentagon + favicons from the accent. Confirm `brand/icon.svg`/`icon.png` match the README's stated colours.
5. **Fill legal placeholders.** Replace every `⟨…⟩` in `impressum.md` / `datenschutz.md` / `haftungsausschluss.md` (incl. the VG Wort METIS disclosure clause) so `verify_legal_placeholders.py` passes.
6. **First green build + Pages.** `hugo --panicOnWarning` builds clean; run the gate battery locally through `bin/kit`; enable GitHub Pages via `deploy.yml`; confirm the deployed placeholder site renders with the teal accent and pentagon.

**Definition of "scaffold done":** site builds, deploys, is correctly branded and legally complete — still *coming soon* on the world map until §8's flip criteria are met.

---

## 4. Curriculum conformance target

**Framework:** `boulingua-curriculum` (pin `framework_version`). IDs follow `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}` per `docs/id-scheme.md`; scales come from `schema/scale-registry.yml`.

- **Target conformance level: `full` (A1–C1).** Realistic and honest for Spanish given the planned scope. `core` (A1–B1) is the MVP milestone on the way there; `complete` (Pre-A1–C2) is **not** targeted — C2 and most C-level mediation/plurilingual material is declared `no-official-descriptor` or an explicit gap, per `docs/conformance.md`'s "declare the gap" rule.
- **Scales implemented vs declared.** Implement every **in-scope** scale that has an official descriptor at each covered level across `REC`, `PROD`, `INT`, `MED`, `LING`, `SOC`, `PRAG`. Where the CV has no descriptor for a (scale, level) cell — common at A1 and across C1 mediation/plurilingual — record `no-official-descriptor` (never mint an ID). `PLUR` gets light coverage with honest gaps; `SIGN` is out of scope (no IDs).
- **Units → descriptor IDs.** Each unit's front-matter `curriculum` block uses `framework: cefr` with `cefr_level` and an `implements`-style list of resolvable descriptor IDs (the polymorphic block in `docs/front-matter-fields.md`). Exams reference the same IDs via their `curriculum` block.
- **Machine-readable scope/coverage manifest.** Publish a `conformance.yml` (shape per `curriculum/examples/de-a1/conformance.yml`): `framework`, `framework_version`, `language: es`, `declared_conformance: full`, and `realizations:` mapping each `implements_id` to its Spanish realization. Every ID **must** resolve. A per-level coverage table (implemented vs `no-official-descriptor`) makes the gaps auditable.
- **Gate.** The reusable workflow `boulingua/.github/.github/workflows/course-build.yml@v1` runs `python .curriculum/scripts/conformance_audit.py resolve --manifest conformance.yml --content content` — format, uniqueness, and resolution against the scale registry + level files, **for this repo's manifest**. `id-audit.sh` audits the framework's *own* level files and **cannot** validate this repo. Do not wire it here.

---

## 5. Content creation plan

Phased by CEFR level, five-step unit model, recurring cast/theme, exams as **first-class sibling bundles** (`…-exam/`). Effort tags: **S** ≈ ½–1 day, **M** ≈ 2–4 days, **L** ≈ a week+.

**Recurring cast & theme.** A small ensemble carried across all levels — e.g. *Lucía* (Madrid student), *Mateo* (barista), *Sr. Reyes* (teacher), *Nadia* (exchange learner as the reader's proxy). Settings spiral: barrio → ciudad → work/travel → media/opinion → abstract argument. This gives continuity, reuses vocabulary, and anchors audio dialogues.

**Per-phase plan:**

| Phase | Content | Units | Effort | Acceptance criteria |
|---|---|---|---|---|
| **0 · Onboarding** | *El alfabeto y los sonidos*: alphabet/sound values, `ñ`+digraphs, accents/stress, `¿?¡!`, greetings | 2–3 | **M** | Builds; audio for every sound; pronunciation rules stated; VG Wort on any page ≥1800 chars |
| **A1** | Greetings, self/family, present tense, *ser/estar/hay*, gender+articles, numbers/time, food, city | 12 | **L** | Every A1 in-scope scale w/ descriptor implemented; answer keys + teacher notes; deck+worksheet+audio committed per unit; A1 exam bundle |
| **A2** | Past tenses (pretérito/imperfecto), routines, health, shopping, travel, comparatives, *por/para* | 12 | **L** | As A1; `core` conformance reachable after this phase (A1–B1) |
| **B1** | Subjunctive intro, future/conditional, opinions, narration, work, media | 14 | **L** | B1 scales implemented; mediation tasks appear; B1 exam |
| **B2** | Subjunctive mastery, argument, register, abstract topics, longer texts | 14 | **L** | B2 coverage; `full` on track |
| **C1** | Nuance, idiom, discourse markers, academic/professional genres | 12 | **L** | C1 in-scope scales; C1 exam; declares C2 + remaining C-level gaps |

**Exams.** One per level as a sibling `…-exam/` bundle with `page_type: exam`, `duration_min`, `total_points`, `notenschluessel`, a committed PDF under `static/downloads/<level>/`, mapped to the level's descriptor IDs.

**Appendices** (`content/appendices/<slug>/`): glossary (ES↔DE↔EN), common errors (German-L1 interference: false friends, *ser/estar*, gender), verb-conjugation reference, pronunciation/stress guide, writing rubrics, skills decision tree, teaching workflow — mirroring EFL's appendix set.

**Editorial pages:** `about/index.md`, references/sources, acknowledgements — each ≥1800 chars carries a VG Wort mark (§7).

---

## 6. Website & materials

- **Section landings via shortcodes.** Each `content/<level>/_index.md` is `page_type: section` and uses the shared shortcodes for its landing grid — never raw HTML (platform standard). Menu mirrors sections.
- **Materials pipeline (committed).** Generate decks and worksheets from the kit's LaTeX templates (`kit/latex/`, Beamer, XeLaTeX, bundled fonts, vendored into `_materials/`) in the boulingua design language; render via `scripts/build_materials_latex.py`. Commit `.pdf` (and editable source) under `static/materials/` + `static/downloads/`; CI only *verifies* (`verify_downloads.py`) — no TeX Live in the deploy path. Front-matter `presentation`/`worksheet` `{ file, thumbnail }` per unit.
- **Native-voice audio (Piper).** **Available and licence-clear for Spanish** — `es_ES-davefx-medium` (CC0) primary, `es_ES-sharvard-medium` (CC BY 3.0, two speakers in one model) for two-speaker dialogues (§2). Both rows live in `kit/audio/voices.yml`; the `.onnx` path passed to the build comes from that row and is never typed in. Run `python build_audio.py /path/to/ele <voice .onnx> es 'content/level-*/units/*/index.md'` — idempotent (re-synth only on text/voice change), output OGG/Opus. Cover vocabulary, dialogues, and reading texts from A1 onward; onboarding sounds get audio too. `sharvard`'s CC BY 3.0 needs an attribution row in `NOTICE.md` before its audio ships.
- **Thumbnails & downloads.** `scripts/render_thumbs.py` for deck/worksheet thumbnails; downloads hub lists per-level PDFs; `pdf_attribution.py` stamps CC BY-SA attribution.
- **Materials hub guard.** The `/materials/` hub is a navigation surface — **no** VG Wort pixel there (enforced by the hub guard, §7).

---

## 7. VG Wort — pixel assignment for ALL content pages (required, non-skippable)

Every original **creative text ≥1800 rendered characters** gets exactly one VG Wort Zählmarke, per `kit/docs/vgwort-standard.md`. This is binding and applies to **every** unit, exam, appendix, and editorial page (about/references/acknowledgements). It does **not** apply to navigation surfaces (home, `/materials/`, tag/category indexes, `/page/N/`) or the **templated legal pages** (Impressum/Datenschutz/Haftungsausschluss).

**Procedure per work:**
1. Draw a **fresh public code** (32-hex "Öffentlicher Identifikationscode") from the author's **T.O.M.** account — one per work, never reused, never inventing codes, never exposing the private code.
2. Register it in `data/vgwort.yaml` matched by `url:` (base-stripped `RelPermalink`) or `path:` (`content/<File.Path>`), with `pixel_url: https://vg09.met.vgwort.de/na/<code>`, `min_chars: 1800`, `author`, `registered_at`.
3. Render via the **single shared resolver** `layouts/_partials/vgwort/url.html` → `<head>` preload + eager `<body>` `<img>` (no JS, no consent gate, `loading="eager"`, off-screen not `display:none`).
4. Record each code in the **private usage registry** (kept outside the repo): `Used`, `Projekt=ele`, `Sprache=es`, `Niveau`, `Kurstitel`, `URL`, `Pixel_URL`.
5. Verify: coverage audit (`verify_vgwort_coverage.py`, warns on unmarked ≥1800-char pages) shows 0 unregistered editorial pages; render verify (`verify_rendered_pixels.py`, blocking) passes with one mark per URL; hub guard confirms `met.vgwort.de` absent from `/materials/`. Exclude `vgwort.de` from lychee.

**Estimated marks needed (full A1–C1):**

| Bucket | Pages |
|---|---|
| Onboarding units | ~3 |
| Units (A1 12 · A2 12 · B1 14 · B2 14 · C1 12) | 64 |
| Exam bundles (long-form prose ≥1800 chars) | ~5 |
| Appendices | ~7 |
| Editorial (about, references, acknowledgements) | ~3 |
| **Total marks to draw from T.O.M.** | **≈ 82** |

Draw codes per phase (not all at once): ~15 for the MVP (onboarding + A1 + A1 exam + core appendices/about), the rest as each level lands. Registration is asynchronous, so the coverage audit *warns* while codes are pending and the render gate *blocks* only registered ones.

---

## 8. Milestones & sequencing

1. **M0 — Scaffold live (S).** §3 complete: branded, legally complete, green build on Pages, still *coming soon*. Depends on: nothing.
2. **M1 — MVP: Onboarding + A1 live (L).** Onboarding bundle + 12 A1 units + A1 exam + core appendices + about; decks/worksheets/audio committed; A1 descriptor IDs resolve under the §4 gate; ~15 VG Wort marks registered. Depends on: M0, the `ele` voice row in `kit/audio/voices.yml`, the kit's LaTeX templates.
3. **M2 — A2 complete → `core` conformance (A1–B1 requires B1) (L).** A2 units+exam; conformance manifest updated.
4. **M3 — B1 complete → declare `core` (A1–B1) (L).** The §4 gate green across A1–B1; publish `declared_conformance: core`.
5. **M4 — B2 complete (L).**
6. **M5 — C1 complete → declare `full` (A1–C1) (L).** All ~82 marks registered; full coverage table with C2/other gaps declared as `no-official-descriptor`.

**Definition of done / ready to flip from coming-soon to active on the world map:** the MVP (M1) is the minimum bar — a coherent, self-contained **A1 (incl. onboarding)** that (a) builds and deploys green, (b) passes the **full gate battery** (VG Wort coverage + render + hub guard, downloads, legal placeholders, attribution, link-check), (c) has **every A1 in-scope descriptor ID resolving** under the §4 gate with a published `conformance.yml`, (d) ships **committed decks, worksheets, and native audio** for every A1 unit, and (e) has its VG Wort marks registered and rendering. At that point the ele node flips from *coming soon* to *active*; later levels extend a live course rather than gate the launch.

---

## 9. Open decisions & risks (language-specific)

- **Variant lock-in (decision).** Confirm **Castilian as taught norm** before authoring; changing it later invalidates audio, keys, and exams. LatAm stays *awareness-only* — resist scope-creep into a second answer key.
- **Second Piper voice quality (risk, now smaller).** Two-voice dialogues need two acceptable Peninsular voices, and `es_ES-sharvard-medium` supplies both from one two-speaker model — the audition is one model, not two. If neither of its speakers is good enough, fall back to single-voice dialogues with speaker labels rather than mixing an `es_MX` voice into a Castilian course; the fallback ladder never crosses into another language, and for a variant it never crosses the taught norm.
- **German-L1 interference (risk).** *ser/estar*, gender, false friends (*embarazada*, *éxito*, *ropa*) and the subjunctive are predictable failure points — front-load them in the common-errors appendix and unit reflect stages.
- **Vosotros vs ustedes (decision).** Teach *vosotros* actively (Castilian), flag *ustedes*-only usage in LatAm callouts; keep exam keys *vosotros*-consistent.
- **Large audience, wide spread (risk).** More ability variance than the school-bound sister sites — invest early in differentiation tiers and a clear self-study path; the onboarding stage is the on-ramp that manages it.
- **C-level material scarcity (accepted gap).** B2–C1 openly-licensed source texts are harder to source cleanly; budget extra sourcing time and declare C2 + thin mediation/plurilingual cells honestly rather than padding.
