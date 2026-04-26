# CLAUDE.md — Steyn Group Pre-Read Portal

This file is read at the start of every Claude Code session. It captures the conventions, structure, source data, and decisions baked into this portal so future edits stay consistent. **Read this first before touching any HTML.**

---

## What this is

A working pre-read portal for the **Steyn Group board session on Tuesday, May 5, 2026 (1:00–4:45 PT)**. Built originally in Cowork mode, now under active iteration.

Audience: Steyn Group principals (board members), with Schuyler presenting the Uplifters JV section.

The portal is a static HTML site — no build step, no framework. Every section is a self-contained slide deck with its own `index.html`. Files open directly in a browser.

---

## File structure

```
.
├── index.html                   # Portal home — agenda + 4 section cards
├── CLAUDE.md                    # This file
├── spc-update/
│   └── index.html               # Section 01 · SPC Broad Update (6 slides)
├── lv-update/
│   ├── index.html               # Section 02 · LV Construction Update (5 slides)
│   └── assets/
│       ├── team/                # Team photos (lucy.jpg, brisa.jpg)
│       └── leads/               # Pipeline lead PDFs (5 files)
├── ai/
│   ├── index.html               # Section 03 · AI Discussion (6 slides)
│   ├── pay-apps-workflow.html   # Standalone Pay Apps current-state visual
│   ├── contracts-workflow.html  # Standalone Subcontractor Contracts visual
│   └── Jonah photo.jpg          # Jonah Dobkin headshot (note the space)
└── uplifters/
    ├── index.html               # Section 04 · Uplifters JV (9 slides)
    ├── Uplifters_IC_Memo.pdf    # Full IC memo + exhibits A–E
    ├── Uplifters_IC_Memo.docx   # Word version (kept but not linked from deck)
    └── assets/
        └── radcliffe-rendering.jpg + radcliffe-floor-1.jpg + radcliffe-floor-2.jpg
```

Cross-links between sections use relative paths (`../uplifters/`, `../spc-update/`, etc.). External links go to `cma-sixpeakcapital.github.io/SixPeak-Board-Portal/...` for the live models and Mission Control library, plus `hvndevelopment.com`, `cypressequity.com`, `lvllc.com`, and `workflowcapture.vercel.app`.

---

## Deck format (every section follows this pattern)

Every section page is a **single-file HTML slide deck** with the same shell. The shell contains:

- **Topbar** (large gradient header) with Back link, section label, h1 title, sub description, and a "Now showing · [slide name]" + progress bar + counter on the right
- **Sticky chip nav** (white bar) — one button per slide, click to jump
- **Stage** — only one `.slide` is `.active` at a time, the rest are `display:none`
- **Fixed footer controls** — crumb on the left, keyboard hint in the middle, Previous + Next buttons on the right
- **JavaScript at the bottom** — `goSlide(i)` function, `slideNames` array, keyboard listeners (←/→, PageUp/Down, Home/End)

To add a new slide:
1. Add a new `<section class="slide" id="slide-N">` block in the stage
2. Add a `<button data-slide="N">` in the chip nav
3. Add the slide name to the `slideNames` JS array
4. Update the topbar `counterTot` and the initial `progressFill` width
5. Update the footer crumb `01 / NN` count

---

## Brand kit (Six Peak / LV)

Pulled from the `sixpeak-brand-kit` skill. Use these tokens consistently.

### Colors (CSS variables, declared in every HTML's `<style>`)

```
--denim:        #0B1833   /* dark backgrounds, slide surfaces */
--space:        #2C3A51   /* secondary surfaces */
--space-light:  #384862
--space-muted:  #1E2C44
--steel:        #BFC7DA   /* body text on dark */
--steel-light:  #E2E7F0   /* page background (web) */
--ghost-warm:   #FAFAF6   /* light surfaces, text on dark */
--rust:         #C4581A   /* PRIMARY ACCENT — buttons, labels, stat numbers */
--rust-glow:    #E8935A   /* hover, gradient endpoints */
--rust-light:   rgba(196,88,26,0.12)
--gray:         #94A3B8
--gray-dk:      #475569
--success:      #10B981
--warning:      #F59E0B
--error:        #EF4444
```

