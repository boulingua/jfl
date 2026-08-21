# JFL — Japanese course roadmap

Build plan for `jfl`, the boulingua Japanese-as-a-foreign-language course, from
today's empty scaffold (LICENSE + README + `brand/`) to a live, curriculum-aligned
site that can flip from *coming-soon* to *active* on the boulingua world map.

Signature accent **`#867B18`** (light) / **`#E7DC7E`** (dark), pentagon mark.
Course code **`jfl`** — already registered in the kit's `data/accents.yaml`.

---

## 1. Overview

**What this is.** A free, openly-licensed Japanese course for the German
*Gesamtschule* system and for independent learners, built on the shared
boulingua `kit` and mapped to the boulingua CEFR curriculum framework. Site
metalanguage of instruction is **German** (legal pages, scaffolding, glosses);
the target language is **Standard (Tokyo) Japanese**.

**Realistic CEFR scope.** Ship **Pre-A1 (script onboarding) → A1 → A2 → B1**.
That aligns with what free OER can sustain for a non-Latin, high-orthographic-load
language and maps cleanly to **JLPT N5–N3**. B2+ is declared a gap, not attempted.

**Learners.** Absolute beginners (including school cohorts) who cannot yet read
kana; adult self-learners wanting a structured, honestly-levelled path to
lower-intermediate.

**Defining challenges (language-specific, drive every later section):**

1. **Three scripts, staged over levels.** Hiragana + katakana must be taught
   before A1 proper; kanji is introduced incrementally across A1–B1 (never
   "finished"). This forces a real **Pre-A1 script-onboarding stage**.
2. **Furigana rendering.** Kanji needs reading aids (`<ruby>`) that degrade
   gracefully, are searchable, and can be toggled per level — a template concern,
   not just content.
3. **JLPT ↔ CEFR crosswalk.** Japanese pedagogy speaks JLPT; the framework speaks
   CEFR. A published, defensible crosswalk is required so units carry both.
4. **Phonology beyond the segment.** Pitch accent and mora timing must be taught
   and notated — they map to `LING.phonological-control` and are not optional.
5. **CJK web font, and no shippable Japanese voice.** A self-hosted CJK font
   (weight/subset budget), and audio that **cannot ship**: the one Japanese voice
   upstream carries is licensed `CC BY-NC-SA 4.0` and cannot sit inside a
   CC BY-SA 4.0 work. `jfl` is transcript-only by decision, not by omission — see
   §2.

---

## 2. Language & localisation decisions

Each item is an opinionated recommendation, to be ratified before Phase 1.

- **Writing system.** Teach in the authentic order kana → kanji. **Recommend:**
  romaji is a *scaffold that is retired*, present only in the Pre-A1 stage and
  A1 vocabulary lists, never in reading passages. Everything from A1 unit 1 is
  written in kana/kanji with furigana.
- **Furigana.** **Recommend** an HTML `<ruby>` approach via a `{{< furi >}}`
  shortcode (`goldmark` already runs `unsafe = true`, so `<ruby>漢字<rt>かんじ</rt></ruby>`
  renders). Furigana density is a **level policy**: full furigana at A1, kanji
  above the current JLPT band only at A2/B1. A body-class toggle lets readers
  hide `rt`.
- **Variant / dialect.** Standard Japanese (*hyōjungo*, Tokyo). Regional dialects
  and Kansai-ben are out of scope except as a culture note.
- **Text direction.** **Horizontal (LTR) only.** Vertical writing (*tategaki*) is
  explicitly out of scope — no layout work spent on it.
- **Web font.** **Recommend** self-hosted **Noto Sans JP** (UI/body) + optional
  **Noto Serif JP** for reading passages, **subset** to the taught kana + kanji
  set per level to keep the payload sane (full JP fonts are multi-MB). `unicode-range`
  + `font-display: swap`, both emitted by the kit's font build (F3) rather than
  hand-written. The per-level subsetting is the one real deviation from the Latin
  font assumption and it is a kit capability, not a course file: a course repo has
  no `static/fonts/` and no `assets/css/`.
- **Pitch accent.** Notate with the standard overline + downstep (`ꜜ`) convention;
  provide a Pre-A1 appendix explaining it and reference it from vocabulary lists.
