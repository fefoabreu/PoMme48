# Handoff: PoMme 48° — UI Localization PM Platform (g48t)

## Overview
PoMme 48° is an internal operations platform for **UI Localization Project Managers**. It manages the end-to-end flow of shipping app UI strings into 48 languages / 150+ markets across Apple-services-style products, driven by an agentic AI engine called **g48t** (LangGraph orchestration + Claude tool-use agents, MCP connectors, RAG over a "golden dataset", evals, and human-in-the-loop governance).

The signature UX conceit: the whole app behaves like an **Apple TV / tvOS operating system**. The home screen is a full-screen "Hello" moment (a cycling multilingual greeting) with a **liquid-glass app dock** of 7 apps. Focusing an app previews it in the top third; selecting it "opens the app" (a section view); a Back affordance returns home. The aesthetic is **Apple liquid glass** (translucent blurred surfaces, hairline highlights) with a warm apple **red/green** accent pair used sparingly. Light mode is primary; there is a full dark mode; the g48t engine view is a dark "engine room".

`g48t` is a numeronym: **g11n + i18n + l10n + t9n** (globalization, internationalization, localization, translation), and 48 nods to the latitude of Paris (48.85°N) — a nod to the French "Pomme" (apple) wordmark.

## About the Design Files
The files in this bundle are **design references created in HTML** — a working prototype showing the intended look, motion, and behavior. **They are not production code to copy directly.** The task is to **recreate these designs in the target codebase's environment** using its established patterns, component library, and state management. If no environment exists yet, choose an appropriate stack (the prototype maps cleanly onto **React** + a CSS-in-JS or utility-CSS approach) and implement there.

