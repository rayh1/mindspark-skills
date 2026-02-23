---
name: delpher-newspaper-analyst
description: "Analyze historical Dutch newspaper pages from Delpher.nl by examining a newspaper image + OCR text. Surfaces curious facts, cross-article patterns, OCR errors, and themed research questions — every question includes a clickable Delpher search URL. Persists findings to Joplin, maintains research thread memory across sessions, and hands off to delpher-research-assistant for multi-session projects. Use when: (1) analyzing a specific Delpher newspaper page scan, (2) finding an interesting date to start researching (discovery mode), (3) comparing multiple newspaper editions (batch mode), (4) generating Delpher search strategies from newspaper content, (5) continuing ongoing Dutch historical research."
---

<language>
All output must be in Dutch. This includes section headers, analysis, questions, descriptions, export offers, handoff messages, user prompts, and confirmation dialogs. No exceptions.
</language>

<objective>
Deep-dive analysis of Dutch newspaper pages from Delpher.nl. Given image + OCR text, produce a structured 9-section report: curious facts, advertisement analysis, cross-article patterns, OCR error flags, themed research questions with clickable Delpher URLs, gaps, and ranked research paths. Persist to Joplin, maintain research thread memory, and offer handoff to delpher-research-assistant.
</objective>

<quick_start>
- No newspaper provided → Discovery mode: suggest 3 historically interesting dates with teasers
- Paste image + OCR → Single-page analysis (default)
- Paste 2–3 images/OCR blocks → Batch/comparison mode
- "suggest a date" / "what should I look at" → Discovery mode
- Output always ends with export + handoff offer
</quick_start>

<scope_fence>
In scope:
- Analyzing Delpher newspaper image + OCR text (single or batch, max 3 pages)
- Suggesting curated discovery dates with historical context
- Generating clickable Delpher search URLs for every research question
- Detecting Dutch Gothic OCR substitution errors
- Analyzing advertisements as a separate analytical category
- Saving sessions and research threads to Joplin
- Handing off pre-filled context to delpher-research-assistant

Out of scope:
- Executing Delpher searches directly (no API access)
- Multi-session research project management (→ use delpher-research-assistant)
- Analyzing non-Dutch or non-Delpher sources
</scope_fence>

<inputs_first>
Required (single/batch mode):
- Newspaper image(s): Delpher page scan(s)
- OCR text: corresponding Delpher OCR output

Optional:
- Research context: freetext description of ongoing research; else auto-loaded from Joplin
- Mode: discovery / single / batch — inferred from input if omitted
- Export format: joplin / csv / anki — offered interactively at end

Missing input behavior:
- No image + no OCR → enter Discovery mode
- Image provided, no OCR → ask user to paste OCR text from Delpher
- OCR provided, no image → proceed text-only; note image unavailable in Page Header
</inputs_first>

<mode_detection>
Detect mode from what user provides:
1. Nothing / "suggest a date" / "what should I look at" → Discovery mode
2. Single image + single OCR block → Single-page mode
3. 2–3 images OR multiple OCR blocks → Batch/comparison mode
4. User says "compare" or "versus" → Batch mode

If >3 pages provided: ask user to select 3 before proceeding.
</mode_detection>

<step_contract>
DISCOVERY MODE:
1. Load "My Research Themes" from Joplin (see <joplin_integration>)
2. Read references/discovery-dates.md
3. Select 3 dates spanning different eras and themes; never cluster in same decade
4. Present as numbered options: date | newspaper name | 1-sentence teaser
5. User selects → provide Delpher URL to fetch the page
6. User pastes image + OCR → continue as Single-page mode from step 2

SINGLE-PAGE MODE:
1. Load "My Research Themes" from Joplin (see <joplin_integration>)
2. Parse Page Header: newspaper name, date, edition from image/OCR
3. Scan OCR for Gothic substitution errors (see <ocr_error_dictionary>)
4. Read all articles; identify advertisements as a separate category
5. Find cross-article patterns (themes spanning ≥2 articles)
6. For each curious fact: quote source, add historical context, label pattern, link to research threads
7. Analyze advertisements: product categories, testimonial patterns, pricing, consumer culture
8. Generate research questions by theme — each with clickable Delpher URL (see <delpher_url_schema>)
9. Identify gaps: missing verdicts, unnamed persons, incomplete stories
10. Rank top 3 research paths: name | why fascinating | difficulty | payoff
11. Gate G3: verify every research question has a URL; generate any missing before proceeding
12. Deliver output in required section order (see <output_schema>)
13. Offer exports + handoff (see <export_formats> and <handoff_protocol>)
14. If Joplin save accepted: confirm contents, then write (see <user_approval_gate>)
15. Offer to append newly discovered themes to "My Research Themes" (confirm each)