- **TTS / native voice. Decided: transcript-only, on licence grounds.** A
  Japanese voice *does* exist upstream — `ja_JA-hi_fi_captain-medium`, medium
  tier, **two speakers in one model**, which would have given dialogues genuine
  alternating turns. Note the locale tag: upstream writes **`ja_JA`**, not
  `ja_JP`. It still cannot be used. Its dataset is licensed
  **`CC BY-NC-SA 4.0`**, and a NonCommercial model's output cannot be
  redistributed inside a CC BY-SA 4.0 work at all. So the question is settled by
  **licence**, not by quality: the `espeak-ng` normalisation problem
  (kanji→reading, pitch accent) never gets to be the deciding factor. It is the
  only Japanese voice in the catalogue, so there is no second same-locale voice to
  fall back to, and a voice is never substituted from a neighbouring language.
  `jfl` therefore ships **transcript-only** at every level: the unit page renders
  its `<details>` transcript with the player slot empty. Revisit only if an
  openly-licensed Japanese voice appears upstream. Voice IDs are read from
  `kit/audio/voices.yml` and are never
  hand-typed — a transliterated ID is how a 404 gets written into a download
  script.
- **Level-0 stage.** A first-class **Pre-A1 "script onboarding"** section
  (`content/level-pre-a1/`) precedes A1 — see §5.

---

## 3. Instantiation from the kit

Stand the site up first; author content second. Nothing below copies code: the
shared surface is imported, not cloned. Steps:

1. **Create the repo around the kit import.** `jfl` gets a short `hugo.toml`
   (the marked values below plus `[module] [[module.imports]] path =
   "github.com/boulingua/kit"`), a `go.mod`/`go.sum` carrying `require
   github.com/boulingua/kit v1.0.0`, `boulingua.yml` (the only per-course config
   the gate battery reads), the twelve-line `.github/workflows/deploy.yml` taken
   verbatim from `kit/templates/deploy.yml`, an empty `content/` skeleton and the
   three legal pages. `layouts/`, `assets/`, `scripts/`, `i18n/` and
   `archetypes/` are **not** copied and must never appear in `jfl/` — they are
   the drift surface, and a file that is not in the repo cannot fork. Hugo
   resolves them from the module at the pinned tag; CI checks the same tag out
   for the gate battery, and `bin/kit` does it locally.