The prototype is authored as a single "Design Component" (`.dc.html`) — a custom runtime that renders an inline-styled template driven by a `Component` logic class (React-class-like: `state`, `setState`, lifecycle, and a `renderVals()` method that returns the template's data + handlers). **Do not port the DC runtime.** Read it as: one big component tree + one state object. All styling is inline (React style objects); there are no external stylesheets. `PoMme 48.html` is the self-contained, offline, standalone build (open in any browser to explore).

## Fidelity
**High-fidelity (hifi).** Final colors, typography, spacing, motion, and interactions are all specified. Recreate the UI pixel-close using the codebase's libraries. The exact hex values, blur radii, radii, and animation timings below are authoritative.

---

## Global Layout & Shell

- **Root**: full-viewport (`100vh`), `overflow: hidden`. Ambient background is a set of layered radial gradients (warm red / green / gold at low alpha) over a base of `#f4f1ec` (light) — see Design Tokens.
- **Two shells depending on view:**
  1. **Home** = full-screen tvOS home (no chrome). Top-left wordmark block; center hero; bottom app dock; a Slack strip and Launch Horizon strip.
  2. **App views** (the other 6) = a top bar (title + subtitle, ⌘K search affordance, greeting, user avatar, and a menu/settings control cluster) with the scrollable content below. Navigation back to Home is via the wordmark / Home affordance.
- **No left-side navigation menu** — this was explicitly removed in favor of the tvOS dock. Do not reintroduce a sidebar.
- **View switching** is by a single `view` state string. App order (for the dock and ←/→ browsing): `['flow', 'board', 'detail', 'vendors', 'team', 'evals', 'arch']`. "flow" (AI Workflow) is intentionally first.

### Settings / control panel (macOS/tvOS style)
Triggered from a stacked-toggles control next to the user-initials bubble in the top bar. Reveals a **liquid-glass panel overlay** with:
- **Light / Dark mode** toggle (icon). Light is default on first land. Dark mode restyles every surface incl. Home and the engine view.
- **Language selector**: English, Français, Português (Brazilian), Español, 日本語, Deutsch, العربية, 中文, 한국어. (Selecting a language flips a `frenchTouch`-style copy layer for a few strings; full i18n of the UI is a stretch goal — the prototype only fully localizes a handful of hero/greeting strings.)
- **Focus Mode**: deactivates platform + Slack notifications (visually mutes the Slack strip / notification dots).

---

## Screens / Views

### 0. Home (tvOS home screen) — `view: 'home'`
- **Purpose**: an inspiring, non-work landing. "Every hello, everywhere." The PM starts their day here.
- **Layout** (top → bottom): 
  - **Top-left wordmark block** (`flex-shrink:0`, width fit-content): row 1 is the wordmark `PoMme 48°` (23px, weight 800, letter-spacing −0.4px, the `48°` in accent red `#e06a5e`) spread `space-between` against the subtitle "UI Localization PM Platform" (11.5px, weight 600, muted). Row 2, aligned to the same right edge: `g48t = g11n + i18n + l10n + t9n · latitude de Paris 48.85°N` (10px, faint).
  - **Center hero**: a large cycling multilingual **Hello** (`clamp(40px,10.5vh,76px)`, weight 600, letter-spacing −2px) with a locale caption beneath (12.5px, muted). Cycles every 2600ms through ~19 languages (Hello, Bonjour, Hola, こんにちは, 안녕하세요, 你好, Ciao, Hallo, Olá, مرحبا, नमस्ते, Привет, Merhaba, Hej, Xin chào, สวัสดี, שלום, Γειά σου, Cześć), each with fade/blur-in animation (`helloIn`, 2.6s). Tagline below (`clamp(11px,2.2vh,13.5px)`). A single **Start my day →** button (dark pill) that navigates **straight to Gate 1 · Intake & Triage** (`view:'flow'`, opens stage 0). (An earlier "Play the story" button and the "six gates, six agents…" line were removed.)
  - **Ambient layer**: faint oversized greeting words drifting in the background (low-alpha, `drift` animation) — intentional; ignore layout-overlap warnings for these.
  - **Slack strip**: a horizontal banner of latest Slack activity (channel + message previews) — how PMs interact most.
  - **Launch Horizon strip**: horizontal row of launch countdown chips (T-12d, T-20d…) with status dots (green on-track / amber watch). Header label "Launch Dates Horizon". (No "g48t back-plans every date" caption — self-explanatory.)
  - **App dock**: a **liquid-glass container** (`border-radius:28px`, heavy blur, hairline top highlight, soft drop shadow) holding a 6-column grid of app tiles. Each tile shows an icon glyph, a top badge with a pulsing status dot, a bold title, and a caption. **Focused tile scales in place** (`scale(1.09)`, centered — it must NOT translateY/rise). The 7th app (detail/Projects) is represented via the chip content. Tiles: AI Workflow, Pipeline board, Projects, Vendors, Team View, Evals, Backend (g48t engine).
- **Dock tile chips (exact content)**:
  - **AI Workflow**: badge "AI WORKFLOW" + pulsing dot; title "7 unassigned"; caption about Grab&Go triage.
  - **Pipeline board**: today-in-pipeline summary ("8 items in flight · 2 due this week").
  - **Projects**: bold title = latest project viewed (**WatchUWatchin+**); details beneath.
  - **Team View**: top badge "TEAM VIEW" + pulsing dot; bold title "22 PMs"; subtitle = active-project count.
  - **Vendors**, **Evals**, **Backend** (g48t engine — dark card, "handled by the Tools team", pulsing dot).

### 1. AI Workflow — `view: 'flow'`
- **Purpose**: the agentic heart. A **downward linear workflow** of 6 gates where each step's size grows with the number of items in it. Hover reveals detail; entering a gate opens a **deep sub-page** to work in, always with a **g48t companion** presence (a floating glass **bubble** panel pinned bottom-right that stays as you scroll, offering AI suggested-actions with canned replies).
- **6 Gates** (renamed per Apple-internal guidance — no "Tools" in gate names):
  1. **Intake & Triage** — the biggest AI moment (see Grab&Go below).
  2. **i18n Readiness** — pseudo-loc, RTL audits, loc-kit generation.
  3. **MTPE** — Machine Translation + Machine-Translation Post-Editing. Vendors pick up by language and return post-edited strings. Grounded by RAG + **golden dataset** (linguist-vetted strings; e.g. "64% golden-dataset match, vendors only post-edit the new 36%").
  4. **QA Validation** — screenshot-based validation using the same vendors (see QA below).
  5. **AIML Loop** — asks/needs routed to the AI/ML team; edit-distance/eval data exported to LM/DS; fine-tunes ship behind regression evals. (Expanded to feel realistic for an AI/ML audience.)
  6. **Release · C'est fini ✓** — GitHub package handoff + release (see Release below). g48t companion is a bubble here too.

#### Gate 1 · Grab&Go (Intake & Triage) — the headline AI feature
- **Left**: a "job board" **well** (dashed border) holding **7 unassigned request labels** — tilted cards (random rotation, `bob` float animation) tagged with **Platform** and originating **engineering team**, an id (`LOC-90xx`), title, and word count.
- Two ways to triage:
  - **Manual grab**: click a label → it expands to show project bins → pick a project.
  - **✦ Ask Siri** (gradient pill): AI pre-tags EVERY label with a suggested project + confidence (e.g. 0.96, 0.71). PM only clicks **Verify** per label, or **Verify all ≥ 0.90** to cascade.
- On assignment: the label animates **sucked into** its project scope on the right (`suckIn` keyframe), and the destination **bin pulses** (`binPulse`) and tallies words. When the board empties → "All requests triaged ✓" → **Build release plan →** (g48t back-plans each scope from its launch date and pre-stages the QA package).
- **Right**: project scope bins, each showing launch date + T-minus, and a running word count as labels land.

#### Gate 4 · QA Validation
- Three columns: **QA Plan → QA Run → QA Ready**.
- Clicking any package **in the Plan or Run lists opens its "UI Loc Scope"** — a full-screen glass carousel of the request labels (same tilted style as Grab&Go), swipeable. (This replaced an over-prominent "See UI Loc Scope" button — discoverable by clicking the item itself.)
- **QA Ready** packages (6 of them) each have **See QA PR →** → a full-screen **screenshot carousel** (see QA PR overlay below).

#### Gate 6 · Release
- Shows all **QA Ready packages** (6). PM **drags & drops** packages into a **GitHub** drop target to build the PR; as packages are added, the **PR statistics update live** (files, +additions, −deletions, vetted strings, locales). No "Sign release" button. The **Golden Dataset** info box sits at the bottom and reflects results. Final action → **notify eng** (GitHub package ready to grab for production).

### QA PR overlay (screenshot validation) — the demo centerpiece
Full-screen dark glass modal, centered via a flex wrapper (do not center with transform alone). Per project it shows a **3–5 screen carousel** of mock app screens (built from positioned solid blocks — NOT real screenshots) for a given platform/device. Each screen highlights the **implemented localized string in-context** with a colored highlight box + label ref, an EN → localized caption, the vendor, language chip, and PASS / FLAG status. Arrow keys ‹ › and dot navigation work; Esc closes. Projects with bespoke carousels: WatchUWatchin+, Wallet Order Tracking, Books Read-Aloud (#471), Apple Music Replay '26 (#484, incl. an RTL Arabic badge), Fitness+ Winter Programs (#490, incl. a flagged German truncation), Maps, TV Live Sports. Includes the signature **"Festa do Relógio" catch** narrative (the QE model catches "Watch Party" mistranslated as "a party of clocks" in pt-BR → corrected to "Assistir Juntos").

### 2. Projects (Project detail) — `view: 'detail'`
- **Purpose**: focus on one project. Title is a **dropdown project picker** (must render above the locale matrix — z-index the open menu to the front). Default selection: **WatchUWatchin+**.
- **Header**: project title dropdown; platform tags (iOS/watchOS/tvOS); meta (words · locales · date range · TM leverage · golden-dataset %); right side stats incl. overall %, open defects, **"on track"** + **launch date emphasis (T-minus)**, and the **Lead UI Loc PM initials** next to "on track". A **project screenshot** reference renders above the "Approved" chip on the right.
- **Gate progress**: a 6-cell row (done / active / upcoming states).
- **Locale matrix**: table of 48 languages × stages (MT, POST-EDIT, LQA/QA, SIGN-OFF) with status glyphs (✓ done / ● in-progress / · queued / ⚑ flagged) and a per-locale QE score column. pt-BR is the flagged row (QE 71.4).
- **Right rail**: a **HITL approval** card (working Approve → Approved state), a **live agent-activity feed** (ticks every 5s), and a **project Slack view** (latest from the project channel — agents surface bugs/defects and suggest what to prioritize toward launch date).

### 3. Vendors & TMS — `view: 'vendors'`
- **Purpose**: manage the LSP vendors that do MTPE (and re-used for QA validation).
- **Vendor cards** (4): Lingua Borealis (Nordics+CEE), TransAlpine (FIGS+Benelux), Kotoba Works (CJK), Meridian Loc (RTL+South Asia). Each: initials avatar, region, languages / active batches / on-time %, MT vs post-edit split bar, TMS sync status, MTD cost.
- **Handoff queue**: table of batches (BATCH id, PROJECT, VENDOR, WORDS, TM LEVERAGE, STATUS chip, DUE), auto-batched by locale group via MCP → TMS by the Vendor Dispatch Agent.

### 4. Team View — `view: 'team'`
- **Purpose**: collaboration & coverage. **22 PMs** reporting to a manager node (Marcus Delaney — see naming note). A **mind-map / network graph**: click a **PM** to see their projects; click a **project** to see its details. Includes an "+12 more PMs" cluster and platform groupings. Highlights coverage (picking up for someone on leave/vacation).

### 5. Evals & models — `view: 'evals'`
- **Purpose**: quality-estimation + the feedback loop into fine-tuning.
- **COMET trend** line chart (12 weeks, fleet avg ~92.3) with the **pt-BR dip-and-recovery** annotated ("regression caught" → "fine-tune shipped"). Loop stat cards (human edit rate 11.8% ↓2.1; edited segments → LM/DS). A numbered **Feedback Loop** (1–4). A **per-language model performance** table (COMET, edit rate bar, Δ quarter, segments, Healthy/Watch status).

### 6. Backend / g48t Engine — `view: 'arch'`
- **Dark "engine room"** (dark gradient bg even in light mode). **Architecture section renders FIRST**, evals/telemetry below.
- Header: "g48t engine · LangGraph orchestration · built & run by the Tools team". A **state machine** of the 6 gates with a **traveling pulse** dot gliding along the track (`glide` 8s). Below: the **6 Claude agents** (Intake, Scoping, Vendor Dispatch, LQA/QA Triage, Eval, Release Gate) each with tool-use chips (e.g. `mcp: tms.push`, `hf: qe_ensemble`, `rag: tm+glossary`). Then the **substrate**: MCP servers, RAG · Golden Dataset, Evals, Governance (HITL).

---

## Interactions & Behavior
- **View switch**: `view` state string; app dock focus + select; Home wordmark returns home. ←/→ browse dock order.
- **Home hello cycler**: `setInterval` 2600ms; each item keyed so the `helloIn` animation replays.
- **Agent feed**: `setInterval` 5000ms prepends a new entry (respects a live-feed toggle / Focus Mode).
- **Case-study / story auto-play** (legacy): optional 7s auto-advance.
- **Grab&Go**: click-to-expand assign; Ask Siri sets suggestions; Verify → `suckIn` (560ms) then item moves to `done`; bins `binPulse`. "Verify all ≥ 0.90" staggers assignments 180ms apart.
- **QA PR / UI Loc Scope overlays**: open/close, prev/next, dot nav, Esc, ArrowLeft/Right.
- **Release**: drag-and-drop packages → live PR-stat recalculation.
- **HITL approve**: button → approved state (green), audit-trail line.
- **Settings panel**: light/dark, language, focus mode.
- **Hover**: cards lift + shadow; dock tile scales in place; nav/list rows get a subtle bg wash.

### Key animations (keyframes)
- `pulseDot` (2s) — status dots. `ringPulse` (2s) — attention ring on pending approval. `feedIn` (0.4s) — feed entries. `glide` (8s linear) — engine pulse traveling the state machine. `cardIn` (0.35s) — board cards. `helloIn` (2.6s) — hero greeting fade/blur. `drift` (6–10s alt) — ambient background words. `bob` (~3.4s alt) — floating Grab&Go labels. `suckIn` (0.55s cubic-bezier(.55,-0.15,.85,.5)) — label pulled into scope. `binPulse` (0.7s) — scope bin receiving. `previewIn` (0.25–0.3s) — overlays.

## State Management
Single component state object. Key fields:
- `view` — current screen (`home|flow|board|detail|vendors|team|evals|arch`).
- `dark` (bool), language/`frenchTouch` (copy layer), `focusMode` (bool).
- `selName` — selected project for detail view; `projOpen` — project dropdown open.
- `flowStage` / stage index — which workflow gate is entered; `flowFocus` — hovered gate.
- `siriOn`, `ggState` (per-label assignment {proj, phase: leaving|done}), `ggOpen` — Grab&Go.
- `qaPrOpen`, `qaSlide` — QA PR carousel; scope-overlay open state.
- `approved` — HITL gate; `ghNotified` / release PR package selection + derived stats.
- `feed` — agent activity list; `helloIdx` — hero cycler; `teamSel` — team-map selection.
- `platFilter` / `lob` — Platform filter for board/workflow.

## Design Tokens

### Color
- **Ink / text**: `#1d1d1f`; secondary `#6e6e73`; muted `#8a8a8f`; faint `#9c9ca1` / `#b3b0ab`.
- **Base bg (light)**: `#f4f1ec` / `#f5f4f1`; card fills use translucent white (`rgba(255,255,255,.6–.78)`).
- **Accent red**: `#c93a2e` (primary), `#e06a5e` (lighter, used for `48°` and dark-mode accents).
- **Accent green**: `#2f7d4f` (primary), `#4bc978` (bright, engine dots).
- **Amber (watch/in-progress)**: `#b3720a` text / `#d9a53a` dot.
- **Platform tints**: Commerce `#d9a53a`, Media `#c93a2e`/`#c98fd9`, Fitness & Health `#4bc978`, Intelligence `#8f8ff2`, Maps & Services `#5ac8fa`/`#6aa9e8`, Books & Education `#c98fd9`/`#b8b8c2`.
- **Dark mode / engine**: bg near-black gradients over `#0c0c10`–`#141419`; text `#e8e8ea`/`#f2f2f4`; borders `rgba(255,255,255,.09–.16)`; muted `#85858e`/`#64646e`. Substrate tag colors: MCP `#e06a5e`, RAG `#8f8ff2`, Evals `#4bc978`, Governance `#d9a53a`.
- Status chip fills are the accent at ~12–18% alpha (e.g. green `rgba(47,125,79,.12)`, red `rgba(201,58,46,.12)`, amber `rgba(179,114,10,.12)`).

### Liquid glass (recreate carefully)
- Standard glass surface: `background: rgba(255,255,255,.6)`, `backdrop-filter: blur(20–26px) saturate(1.6–1.8)` (+ `-webkit-` prefix), `border: 1px solid rgba(255,255,255,.85)`, `box-shadow: 0 2px 10px rgba(29,29,31,.05), inset 0 1px 0 rgba(255,255,255,.9)`.
- tvOS dock: `border-radius: 28px`, `blur(32px) saturate(1.8)`, layered inset highlights.
- Dark glass (engine/overlays): `rgba(18,18,26,.9)` / `rgba(26,26,36,.5)`, `blur(22–36px)`, `border rgba(255,255,255,.09–.16)`, `inset 0 1px 0 rgba(255,255,255,.06–.12)`.

### Typography
- Font stack: `-apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", Helvetica, Arial, sans-serif` (monospace for ids/logs: `ui-monospace, Menlo, monospace`).
- Scale (px): hero `clamp(40,10.5vh,76)`; wordmark 23/800; view title 15–19/700; section head 12–13/700–800 (letter-spacing .3–1px for all-caps labels); body 11.5–13.5/600; captions 10–11/faint. Negative letter-spacing on large headings (−0.2 to −2px).

### Spacing / radius / shadow
- Radii: chips/badges 5–8px; cards 11–16px; large panels/dock 20–28px; pills/dots 999px/50%.
- Card padding 12–20px; grid `gap` 10–16px. Use flex/grid + `gap` (not margins) for sibling groups.
- Shadows: resting `0 1–2px 6–12px rgba(29,29,31,.05)`; hover `0 6–10px 18–26px rgba(29,29,31,.10–.11)`; overlays `0 30px 80px rgba(0,0,0,.6)`.

## Assets
- **No external image/font assets.** All icons are inline SVG; all "screenshots" are CSS block compositions; fonts are the system stack. Nothing to license or fetch. The standalone build inlines everything.

## Naming note (IMPORTANT)
Two variants exist:
- **`PoMme 48 v3.dc.html`** (in this bundle) is the internal/demo version with real names (manager **Adrian Sentoyo**, PMs incl. Nathalia Maciel, Jeannette Tang, Cedric Bambou, Ethan Brum, etc.; lead PM **Alessia Faye**).
- The public/standalone build reshuffled all names to fiction **except Alessia Faye** (manager became Marcus Delaney). If this platform is actually built, confirm with the team which names (if any) are appropriate — treat all sample data (project names, PMs, metrics, Apple-services references) as **placeholder**.

## Files
- `PoMme 48 v3.dc.html` — the full source prototype (design reference). Read the `Component` class's `renderVals()` for all data + handlers, and the template markup for structure/styles.
- `PoMme 48.html` — the self-contained standalone build; open in a browser to explore the live design end-to-end.