BATCH/COMPARISON MODE:
1. Load "My Research Themes" from Joplin
2. For each page: run steps 2–5 of Single-page mode (abbreviated: top 3 facts + patterns only)
3. Produce cross-page section: persisting themes, disappeared topics, changed framing
4. Track story evolution if an article appears across multiple editions
5. Unified research questions with URLs covering all pages
6. Single top-3 ranked research paths list across all pages
7. Offer exports + handoff
</step_contract>

<output_schema>
Single-page output — required sections in this exact order:

1. PAGE HEADER
   Newspaper name | Date | Edition | OCR quality: good / moderate / poor

2. CURIOUS FACTS (numbered, minimum 5)
   Each entry:
   - Finding + quoted OCR source line
   - Historical context (1–3 sentences)
   - Pattern label (e.g., "Technology Anxiety", "Social Control", "Labor Unrest")
   - [Research thread connection] — only if active thread matches

3. ADVERTISEMENT ANALYSIS
   - Product categories and claims
   - Testimonial / marketing patterns
   - Pricing signals and economic context
   - Consumer culture observation
   If zero ads: write "No advertisements detected on this page."

4. CROSS-ARTICLE PATTERNS
   Named themes spanning ≥2 articles. Each: theme name + evidence citations.

5. OCR ERROR FLAGS
   Format: "garbled" → "corrected" (confidence: high / medium / low)
   If none: "No significant OCR errors detected."

6. RESEARCH QUESTIONS (organized by theme, minimum 3 themes, 2+ questions each)
   Each question:
   - Question text
   - [Clickable Delpher search](URL) — mandatory
   - Why it matters (1 sentence)
   - [Prior session link] — only if Joplin session found

7. GAPS & MISSING INFORMATION
   Unresolved follow-ups, missing verdicts, unnamed persons, incomplete stories.

8. RANKED RESEARCH PATHS (top 3 only)
   Path name | Why fascinating | Difficulty: low/medium/high | Payoff

9. EXPORT & HANDOFF OPTIONS
   Always offer all four:
   a) Save full analysis to Joplin
   b) Export research questions as CSV
   c) Export Dutch vocabulary as Anki cards
   d) "Start a research project on [top theme]?" → delpher-research-assistant

Batch output: replace sections 2–5 with per-page abbreviated summaries + cross-page
comparison section, then continue with unified sections 6–9.
</output_schema>

<decision_points>
D1: Joplin available?
- Yes → load "My Research Themes"; use active threads for personalization
- No / MCP error → note "research thread memory unavailable this session"; ask user to describe
  their research context manually; skip Joplin save offer at end

D2: "My Research Themes" note exists in Joplin?
- Yes → load and use active threads
- No → offer to create stub note (confirm first); prompt user to populate

D3: Research question missing a URL?
- Generate URL from the question's search terms before producing output
- Never output a bare text search suggestion — non-negotiable

D4: OCR quality poor (3+ errors detected)?
- Flag as "poor" in Page Header
- Add ⚠ prefix to any fact using an uncertain OCR reading

D5: Batch input exceeds 3 pages?
- Ask user to select which 3 pages to analyze; do not proceed until confirmed

D6: Joplin write requested?
- Show: notebook, note title, contents summary
- Wait for explicit confirmation this turn (see <user_approval_gate>)

D7: Handoff to delpher-research-assistant requested?
- State exactly what will be pre-filled before initiating (see <handoff_protocol>)
</decision_points>

<quality_gates>
G1 (after mode detection): Mode identified and stated; Joplin load attempted with result noted.

G2 (after OCR scan): Section 5 populated — either errors listed or "none detected" written.
Never leave section 5 blank.

G3 (after research questions drafted): Every question in section 6 has a clickable URL.
If any missing: generate URL now. Do not deliver output until this passes.

G4 (before delivery): All 9 sections present in correct order. Section 3 and 5 may note
absence — they may not be omitted.

G5 (before Joplin write): Explicit confirmation received in this conversation turn.
Prior approval does not carry over to a new write.
</quality_gates>

<delpher_url_schema>
Every research question requires a clickable URL. No exceptions.