2. **Vendor `_materials/`.** `kit materials sync` assembles the `.sty` files,
   the fonts and the icon PDFs flat into `jfl/_materials/`, because XeLaTeX
   cannot read a Hugo module. It is the only vendored surface in the repo, every
   byte of it is digest-locked in `kit.lock`, and a hand edit fails the next
   `bin/kit check`. (The old instruction to copy `_materials/` "from the
   template" was never executable — `pagegen/_materials/` did not exist.)
3. **Edit the marked `hugo.toml` values (only these):**
   - `baseURL = "https://boulingua.github.io/jfl/"`
   - `title = "Japanese — S. Le Boulanger"`
   - `languageCode = "de"`, `defaultContentLanguage = "de"`
   - `[params].navTitle = "Japanese"`, `description`, `keywords`
     (`Japanese,Japanisch,CEFR,JLPT,hiragana,katakana,kanji,OER`)
   - `params.code = "jfl"` — this now carries more weight, not less: the kit
     ships the neutral graphite `template` accent, so a course that forgets its
     code renders obviously unfinished instead of quietly looking like DaF.
     Gate F7-C3 makes an unset code fatal.
   - `[params.plausible].domain = "boulingua.github.io/jfl"` — keep the block
     **last**, after every bare `[params]` key. TOML scopes everything after a
     header to that header, so a sub-table opened early silently swallows the
     keys above it.
   - `[[params.social]].url = "https://github.com/boulingua/jfl"`
   - `[[menu.main]]` — mirror the real sections: Pre-A1 (Script), A1, A2, B1,
     Materials, About, Legal.
4. **Regenerate the pentagon.** The kit's `data/accents.yaml` already carries the
   `jfl` row (accent `#867B18`, hover `#5F5811`); confirm it, then run the kit's
   `make_icon.py` to emit the pentagon + favicons for `jfl` into `brand/`, which
   is the one brand surface the course owns. The accent flows from `code` through
   the kit's CSS; there is **no** course CSS to edit, and the CJK face arrives
   with the kit font tier named by `script_tier` in `boulingua.yml` (§2) rather
   than as a hand-added `@font-face`.
5. **Fill legal placeholders.** Replace the `⟨…⟩` placeholders in `impressum.md`,
   `datenschutz.md`, `haftungsausschluss.md`; keep the VG Wort METIS disclosure in
   Datenschutz. The placeholder gate is blocking — there is no `|| true` to drop,
   because suppressions are prohibited org-wide.
6. **First green build.** `hugo --gc --minify --panicOnWarning` clean — a warning
   is now a failed build — then `bin/kit check`, which is the same gate battery CI
   runs (VG Wort coverage/render, legal placeholders, downloads, attribution,
   `_materials/` digests). Push to `main`, enable **GitHub Pages**, and confirm
   the deployed placeholder home renders with the correct accent and CJK font.

**Exit criterion for §3:** a green Pages deploy of an empty-but-correct `jfl`
site (nav, accent, pentagon, CJK font, legal pages, VG Wort plumbing live), with
no `layouts/`, `assets/`, `scripts/`, `i18n/` or `archetypes/` anywhere in the
repo.

---

## 4. Curriculum conformance target

- **Framework:** `boulingua-curriculum`, pinned `framework_version`.
- **Declared conformance level: `core` (A1–B1).** Honest and reachable for free
  Japanese OER. `full`/`complete` (B2–C2) are **declared gaps**, per
  `curriculum/docs/conformance.md` — declare, don't hide.
- **In-scope scales implemented vs. declared `no-official-descriptor`:**
  - **Implement** where a descriptor exists at A1/A2/B1: all of `REC.*`
    (oral/reading comprehension, announcements, audio-visual), `PROD.*`
    (oral/written production + strategies), `INT.*` (conversation, information
    exchange, correspondence, transactions, online), and `LING.*`
    (**`phonological-control` and `orthographic-control` carry the script/pitch
    load**), `SOC.sociolinguistic-appropriateness`, `PRAG.*` (fluency, coherence,
    flexibility).
  - **Declare `no-official-descriptor`** for CV cells the framework leaves empty
    at these levels (many `preA1`/`A1` cells), recorded per level file — a silent
    missing scale is the only real failure.
  - **Declare partial/gap:** most `MED.*` and all `PLUR.*` at A1–B1 (thin in free
    material); implement the mediation scales that naturally fit (e.g.
    `MED.relaying-specific-information`, `MED.note-taking` at B1), gap the rest.
    `SIGN.*` is out of scope (no IDs), as in the registry.
- **Units → descriptor IDs.** Every unit's `curriculum.cefr_can_do` maps to one or
  more `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}` IDs from `curriculum/levels/`. Never mint
  IDs locally; reference existing ones (add to the framework upstream if a needed
  statement is missing).
- **Machine-readable scope manifest.** Publish `conformance.yml` at the **repo
  root** — not under `curriculum/`; all eighteen courses put it in the same place
  (mirroring `examples/de-a1/conformance.yml`): `language: ja`, `level`
  coverage, `declared_conformance`, and `realizations` mapping each unit's
  can-dos to `implements_id`s.
  **Gate.** The reusable workflow
  `boulingua/.github/.github/workflows/course-build.yml@v1` runs
  `python .curriculum/scripts/conformance_audit.py resolve --manifest conformance.yml --content content`.
  `id-audit.sh` audits the framework's *own* level files and **cannot** validate
  this repo. Do not wire it here.
- **JLPT crosswalk (appendix + front-matter).** N5≈A1, N4≈A2, N4–N3≈A2/B1,
  N3≈B1 (N2≈B2, N1≈C1 documented but out of course scope). Each unit carries both
  its CEFR IDs and a JLPT tag; the crosswalk rationale lives in an appendix.

---

## 5. Content creation plan

Per-level page bundles under `content/<level>/units/<unitNN-slug>/`; exams as
**first-class sibling bundles** (`…-exam/`) linked by `unit_nr`; section landings
via shortcodes only. Every unit follows the five-step model
(**Objectives → Input → Practise → Produce → Reflect**) from the archetype.

**Recurring cast & theme.** A small recurring cast anchors continuity across all
levels: a German exchange student in Tokyo, a host family, a classmate, a
konbini clerk. Themes spiral (self/family → school/daily life → town/travel →
work/opinions), so vocabulary and kanji recur in widening contexts.

| Phase | Stage | Units | Exams | Effort |
|---|---|---|---|---|
| 1 | **Pre-A1 — Script onboarding** (hiragana, katakana, first ~40 kanji, pitch/mora intro) | 5 | 1 | **L** |
| 2 | **A1 (JLPT N5)** | 12 | 1 | **L** |
| 3 | **A2 (JLPT N4)** | 12 | 1 | **L** |
| 4 | **B1 (JLPT N3)** | 12 | 1 | **L** |

- **Script stage (Phase 1):** hiragana set (gojūon + dakuten/handakuten +
  yōon), katakana set, reading fluency drills, first kanji (numbers, days,
  person/日/月/…), a mora/pitch primer. Romaji allowed here; retired after.
- **Appendices (built alongside, **M** each):** hiragana chart, katakana chart,
  kanji index (by level/JLPT), grammar reference, particle guide, pitch-accent
  guide, JLPT↔CEFR crosswalk, cumulative glossary, CEFR assessment grid.
- **Acceptance criteria per phase:**
  - Each unit: five-step structure complete; differentiated exercises **with
    answer keys**; teacher notes; furigana correct; CEFR IDs + JLPT tag in
    front-matter and resolving under the §4 gate; sources openly-licensed/PD and
    cited; committed `.odp` deck + PDF worksheet with thumbnails.
  - Each exam: sibling bundle, `duration_min` / `total_points` / `notenschluessel`
    set, PDF under `static/downloads/<level>/`.
  - Phase green build: coverage manifest updated, all gates pass, VG Wort marks
    assigned for every ≥1800-char page (§7).

---

## 6. Website & materials

- **Section landings** rendered by the template shortcodes (never raw HTML); each
  level `_index.md` is `page_type: section` listing its units + exam.
- **Materials pipeline.** Decks and worksheets generated locally from the branded
  LaTeX templates in `kit/latex/` (accent-driven), exported to
  committed `.odp` + PDF under `static/materials/` and `static/downloads/`;
  **CI only verifies** presence/attribution (no TeX Live in the deploy path).
  Japanese LaTeX needs a CJK-capable toolchain (LuaLaTeX + `luatexja` or XeLaTeX
  + Noto CJK) — a one-time local setup for the author.
- **Furigana in materials.** The deck/worksheet templates must render ruby too
  (LuaLaTeX `ruby` package) so print and web agree.
- **Native-voice audio: none, by decision (§2).** The kit's Piper pipeline
  (`kit/audio/build_audio.py`) is not run for `jfl`. The sole Japanese voice,
  `ja_JA-hi_fi_captain-medium`, is `CC BY-NC-SA 4.0` and cannot be redistributed
  inside CC BY-SA 4.0 content — and its two speakers, which would have carried
  alternating dialogue turns, go with it. Dialogues therefore mark turns
  typographically, and every listening-shaped task ships as a written transcript
  with the player slot empty. None of this is a quality judgement and no spike
  changes it. If an openly-licensed Japanese voice lands upstream it arrives as a
  row in `kit/audio/voices.yml` — the registry is the single source of truth for
  voice IDs, which are read from it and never hand-typed into prose.
- **Thumbnails** via `scripts/render_thumbs.py`; **downloads** verified by
  `scripts/verify_downloads.py`.

---

## 7. VG Wort — pixel assignment for ALL content pages

**Required and non-skippable.** Per `kit/docs/vgwort-standard.md`, **every**
content page that is original creative prose **≥ 1800 rendered characters** gets
exactly **one** VG Wort Zählmarke, on exactly one URL.

- **Which pages:** every unit, every exam, every appendix, and the editorial
  About / course-overview prose. **Never** the home page, the materials hub,
  tag/section indexes, paginated continuations, or the templated legal pages
  (Impressum/Datenschutz/Haftungsausschluss).
- **Mechanism (verbatim from the standard):** fresh **public** codes (32-hex)
  drawn per work from the author's **T.O.M.** account → registered in
  `data/vgwort.yaml` keyed by `url:`/`path:` → rendered through the single shared
  resolver (`layouts/_partials/vgwort/url.html`) as a `<head>` preload + eager
  `<body>` `<img>` → recorded in the **private** usage registry (kept outside the
  repo) → verified by CI: coverage audit (warning), render verify (blocking),
  hub guard (blocking). Never invent codes; never expose the private code; never
  reuse a code across pages.
- **Estimated total marks:** ≈ **41 units + 4 exams + ~9 appendices + ~2
  editorial = ~56 Zählmarken** (draw a small buffer, ~60). Codes are drawn in
  batches per phase as pages cross the 1800-char threshold; registration is
  asynchronous, so the coverage audit warns (never fails) on not-yet-registered
  long-form pages.

---

## 8. Milestones & sequencing

1. **M0 — Site up (Phase 3 above).** `jfl` stood up on the kit (§3), green
   Pages deploy, accent + pentagon + CJK font live, legal filled, VG Wort plumbing
   verified. *(dep: none)*
2. **M1 — Furigana + font spike.** `{{< furi >}}` shortcode, subset Noto JP fonts,
   ruby toggle, pitch-accent notation proven in web **and** LaTeX. *(dep: M0)*
3. **M2 — TTS position recorded (no spike needed).** The go/no-go is already
   answered on licence grounds (§2), so M2 shrinks to writing that rationale into
   the About page and the conformance notes: no Japanese audio ships, dialogue
   turns are marked typographically, transcripts stand in.
   *(dep: M0; parallel to M1)*
4. **M3 — MVP: Pre-A1 script stage live.** 5 script units + script exam +
   hiragana/katakana/pitch appendices, materials committed, marks assigned,
   repo-root `conformance.yml` green under the §4 gate. **First flippable milestone.**
   *(dep: M1)*
5. **M4 — A1 (N5) complete.** 12 units + exam + glossary/crosswalk appendices.
   *(dep: M3)*
6. **M5 — A2 (N4) complete.** *(dep: M4)*
7. **M6 — B1 (N3) complete + `core` conformance formally declared.** *(dep: M5)*

**Definition of "done / ready to flip from coming-soon to active":** M0–M4
achieved — i.e. the site is live and stable, the Pre-A1 script stage **and** the
full A1 level are published with materials, answer keys, thumbnails and
transcripts in place of audio (§2); every content page carries its Zählmarke and
passes the render gate; the repo-root `conformance.yml` manifest is green under
the §4 gate; all CI gates green. B1-complete `core` conformance (M6) is the subsequent target, not a
prerequisite for going active.

---

## 9. Open decisions & risks (language-specific)

- **Japanese audio is closed, not risky.** The single upstream voice,
  `ja_JA-hi_fi_captain-medium` (medium tier, two speakers), is `CC BY-NC-SA 4.0`
  and so unusable in a CC BY-SA 4.0 course; there is no second Japanese voice and
  no substitution across languages. *Consequence:* `jfl` is transcript-only, and
  this is no longer the course's top risk. What remains is **pedagogical**:
  listening-shaped `REC.*` work has to be carried by transcripts and by reading
  aloud in class, which the About page must say plainly so the declared coverage
  is not read as a claim about audio.
- **Font payload.** Full Noto JP is multi-MB. *Mitigation:* per-level `unicode-range`
  subsetting to the taught character set; `font-display: swap`.
- **Furigana correctness at scale.** Wrong readings mislead learners. *Mitigation:*
  furigana comes from vetted per-unit vocabulary lists, not auto-generation;
  reviewed against a dictionary.
- **JLPT↔CEFR crosswalk is approximate.** Bands don't align 1:1. *Mitigation:*
  publish the rationale as an appendix; treat JLPT as a tag, CEFR IDs as canonical.
- **Kanji sequencing.** No single canonical order for OER. *Mitigation:* sequence
  by JLPT band + frequency + theme recurrence; kanji index appendix makes it
  auditable.
- **CJK LaTeX toolchain.** The kit's LaTeX templates assume Latin. *Mitigation:* one-time
  LuaLaTeX + `luatexja`/Noto CJK setup, documented; CI stays verify-only.
- **Romaji creep.** Risk of learners leaning on romaji. *Mitigation:* romaji
  strictly confined to Pre-A1 and A1 vocab lists; reading passages kana/kanji only.
- **Mediation/plurilingual thinness.** *Mitigation:* declare the gap in the
  conformance manifest rather than fabricate coverage.