### Typography

- **Primary (body, UI, data):** `'Aptos', 'DM Sans', Calibri, 'Segoe UI', sans-serif`
- **Display (titles, KPI numbers):** `'DM Sans', 'Aptos', 'Segoe UI', sans-serif`
- DM Sans is loaded from Google Fonts in every file's `<head>`. Aptos is local fallback.

### Component patterns (reuse, don't reinvent)

- **Section label:** `0.7rem`, 700 weight, uppercase, `0.25em` letter-spacing, rust color, with a 60px gradient divider underneath (rust → transparent)
- **KPI tile:** white card, dashed gray border *only* for placeholders, value in DM Sans `1.95rem` 700 weight (rust for hero metrics, denim for secondary)
- **Callout:** rust-tint background with 3px rust left border, 8px rounded right corners. `.callout.warn` variant uses warning amber
- **Card:** white bg, `1px solid rgba(11,24,51,0.08)` border, 12px radius, `0 4px 20px rgba(0,0,0,0.04)` shadow
- **Hero panel (dark gradient):** `linear-gradient(135deg, var(--denim) 0%, var(--space-muted) 100%)`, with rust gradient stripe at top, used for thesis statements and major callouts (HVN anchor on LV Slide 05, Jonah bio on AI Slide 04, Thesis hero on Uplifters Slide 01, etc.)
- **Placeholder block:** `.ph-block` — dashed slate border with "ADD CONTENT" tag chip. Used wherever Chris needs to drop in real content later

---

## Section-by-section content

### Section 01 · SPC Broad Update (`spc-update/`)

6 slides:

1. **Numbers at a Glance** — FY 2026 + FY 2027 KPIs from `SixPeak_BaseCase_v24.xlsx` Annual Summary. Two divider rows ("FY 2026" / "FY 2027") with KPI rows below each. **The FY 2027 section opens with a warning callout flagging that Reseda ($45.3M, scheduled to start April 2027) is not yet funded — it's roughly half the FY 2027 throughput.**
2. **Steyn Debt Facility** — $1.5M principal + $115K accrued interest = $1.62M Apr 2026 balance. 15% annual rate, $130K monthly payments starting Oct 2026, paid off Dec 2027 with a $96,780 final payment. Full month-by-month schedule table.
3. **LIHTC Portfolio** — Three LIHTC projects (Francis, 3rd Street, Reseda) combined: ~−$50K in-model NI (FY26–FY33), $6.58M post-model NI (post-2033), $6.53M lifetime. **Headline message: 99% of dev fee income lands post-2033.** Per-project table.
4. **Scenario Plan** — Side-by-side FY 2026–2031 tables for Base Case and Extended BD (numbers in $ millions). Below: 4-tile breakeven KPI panel (~$53M / $62M / $61M / $48M of construction billing required without Uplifters). **Extended BD has a callout below the table noting that bonding revenue + costs are assumed neutral from FY 2029+ (assumes LV stands up its own bonding capacity).**
5. **Strategy** — "Three pillars driving the next 12 months": (1) AI-native operations at LV, (2) LV expansion complete · stabilizing the roster, (3) BD plan led by Tom & Chris. Connective callout below explains how the three chain together: stabilize the team, fill it with work, scale it with AI.
6. **References** — Live links to board portal, Mission Control library, and the four model files (Base Case, Extended BD, Breakeven BD, Klump Scott). Uplifters models are NOT here — they live on the Uplifters slides.

**Source: `SixPeak_BaseCase_v24.xlsx` Annual Summary, LIHTC Project P&L, Assumptions, Projects, and Helpers sheets.** Also `SixPeak_ExtendedBD_v24.xlsx` and `SixPeak_BreakevenBD_v24.xlsx` for the scenario comparison.