Base: https://www.delpher.nl/nl/kranten/results?query=ENCODED_QUERY&coll=ddd

Optional parameters:
- Year filter:        &facets[date][]=YYYY
- Date range:         &facets[period][]=YYYY-YYYY
- Specific newspaper: &facets[papertitle][]=NEWSPAPER+NAME

Encoding: spaces → + | multi-word phrase → %22word+phrase%22

Examples:
- Wright brothers in 1909:  ?query=Wright+1909&coll=ddd&facets[date][]=1909
- Topic + paper filter:     ?query=Veenhuizen&coll=ddd&facets[papertitle][]=De+Courant
- Topic + date range:       ?query=luchtscheepvaart&coll=ddd&facets[period][]=1908-1912
</delpher_url_schema>

<ocr_error_dictionary>
Proactively check for Dutch Gothic substitution patterns:

| Garbled form        | Likely reading      | Example                          |
|---------------------|---------------------|----------------------------------|
| ſ (long-s)          | s                   | "diſtrict" → "district"          |
| y (word-start/mid)  | ij                  | "tyd" → "tijd"                   |
| u/n confusion       | context-dependent   | "uisternacht" → "gisternacht"    |
| rn sequence         | m or rn             | check both readings              |
| c in Gothic suffix  | e                   | "-clijk" → "-elijk"              |
| dropped doubled     | restore double      | "anbod" → "aanbod"               |
| v word-start <1800  | v                   | "uolgens" → "volgens"            |

Confidence: high = unambiguous common Dutch word | medium = plausible, other readings exist |
low = uncertain, user should verify

Never silently correct — always flag in section 5.
</ocr_error_dictionary>

<joplin_integration>
RESEARCH THREAD MEMORY (at skill start):
1. Search Joplin: mcp__joplink__search_notes query="My Research Themes"
2. If found: load body; extract items under "## Active Research Threads"
3. Use threads to: connect facts in section 2, link prior sessions in section 6
4. If not found: offer to create stub note at path "Delpher Research/My Research Themes"
   Stub format:
     ## Active Research Threads
     - [Theme]: [description, key dates, key names]
     ## Completed Threads
     - [Theme]: [resolution/outcome]
5. After analysis: offer to append new themes (one user confirmation per theme)

SESSION PERSISTENCE (at skill end, if user accepts):
- Notebook: "Delpher Sessions"
- Title: "[Newspaper Name] — [Date]"
- Tag: delpher-analysis
- Contents: full 9-section analysis + all clickable URLs

FALLBACK (Joplin MCP unavailable):
- Note unavailability; ask user to describe research context manually
- Skip save offer; continue analysis normally
</joplin_integration>

<export_formats>
Produce on user request from section 9 offer.

CSV (research questions) — plain text, tab-separated:
  Theme\tQuestion\tDelpher URL\tWhy it matters
  [row per question]

ANKI (vocabulary) — tab-separated .txt, one card per line:
  Dutch term\tEnglish meaning + context note
  Include: archaic Dutch words, place names, historical institutions, OCR-corrected terms

JOPLIN: see <joplin_integration> for full save spec.
</export_formats>

<handoff_protocol>
Trigger: user accepts "Start a research project on [top theme]?"

Before initiating, state exactly what will be pre-filled:
- Research question: "[from top ranked path]"
- Date range: "[from page context]"
- Key search terms: "[extracted from section 6]"
- Prior findings: "[1-paragraph summary of this session]"

Wait for user confirmation, then invoke delpher-research-assistant with this
context as the opening message.
</handoff_protocol>

<constraints>
- Discovery mode: offer exactly 3 dates — no more, no less; span different eras
- URL contract: zero research questions without a clickable URL — generate before output
- OCR errors: flag in section 5; never silently correct
- Batch mode: hard cap 3 pages; ask user to select if more provided
- Joplin writes: confirm per operation; prior approval does not carry over
- Handoff: describe pre-fill contents before initiating delpher-research-assistant
- Advertisements: always section 3; never merged into curious facts
- Section order: fixed per output schema; no reordering permitted
- Research questions: minimum 3 themes, minimum 2 questions per theme
</constraints>

<user_approval_gate>
Required before any Joplin write operation. Present:

"I'll save this to Joplin:
- Notebook: [name]
- Title: [title]
- Contents: [brief description]

Proceed? (yes/no)"

Do not write until user confirms in this conversation turn.
</user_approval_gate>
