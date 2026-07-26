# Tanda Miranda — Design System

## What is Tanda Miranda

Tanda Miranda is a Mexican **SOFOM** (Sociedad Financiera de Objeto Múltiple) that provides
**fair, easy credit to formal (payroll) workers in northern Mexico**, distinguished by a
commitment to financial equity and promoting a savings culture. It began as an internal
extension for the employees and salespeople of **Promessa** (an insurance business), then
grew into a standalone lender that uses technology both for risk scoring and to deliver its
services through digital platforms. Its stated vision: a world where Mexicans enjoy fairer,
easier, more inclusive interest rates. Marketing leans on milestone purchases ("¡Este mes
estrena auto!") and the name plays on the *tanda* / *vaquita* — a communal savings pool —
repositioned as "the only tanda where you're always No.1."

- **Mission:** provide fair, easy credit for Mexican workers.
- **Vision:** be present on Mexican workers' devices as a financial-education tool.
- **Values:** Innovación, Eficiencia, Comunidad, Transparencia, Previsión.
- **Target market:** formally-employed workers in northern Mexico with payroll income and
  demonstrated job stability.

Surfaces covered: the original **marketing/brand identity** (from the 2024 manual) plus a
**product-UI layer** (customer app/portal + back-office ops) added in v2 as an extension —
no product UI existed in the brand source; every product-layer decision is flagged as
derived, with the manual kept authoritative for everything it specifies.

## Sources

All brand material came from a single attached folder, **`Tanda Miranda 2024/`** (local,
not a public repo/Figma — reattach via the Import menu to regenerate):
- `Manual de Identidad y Guía de Uso/` — two brand PDFs, the **authoritative source**:
  colors (Pantone/CMYK/RGB/HEX), named type hierarchy, logo rules, voice, photography.
- `Logotipo y elementos/` — logo lockups + cow mascot illustration (svg/png/jpg).
- `Tipografías/` — Libre Baskerville (OFL) and Noto Sans (OFL) font files.
- `Materiales/` — website screenshot + PSD, social art, business cards, email signature,
  folder, letterhead (incl. a real credit-offer letter), presentation pptx + photography.
- `Presentaciones/` — three finished PDF decks (no blank template — see Caveats).

The **product-UI layer** (v2) follows a production-grade reference architecture supplied
by the user (three-tier tokens, ~50-component inventory, WCAG 2.2 AA, Mexico-first
fintech patterns: CAT disclosures, CLABE, SPEI receipts, maker-checker). Structure comes
from that reference; all values are brand decisions documented here.

## Architecture (v2)

**Three-tier tokens** — all in `tokens/`, imported by `styles.css`:

1. **Primitives** (`primitives.css`) — raw ramps, never consumed by product UI directly:
   `--tm-navy-*` (★900 = Azul Tanda), `--tm-red-*` (★600 = Rojo Miranda; 650 = AA-safe
   solid), `--tm-neutral-*` (navy-tinted), `--tm-green/amber/blue-*` (derived status
   hues), `--tm-alpha-navy/white-*`, `--tm-carmin` (pattern only).
2. **Semantic roles** (`colors.css`) — what components consume: `--color-bg-*`,
   `--color-fg-*`, `--color-border-*`, `--chart-*`. **Dark theme = re-map of this layer**
   via `[data-theme="dark"]` (page navy-1000, surface 950, raised = Azul Tanda ★900;
   every pair re-verified AA). Legacy aliases (`--color-text-primary`, …) preserved.
3. **Component tokens** (`modes.css`, sparing) — `--control-*`, `--button-*`, `--input-*`,
   `--table-*`, `--card-*`, re-mapped by two orthogonal modes:
   - **Surface registers:** default/`[data-surface="marketing"]` = pill controls,
     uppercase tracked labels, official red-600 CTAs, heights 36/44/52.
     `[data-surface="app"]` = md radius, sentence case, red-650 CTAs (white text passes
     4.5:1), heights 32/40/48. One attribute on `<body>` switches the register.
   - **Density:** `[data-density="compact"]` steps controls to 28/32/40 and table rows
     48→36 (ops consoles ship a switcher; customer surfaces stay comfortable).

Plus `typography.css` (manual hierarchy + UI scale + mono for identifiers +
`--numeric-tabular`), `spacing.css` (4px base, radius, borders, containers),
`elevation.css` (shadows ↔ z-scale, ONE global focus ring), `motion.css` (productive
register only), `fonts.css` (@font-face).

**Key decisions (user-confirmed):**
- **Rojo Miranda doubles as CTA and danger.** Consequence: status is NEVER color-alone —
  every status pairs icon + label (see StatusIndicator, Alert); danger *text* uses red-700
  (red-600 fails 4.5:1); app solid fills use red-650.
- **Radius register:** pills stay the marketing signature; product UI steps to md (10px).
- **Dark mode:** semantic re-map, brand-navy based (no pure black).
- English docs; UI copy es-MX (tú in product/marketing, usted in official letters only).

## Index

- `styles.css` — imports all tokens + base rules (global `:focus-visible` ring, link
  colors, `prefers-reduced-motion`).
- `tokens/` — `fonts` · `primitives` · `colors` · `typography` · `spacing` · `elevation` ·
  `motion` · `modes`.
- `guidelines/` — specimen cards (Design System tab): brand set (logo, mascot, pattern,
  photography, voice), color set (official, ramps, semantic+pairing matrix, dark), type,
  spacing, plus v2 foundations (elevation/z/focus, motion, states matrix,
  registers & density) and patterns (forms & validation, financial data display,
  accessibility gates, tone map).
- `assets/` — `logo/` (all lockups), `illustrations/` (vaquita + pattern), `photography/`,
  `patterns/`, `reference/`, `fonts/`.
- `components/` — `core/`, `actions/`, `forms/`, `navigation/`, `feedback/`, `overlays/`,
  `data/`, `domain/` (each with a demo card).
- `showcase/` + `Component Showcase.html` — single interactive page with all 84
  components live, grouped in 8 sections with a sticky SideNav, theme (light/dark) and
  density switchers in the AppBar. Also a "Home" card in the Design System tab.
- `ui_kits/website/` — marketing homepage recreation.
- `ui_kits/app/` — **pilot**: customer loan dashboard (app register, light/dark, payment
  flow with review → OTP → receipt).
- `ui_kits/ops/` — **pilot**: back-office ops console (compact density + switcher,
  filter chips, bulk bar, virtualized DataGrid, detail drawer, maker-checker, ⌘K).
- `templates/brand-deck/` — comprehensive 17-slide 16:9 presentation template (Design
  Component): portada, agenda, dividers, timeline, team, products, process, photo,
  stats, chart, table, testimonial, cierre — see Templates below.
- `templates/estado-cuenta/` — regulated account-statement template, 100% data-driven
  from `data.json` — see Templates below.

## Components

All are React components compiled into the shared bundle; consume via
`window.TandaMirandaDesignSystem_a4a6f5`. Register-aware components restyle themselves
under `[data-surface]` / `[data-density]` / `[data-theme]` with zero code changes.

- **Core (v1, upgraded):** Button (variants primary/navy/secondary/tertiary/destructive/
  link, sizes sm–lg, loading with preserved width, iconStart/iconEnd, href renders true
  link) · Input (sizes, invalid/disabled/read-only, icons/addons, mono) · Badge ·
  FeatureCard · SectionHeading.
- **Actions:** IconButton (mandatory accessible name) · SegmentedControl (radio
  semantics, roving tabindex) · ButtonGroup (attached related actions) · ToggleGroup
  (multi-select, aria-pressed) · Fab (one per mobile view; circle or extended) ·
  SpeedDial (2–5 labeled fan-out actions) · CopyButton (visible "Copiado" + live region).
- **Forms:** Field (label/help/error/counter wrapper — wires ids + aria-describedby) ·
  Textarea · Select (APG listbox: arrows, type-ahead, Esc) · Combobox (input + filtered
  listbox, result-count live region, async-ready) · Checkbox (indeterminate) ·
  RadioGroup (fieldset+legend, native roving) · Switch (role="switch", instant effect) ·
  Slider (full keyboard map, visible output) · DateInput (dd/mm/aaaa mask, never mm/dd) ·
  DatePicker (input-first + APG calendar grid: arrows/PgUp/PgDn/Home/End, min/max
  disabled-with-reason) · FileUpload (dropzone + browse button path, limits declared up
  front, live region) · NumberInput (± stepper, ↑↓, clamp) · TimePicker (hh:mm 24 h mask
  + step listbox) · Rating (native radios, "n de N" text always) · TransferList
  (dual checkbox lists + explicit move buttons).
- **Navigation:** AppBar (environment badge slot — sandbox/producción) · SideNav (red
  active indicator, aria-current) · Tabs (APG tablist) · Breadcrumbs (collapses >4) ·
  Pagination (es-MX "1–25 de 3,412") · Stepper (done/current/upcoming) · Menu (roving
  menuitems, destructive items) · CommandPalette (⌘K/Ctrl+K, grouped results + recents,
  never the only path to a feature) · Link (external icon + SR notice, standalone mode) ·
  BottomNav (3–5 labeled destinations, red indicator, safe-area) · MegaMenu (marketing
  disclosure panel: columns + promo slot) · ScrollProgress (editorial pages only,
  decorative).
- **Feedback:** Alert (4 severities, subtle/solid, icon+label always) · Toast +
  ToastRegion (polite live region) · ProgressBar · ProgressRing (determinate circular,
  center figure as text) · Spinner (300ms anti-flicker delay) · Skeleton (no layout
  shift) · EmptyState (4 types incl. reference ID) · Backdrop (blocking scrim,
  contained mode, role=status).
- **Overlays:** Modal (focus trap, Esc, focus-return, alertdialog variant) · Drawer ·
  Tooltip (hover AND focus, 1.4.13) · Popover · Lightbox (document/photo viewer: zoom,
  ←→, focus trap, counter) · Coachmark (walktour: spotlight + n-de-N card, Esc/←→,
  first-run only, always dismissible).
- **Data display:** Table (semantic table, sticky header, aria-sort, tri-state selection,
  skeleton rows, density-aware) · DataGrid (virtualized — thousands of rows windowed,
  APG grid keyboard, editable cells with Enter/F2·Esc, density-aware row height) · Card ·
  StatTile (tabular nums, ▲/▼ deltas with SR text) · Tag (removable) · StatusIndicator
  (shape+label+color) · Avatar (initials fallback) · Timeline (audit stream) ·
  DescriptionList · Accordion · PageHeader · Typography (token-mapped text, semantic
  `as` decoupled) · Divider (label option) · List/ListItem (≥44px rows, button/link/
  static) · Image (reserved ratio, explicit fallback) · Clamp (never on legal copy) ·
  AnchorBadge (dot/count on icon, srLabel) · TreeView (APG tree keyboard) · OrgChart
  (≤3 levels, CSS connectors) · Carousel (no autoplay, arrows+dots+counter) · Charts:
  BarChart / LineChart / DonutChart (dependency-free SVG on --chart-* tokens,
  role=img summaries, text legends).
- **Fintech domain:** AmountInput (es-MX formatting, ISO currency suffix) · ClabeInput
  (18-digit mask, checksum, bank auto-identify) · OtpInput (paste-distributing,
  one-time-code) · MaskedValue (•••• 4587, auditable reveal, copy) · CatDisclosure
  (versioned CAT/GAT block, text-xs floor).

Deliberately NOT built, with reasons: Map (external tile service — the system ships no
third-party deps), rich-text Editor & Markdown renderer (heavy deps; out of token scope),
Animate presets (motion register is productive-only), custom Scrollbar (native scrollbars
kept for a11y), Box/Stack/Grid/Container/Masonry primitives (layout is plain CSS grid/
flex/columns here), ImageList (CSS grid + Image covers it), Paper (= Card),
TextareaAutosize (Textarea resizes), Snackbar (= Toast), Dialog (= Modal), Autocomplete
(= Combobox), multi-language switcher (i18n architecture is documented; the control is a
plain Select). MUI's anchored Badge = AnchorBadge; chip-style label = Badge/Tag;
Minimals' Walktour = Coachmark; Text max line = Clamp; Upload = FileUpload;
Navigation section = SideNav; Organization chart = OrgChart.

## UI Kits

- **`ui_kits/website/`** — homepage recreation (marketing register): nav, hero, navy
  trust-split, benefits row, email capture, footer. Copy reproduced verbatim from the
  live screenshot (including its production "Lorem ipsum…" body).
- **`ui_kits/app/`** — pilot loan dashboard proving the product layer: AppBar + SideNav
  shell, KPI tiles with as-of timestamps, amortization Table, activity Timeline, masked
  card + domiciliación Switch, vaquita advice card, CAT disclosure, light/dark toggle
  (persisted), and a non-optimistic pay flow (amount → review + OTP → receipt with folio).
- **`ui_kits/ops/`** — pilot ops console proving the back-office register: compact
  density default with switcher, producción environment badge, search + Select + Combobox
  filter bar with applied-filter chips, bulk-action bar with alert-dialog confirmation,
  834-row virtualized DataGrid (sortable, selectable), record Drawer with maker-checker
  approval + audit Timeline, and a ⌘K CommandPalette.

## Templates

- **`templates/estado-cuenta/`** ("Estado de Cuenta") — regulated Crédito Simple
  statement (CONDUSEF format), rebuilt from the company's real PDF as a printable
  doc-page DC. 100% data-driven: `EstadoCuenta.dc.html` renders from a sibling
  `data.json` (or `window.setEstadoCuenta(json)`, or the `fuenteDatos` tweak pointed at
  an endpoint); saldos, próximo pago, vencidos, resumen del periodo, totales and the
  gráfica are all DERIVED from `pagos[]`, so the printed document always reconciles.
  `InstruccionesJson.dc.html` is the printable integration spec (schema tables, computed
  fields, formats, full example). The sample `data.json` reproduces the original
  statement to the cent (96-row schedule, transcribed + consistency-checked).

- **`templates/brand-deck/`** ("Brand Deck") — comprehensive 17-slide 16:9 deck in full
  brand language: navy portada, agenda, red Carmín-pattern + navy section dividers,
  bullets, history timeline, team (circle image slots), product cards, 4-step process,
  two-column text+image, 70/30 photo, tabular stats, CSS bar chart, comparison table,
  testimonial quote, vaquita cierre with regulated footer. Copy/years/figures are
  placeholder in brand voice — built from the Manual's rules (the `Presentaciones/`
  PDFs are finished decks whose content isn't cloned); illustrative values are marked
  in-slide.

## Content fundamentals

- **Language & register:** es-MX, informal *tú* ("Descubre…", "Alcancemos juntos tus
  metas") — never *usted* in marketing/product. Formal *usted* only in official
  correspondence ("su consideración", "Presente:").
- **Punctuation:** inverted exclamation marks are a signature — headlines wrapped `¡…!`
  ("¡Este mes estrena auto!", "¡Los sueños sí se hacen realidad!"). Marketing only;
  ≤1 per flow in product, never in errors.
- **Voice:** aspirational, warm, encouraging — dreams and milestones, not APRs; money is
  framed by what it buys or protects ("más fácil y segura" recurs across campaigns).
  Product register adds *Trustworthy*: precise about money, risk, and state; **no humor
  or celebration in money movement, collections, or errors** (see tone-map card).
- **Headline pattern:** serif-italic emotional hook + calm sans payoff line; one word per
  headline picked out in brand red (usually the payoff — "No.1").
- **CTAs:** marketing — short imperative uppercase ("APLICA HOY MISMO"); product —
  verb-first sentence case ≤3 words ("Solicitar crédito"), pairs are verb-vs-verb,
  never "Sí/No/OK".
- **Errors:** what happened + how to fix, system takes responsibility ("No pudimos…"),
  reference ID at page level. Terminology glossary is law (one concept = one term).
- **No emoji.** Emphasis = red, italics, exclamation — never emoji.

## Visual foundations

- **Color (authoritative — Manual p.10):** Blanco `#FFFFFF` predominant; **Azul Tanda**
  `#0F1D41` (Pantone 281 C) backgrounds/titles; **Rojo Miranda** `#ED1C24` (Pantone 485 C)
  icons, plecas, accents — as background only with the pattern; **Rojo Carmín** `#B5231D`
  (Pantone 7620 C) EXCLUSIVELY for the pattern (30% digital / 60% print over Rojo
  Miranda). K100 for long body copy. Gradients need Marketing approval. All other ramp
  steps + status hues (green/amber/blue) are derived working values, chroma-matched in
  OKLCH.
- **Type (authoritative — Manual p.11):** **Noto Sans** + **Libre Baskerville Italic**
  (serif only ever italic — never bold, never upright). Hierarchy: Títulos de acento
  (LB Italic 75pt) · Título (NS Bold 48pt) · Subtítulo (NS SemiBold Italic 28pt) · Textos
  (NS Regular 18pt) · Website (NS Bold 18pt). v2 adds a UI scale (`--text-xs…lg`), system
  mono for identifiers (CLABE/RFC/folios), and mandatory tabular lining figures wherever
  numbers align. (Ignore the usage-guide's stray "Jost / Haute Couture" line — template
  boilerplate.)
- **Shape:** pills on every marketing button/input (`999px`); 10–20px on cards; product
  UI steps controls to `--radius-md` 10px (user decision) with pills preserved for tags/
  avatars. Nested radii: inner ≈ outer − padding.
- **Elevation:** brand is nearly flat — color blocking over shadows. v2 defines a full
  `elevation-0…500` + z-scale for product overlays; dark mode expresses elevation by
  surface lightening.
- **Backgrounds:** flat color or full-bleed warm photography; the M-glyph pattern
  (red-on-red / navy-on-navy) as background texture only. No gradients, no glassmorphism.
- **Vaquita (Manual p.18):** the cow mascot — abundance + communal savings reframed with
  legal certainty. Use **moderately**, customer-contact warmth moments only (the pilot
  uses it once, on the advice card). Navy/white/color, min 2cm.
- **Isotipo (Manual p.17):** abstract T+M mark (cow ears / diagonal legs, growth step).
  Space-constrained substitute or ornament; opposite corner from the logo. Min 1cm.
- **Photography (Manual p.20):** *Productos y servicios* · *De personas y aspiracionales*
  · *Conceptuales*. Luminous, no filters; carry blue/red accents; ads compose 70% image /
  30% message. See `guidelines/brand-photography*.card.html`.
- **Iconography:** no icon set ships with the brand — thin-stroke Lucide-style SVGs are
  substituted inline (aria-hidden when decorative; icon-only controls get aria-label +
  tooltip). Flag if pixel-exact originals turn up.
- **Motion:** none exists in brand sources; v2 tokens are conservative productive-register
  additions. Transform/opacity only; reduced-motion honored globally.

## Accessibility

WCAG 2.2 AA is an entry gate: pre-approved pairing matrix (see semantic-roles card),
one global `:focus-visible` ring, roving-tabindex composites, focus trap + return in
overlays, ≥24px targets (44 touch), paste never blocked, redundant entry avoided,
`prefers-reduced-motion` global. Because brand red = danger red, **icon + label always
accompany status color** — this is the system's most enforced rule. See
`guidelines/a11y-gates.card.html`.

## Caveats & asks

- **Fonts are exact** — Libre Baskerville + Noto Sans TTFs ship in `assets/fonts/`.
  Mono is a system stack (no brand mono exists).
- **The product layer is original work** — no app UI existed in the source. The manual
  stays authoritative where it speaks; every derived value (neutrals, status hues,
  elevation, motion, app register) is labeled as such in the token files.
- **Icons are substitutions** (see Iconography).
- **SVG colors are baked as `fill` attributes** — this environment strips `<style>` from
  stored SVGs; re-baked variants render correctly on light/navy/red. Re-export from
  Illustrator with "presentation attributes" or expect to re-bake.
- **Slide template:** `templates/brand-deck/` now exists (see Templates). It derives
  from the Manual's rules rather than the finished `Presentaciones/` PDFs — point me at
  a specific deck if you want its exact layouts recreated as additional slide types.
- **Not-yet-built components** listed at the end of the inventory above; the reference
  architecture's governance/tooling sections (Figma pipeline, CI gates, versioning) are
  process docs beyond this repo's scope — the token structure here is ready for a DTCG/
  Style Dictionary pipeline when engineering picks it up.

**Ask:** confirm the derived status hues (green/amber/blue) and the red-650 app-CTA
compromise with Marketing (they sit next to official colors); tell me which untouched
collateral (social templates, deck, business card) to turn into editable kits next.