### Section 02 · LV Construction Update (`lv-update/`)

5 slides:

1. **Team** — Three recent additions (Lucy Mohler · Assistant Project Manager, Brisa Sanchez · Project Coordinator, Steven Garcia · Project Superintendent). Lucy/Brisa cards expect photos at `assets/team/lucy.jpg` and `brisa.jpg` — graceful fallback if missing. Steven uses a person-icon avatar (no headshot on lvllc.com). Below: placeholder for projected hiring from the **4/27 call with Pedro**.
2. **Marketing & Website** — Live iframe embed of `lvllc.com` (with Open-in-new-tab fallback). Two narrative cards: "What's New on the Site & What's Coming" (developer-partner content, AI estimating tool v1, expansion path) and "Marketing Posture · Next 12 Months" (positioning as construction-led partner, mid-market multifamily target, AI estimating tool as top-of-funnel inbound). Closing callout ties the AI estimating tool into the AI investment thesis from Section 03.
3. **Project Buyout** — Where GMP risk lives. Five projects in active buyout (Crenshaw, Ramsgate, Califa, Whipple, Nelrose) — those that started construction April 2026. Column header is **"Remaining Contract"** (not total contract) — Chris to fill in actual remaining values. Three risk-context cards (Trades Most Exposed, Material/Pricing Watch, Mitigants in Flight) with placeholders.
4. **Pending Closings** — Four projects ordered by **construction start date**: Francis (7/1, Six Peak developer, on-grade 3-story podium + 5 stories Type IIIA, 232 units / 326 beds, $43.0M), Riverton (7/15, HVN, slab on grade + 5 IIIA, 80 units / 160 beds, **a.k.a. "Denny" on lvllc.com**), Lexington (8/15, HVN, slab + 5 IIIA, 67 units / 134 beds, $13.6M), Acama (9/15, HVN, slab + 5 IIIA + podium parking section, 131 units / 262 beds, $21.3M). Aggregate stats panel: ~$77.9M+, 510 units, 882 beds. **Two-part bonding callout (see "Bonding mechanics" below).**
5. **Extended BD** — HVN as anchor client (Tommy Beadel principal · 500–700 units in 26/27 acquisitions for 27/28 LIHTC rounds · LV #1 on internal GC Power Rankings). Five active leads: 338 Douglas (VDE · 373 units), Menlo (Cypress Equity · 195 units), 1275 W Sunset (VDE · 163 units), 42nd Street (Palmdale LIHTC · 160 units), 1415 Wilshire (Cypress Equity · 48 units). PDF deep-dives in `assets/leads/`. Aggregate: 939 units across 5 leads. Tie-in card names three anchor relationships (HVN, VDE, Cypress).

### Section 03 · LV · AI Discussion (`ai/`)

6 slides:

1. **Thesis** — "The constraint on scaling the GC isn't capital — it's manual workflow." Hero panel with three stats (2 of 4 phases mapped, ~12 workflow surfaces, Business Plan v5).
2. **Current State** — Two interactive walkthroughs: `pay-apps-workflow.html` and `contracts-workflow.html` (standalone HTML files Chris uploaded in the original Cowork session — they self-render).
3. **Workflow Capture** — Live iframe of `workflowcapture.vercel.app` (the bottoms-up team-wide inventory tool Grady sent on April 23). Three context cards (Ask / Goal / Where It Feeds), two cards explaining the capture format (Header Fields, Per-Step Detail), and a worked example pulled from Grady's email (the Subcontractor Change Request workflow).
4. **Jonah Demo** — Dark gradient bio panel at top with Jonah's photo (`Jonah photo.jpg` — note the space in the filename, URL-encoded as `Jonah%20photo.jpg`), name (Jonah Dobkin · Assistant Project Manager · LV / AI Mission Control Lead), and a bio placeholder. Below: two demo placeholder slots (embed + walkthrough notes), then the Engineering Bench section (placeholder for the engineers supporting Jonah).
5. **Phases & Library** — Consolidated. Top: four-phase strip (Preconstruction, Field Operations, Project Controls, Office & Admin) each linking to its Mission Control PDF. Bottom: full deep-dive library grid (Business Plan v5, Competitive Imperative, Financial Analysis, Hiring Plan, Job Descriptions, System Evaluation, Accounting Automation, Entity Valuation, QSBS Analysis, Teaser Deck, plus link to live library).
6. **Paths Forward (for discussion)** — Two parallel cards (NOT sequential, distinct bets):
   - **Path 1 · Internal margin lever** — modest internal investment, AI compresses LV's own back-office, same team carries 2× billing, ~25–40% margin expansion captured directly in LV's GC P&L
   - **Path 2 · External capital · AI-enabled GC platform** — substantial outside capital into a sister company, LV as anchor/proof customer, platform applied to acquired GCs in good markets seeing margin compression, **value creation rooted in operational margin improvement, not multiple expansion**
   - "How to read these together" panel below + two discussion-question cards at the bottom

### Section 04 · Uplifters Foundation JV (`uplifters/`)

9 slides. Schuyler presents this section. Authorization ask at the end.

1. **Thesis** — "First-of-its-kind tax-exempt bond structure to rebuild Pacific Palisades — and a first-mover platform for Six Peak." Hero stats: ~60 homes, $9.0M gross fees, $4.1M net to SPC, 5-year operating period FY 2027–2031.
2. **Opportunity** — Foundation overview, financing innovation, three replication paths (adjacent disaster markets, workforce/attainable housing, institutional adoption).
3. **Mandate** — Five-card scope grid: A. Program-Level, B. Acquisition, C. Project Mgmt, D. Sales & Exit, E. Post-Completion. Risk-profile callout (no equity, no completion guarantee).
4. **Economics** — Fee table (1% acquisition, 6% dev mgmt, 3% leasing, 1% disposition, ~$151K per home illustrative), gross-to-net flow ($9.0M → $4.9M overhead → $4.1M net), illustrative annual revenue chart (FY27 $0.7M → FY29 peak $2.6M → FY31 $0.7M).
5. **Sample Deal · 701 Radcliffe** — Real Westward Homes comparable ($1.2M acquisition, $1.6M total construction, ~$151K SPC capture). **Includes three architectural visuals from Exhibit E of the IC memo:** rendering at top, ground floor + second floor plans side-by-side below.
6. **Timeline** — Five-year operating window from FY 2026 mandate signing through FY 2031 wind-down.
7. **Risk & Status** — 50/50 probability gauge, two gating variables card (regulatory pathway + bond fundraising), execution risk grid (5 rows).
8. **Strategic Upside** — Three benefit cards (profile, capability match, first-mover) + framing on option value.
9. **Authorization & Attachments** — Recommendation in the dark CTA panel. Below: lettered list (A–E) describing the IC memo exhibits, then three downloads only (IC Memo PDF + 2 model files). Word version and individual exhibit page-jump links removed per Chris's earlier instruction.

---

## Source data — where numbers came from

- **`SixPeak_BaseCase_v24.xlsx`** (in the SixPeak-Board-Portal repo) — the canonical model. Annual Summary tab provides FY 2026–FY 2033 lines for Construction Billing, GC Revenue, GC Costs, Six Peak Revenue/Expenses, Consolidated NI, Bonding Outstanding, Cash positions. Projects tab provides per-project contract value, start month, duration, bonding tier. Assumptions tab provides bonding rates, interest rates, etc. LIHTC Project P&L tab provides the post-2033 framing.
- **`SixPeak_ExtendedBD_v24.xlsx`** — Extended BD scenario, used for the side-by-side comparison on SPC Slide 04.
- **`SixPeak_BreakevenBD_v24.xlsx`** — used for the breakeven construction billing KPI panel on SPC Slide 04.
- **`Uplifters_IC_Memo_Combined.docx`** — original IC memo Chris uploaded. Converted to PDF via LibreOffice and saved as `uplifters/Uplifters_IC_Memo.pdf`. All Uplifters numbers and exhibits trace to this memo.
- **`lvllc.com/team`** — Lucy / Brisa / Steven Garcia titles (Assistant Project Manager / Project Coordinator / Project Superintendent) and headshot references.
- **`lvllc.com/track-record`** — unit/bed counts for the four pending closings (Francis 232/326, Denny=Riverton 80/160, Lexington 67/134, Acama 131/262, sizes in SF where shown).
- **5 lead PDFs in `lv-update/assets/leads/`** — uploaded by Chris on April 26, 2026. Source of the 5 active BD leads on LV Slide 05.

---

## Key facts and calculations baked into the decks

### Bonding mechanics (LV Slide 04, two-part callout)

**Phase 1 — current state through these four closings:**
- LV charges projects **1.50%** in bonding revenue
- Surety cost is **0.74%** (Tier 1)
- Spread to LV: **0.76%** = real cash that funds Krueger's equity purchase at **$18M valuation**
- **FY 2026 dollar amount: ~$712K** ($1,405,538 revenue − $693,399 surety cost) — already included in the FY 2026 consolidated net income of $1.78M shown on SPC Slide 01
- Bonding the four projects pushes Krueger across his **10% bonding vesting threshold**

**Phase 2 — what changes after these four:**
- Surety steps to **Tier 2 at 3.25%** on the next round of bonded projects
- Compresses the 1.50% project charge against the new 3.25% cost
- Mitigant: LV stands up **its own bonding capacity** (eliminates third-party surety entirely) — this is the assumption baked into the SPC Extended BD plan from FY 2029+

### Steyn debt facility (SPC Slide 02)

- Principal: $1,500,000
- Accrued interest at Apr 2026: $115,485
- April 2026 starting balance: $1,615,485
- Annual rate: 15% (1.25% monthly, 3.75% quarterly)
- Monthly payment: $130,000 starting Oct 2026 (Month 7)
- 6 months interest-only accrual (Apr–Sep 2026) before payments begin
- 14 full payments + final payment of $96,780 in Dec 2027
- Total interest paid over life: ~$271K
- Total payments: ~$1,886,780

### LIHTC P&L (SPC Slide 03)

- Three projects: Francis, 3rd Street, Reseda
- Combined in-model NI (FY26–FY33): −$49,861 (essentially zero)
- Combined post-model NI (post-2033): $6,584,065
- Combined lifetime NI to SPC: $6,534,204
- **The headline message: 99% of net income lands after 2033** — these are deferred-income assets, not in-window contributors

### Reseda funding flag (SPC Slide 01, FY 2027 section)

- Reseda is a $45.3M contract scheduled to start April 2027 (in-model)
- **Not yet funded** — gating to the $87.4M FY 2027 construction billing peak
- If Reseda's financing slips, FY 2027 revenue, GC NI, and ending cash all step down materially

---

## External dependencies

- **`cma-sixpeakcapital.github.io/SixPeak-Board-Portal/`** — separate live portal hosting the Excel models and Mission Control PDFs. Multiple slides link out to this. If this URL changes, all the model/PDF links break.
- **`workflowcapture.vercel.app`** — embedded as iframe on AI Slide 03. Has Open-in-new-tab fallback if iframe is blocked.
- **`lvllc.com`** — embedded as iframe on LV Slide 02. Open-in-new-tab fallback present.
- **`hvndevelopment.com`** — linked from LV Slide 05.
- **`cypressequity.com`** — linked from LV Slide 05.
- **Google Fonts (DM Sans)** — every HTML loads this from `fonts.googleapis.com`. Site renders fine without it (Aptos / system sans fallbacks) but display weight will look different.

---

## Conventions to maintain

- **Use "pipeline", not "pipe"** — Chris flagged this explicitly. Audit before committing.
- **Numbers in $ millions** in scenario tables — the SPC Scenario Plan slide makes this explicit in the slide-cover lede. Don't mix M and dollar amounts in the same table.
- **Placeholder blocks** stay clearly marked with the "ADD CONTENT" tag chip. Don't silently delete them — that hides what's still pending.
- **No Uplifters content in SPC slides** — Uplifters has its own dedicated section; don't duplicate (Chris removed it from Strategy and References explicitly).
- **No homebuilder brand mention in LV slides** — Chris asked for this to stay out of LV (it's still in the Uplifters Brand Roadmap exhibit).
- **Cross-link references to other slides** use the format "Slide N · short name" (e.g., "Slide 05 · Phases & Library") — this matches the chip nav labels.
- **Cross-link references to other sections** use the format "Section NN" (e.g., "Section 01 of the SPC update") — this matches the agenda numbering.

---

## Common edits

- **Add a new slide:** see "Deck format" above. Update the chip nav, the `<section class="slide">`, the slideNames JS array, the topbar counter total, and the footer crumb.
- **Update KPI numbers:** find the `.kpi` block (look for the `.lbl` text). Numeric values are inline. After updating, also check the surrounding callout — many callouts cite the same numbers and need to stay in sync.
- **Add a placeholder content slot:** wrap in `<div class="ph-block">` with a `<span class="ph-tag">` chip. The CSS already handles styling.
- **Add a new project to the closings or buyout tables:** copy an existing `.closing` tile or `<tr>` and update fields. Make sure to update the aggregate stats panel below (totals, units, beds).
- **Refresh model numbers:** the canonical source is `SixPeak_BaseCase_v24.xlsx` Annual Summary. Read with `openpyxl` (Python) using `data_only=True` to get cached calculated values. Don't try to re-derive from formulas without recalculating — the sheet uses `data_only=True` cached values which may be stale relative to formulas.

---

## What's still placeholder vs. real

**Real (filled with actual data):**
- All SPC update KPIs, Steyn debt schedule, LIHTC P&L, scenario tables, breakeven figures
- LV Pending Closings — units, beds, sizes, dates, scope, developers, contract values (except Riverton contract)
- LV Marketing & Website — narrative is real, lvllc.com iframe is live
- LV Extended BD — HVN context, 5 leads with project specifics, aggregate stats, Cypress/VDE attribution
- AI thesis, current-state walkthroughs, workflow capture content, phases & library, paths forward
- Uplifters everything (sourced from IC memo)

**Placeholder (needs Chris to fill in):**
- LV Team Slide — Lucy and Brisa photos (`assets/team/lucy.jpg` + `brisa.jpg`); projected hiring detail from the 4/27 Pedro call
- LV Project Buyout — % bought out, $ remaining, variance, risk notes per project; trades exposed / material watch / mitigants
- LV Pending Closings — Riverton contract value
- AI Jonah Demo — Jonah's bio, demo embed/recording, walkthrough notes, engineering team detail

---

## Hosting context (for later)

The portal is currently a static HTML site living in a Dropbox folder. For production hosting, the path discussed is:

1. Move the folder out of Dropbox into a clean dev location (Dropbox + git fight each other)
2. `git init` and create a GitHub repo (`cma-sixpeakcapital/SixPeak-Steyn-Portal`)
3. Connect the repo to **Cloudflare Pages** for hosting (no build step — framework "None", output `/`)
4. Add custom domain (`steyn.sixpeakapps.com` or similar)
5. Enable **Cloudflare Access** with Google/Microsoft SSO restricted to Steyn principals + SPC team emails
6. Each `git push` auto-deploys

If considering moving to a dynamic stack later (Next.js on Vercel for CMS-style editing, per-user analytics, etc.), the current HTML structure ports cleanly into JSX.

---

## Last updated

This file was written at the close of the Cowork build phase, immediately before handing off to Claude Code. If you make significant structural changes (add/remove sections, change the deck shell, swap brand kit conventions), update this file so the next session inherits accurate context.
