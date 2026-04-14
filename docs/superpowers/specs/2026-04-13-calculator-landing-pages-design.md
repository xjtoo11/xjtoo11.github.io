# Web Calculator Landing Pages - Design Spec

## Context

5 iOS Toolkit Apps need SEO landing pages to drive organic Google traffic to App Store downloads. Each app gets 5 pages (25 total), each with a free web calculator + educational content + App Store CTA. **P0 scope: HVAC 5 pages first.**

Competitors are weak: FUTEK has 0 words, Lennox requires login, OmniCalculator uses generic templates. 1000+ word expert content + FAQ schema + no login wall will outrank them.

---

## Architecture

### Tech Stack
- Pure static HTML + CSS + vanilla JavaScript
- No frameworks (SPA hurts SEO — FUTEK's Angular is the anti-pattern)
- Hosted on GitHub Pages at `dust.github.io`
- Each page is an independent HTML file

### Three-AI Workflow (adapted from iOS workflow)

| Role | Claude Code (Commander) | Codex CLI (Coder) | Gemini CLI (Reviewer) |
|------|------------------------|-------------------|----------------------|
| Does | Plan architecture, design CSS template, manage git, deploy, final decisions | Write HTML pages + calculator JS + educational content | SEO audit, content quality review, JS logic verification |
| Doesn't | Write bulk HTML/content | Make architecture decisions, touch CSS template | Write code, make final decisions |

### Execution Phases

```
Phase 1: Claude Code plans architecture + task breakdown → user confirms
Phase 2: Claude Code creates shared CSS + HTML template skeleton → commit
Phase 3: Codex writes pages → Gemini reviews → Claude Code arbitrates (loop × 5)
Phase 4: Gemini global review (SEO checklist + cross-page consistency) + browser verify
Phase 5: Claude Code generates sitemap.xml + robots.txt + deploys to GitHub Pages
```

### Project Structure (P0)

```
/Volumes/SSD1/web/
├── index.html                              # Homepage: tool index
├── styles.css                              # Shared styles
├── hvac/
│   ├── superheat-subcooling-calculator.html
│   ├── manual-j-calculator.html
│   ├── btu-calculator.html
│   ├── refrigerant-charge-calculator.html
│   └── glycol-freeze-point-calculator.html
├── sitemap.xml
└── robots.txt
```

---

## Shared CSS Design (`styles.css`)

### Design System

- **Style**: Clean engineering — professional, trustworthy, function-first
- **Background**: `#ffffff`
- **Text**: `#1a1a2e` (primary), `#4a5568` (secondary)
- **Accent**: `#2563eb` (professional blue)
- **Success/Result**: `#059669` (green, for result numbers)
- **Calculator bg**: `#f0f7ff` (light blue card)
- **CTA bg**: `#f7fafc` (light gray card)
- **Table zebra**: `#f8fafc`
- **Font stack**: `-apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif`
- **Max content width**: `800px`, centered
- **Responsive breakpoint**: `768px`

### Key CSS Components

1. **`.calculator`** — Rounded card with light blue bg, padding 24px, shadow
2. **`.calc-input`** — Height 44px+, `inputmode="decimal"`, large touch target
3. **`.calc-result`** — Large number `2rem`, green color, bold
4. **`.unit-toggle`** — Simple Imperial/Metric toggle switch
5. **`.app-cta`** — Light gray card with App Store badge, non-intrusive
6. **`.formula-block`** — Highlighted formula display (monospace bg)
7. **`.reference-table`** — Zebra-striped HTML table, horizontal scroll on mobile
8. **`.faq-section`** — Accordion-style FAQ (CSS-only, no JS needed)
9. **`header`** — Minimal: logo + nav to other calculator categories
10. **`footer`** — Copyright + privacy + links to all 5 toolkit App Store pages

---

## Page Template Structure

Each landing page follows this exact structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>{Keyword} Calculator - Free Online Tool | EngineerCalc</title>
  <meta name="description" content="Free {keyword} calculator. Enter {inputs}, get {outputs} instantly. No login required. {value prop}.">
  <link rel="canonical" href="https://dust.github.io/{path}">
  <link rel="stylesheet" href="../styles.css">
  
  <!-- JSON-LD: WebApplication + Article + FAQPage -->
  <script type="application/ld+json">{ ... 3 schemas ... }</script>
</head>
<body>
  <header>
    <a href="/">EngineerCalc</a>
    <nav>HVAC | Welding | Marine | Oil&Gas | DataCenter</nav>
  </header>

  <main>
    <h1>{Keyword} Calculator</h1>
    <p class="subtitle">Free online {description}. No login required.</p>

    <!-- Calculator Card -->
    <section class="calculator">
      <div class="calc-inputs">
        <!-- Input fields with labels, units, default values -->
      </div>
      <div class="unit-toggle">
        <label><input type="checkbox" id="unit-toggle"> Metric</label>
      </div>
      <div class="calc-results">
        <!-- Real-time results, large numbers -->
      </div>
    </section>

    <!-- App CTA Card (right after results) -->
    <section class="app-cta">
      <p>Get 13 HVAC calculators + refrigerant PT tables offline on your iPhone.</p>
      <a href="{app-store-url}?utm_source=web&utm_medium=calculator&utm_campaign={keyword}">
        <img src="../assets/app-store-badge.svg" alt="Download on the App Store">
      </a>
    </section>

    <!-- Educational Content (1000-1500 words) -->
    <article>
      <h2>What is {Concept}?</h2>
      <p>150-200 words definition and explanation</p>

      <h2>How to Calculate {Topic}</h2>
      <p>200-300 words formula explanation</p>
      <div class="formula-block">{Formula}</div>
      <p>Worked example using default values</p>

      <h2>{Topic} Reference Table</h2>
      <table class="reference-table">...</table>

      <h2>Frequently Asked Questions</h2>
      <div class="faq-section">
        <!-- 3-5 Q&As matching FAQPage schema -->
      </div>
    </article>

    <!-- Related Calculators (internal links) -->
    <nav class="related-calculators">
      <h3>More HVAC Calculators</h3>
      <ul><!-- Links to other 4 HVAC pages --></ul>
    </nav>
  </main>

  <footer>
    <p>&copy; 2026 EngineerCalc | <a href="/privacy">Privacy Policy</a></p>
    <div class="footer-apps"><!-- All 5 toolkit links --></div>
  </footer>

  <!-- Page-specific calculator JS (inline) -->
  <script>
    // All inputs bind to calculate()
    // Invalid input shows "—"
    // Unit toggle switches all labels and values
  </script>
</body>
</html>
```

---

## HVAC Pages Detail (P0)

### Page 1: Superheat & Subcooling Calculator

**URL**: `/hvac/superheat-subcooling-calculator.html`
**Target keyword**: superheat subcooling calculator

**Calculator inputs**:
- Refrigerant type: dropdown (R-410A, R-22, R-32, R-134a, R-407C, R-404A)
- Suction pressure (psig): number input, default 118
- Suction temperature (°F): number input, default 55
- Liquid line pressure (psig): number input, default 418
- Liquid line temperature (°F): number input, default 100

**Calculator outputs**:
- Superheat: `Suction Temp - Saturated Suction Temp` (°F)
- Subcooling: `Saturated Liquid Temp - Liquid Line Temp` (°F)
- Saturated suction temperature (from P-T lookup)
- Saturated liquid temperature (from P-T lookup)

**JS data**: Refrigerant P-T lookup table (pressure → saturation temp) for each refrigerant. Use linear interpolation between table points.

**Reference table**: Refrigerant P-T chart for R-410A, R-22, R-32 (pressure, sat temp evap, sat temp cond)

**FAQ topics**: What is superheat, what is subcooling, normal ranges, how to measure, target values by metering device

---

### Page 2: Manual J Load Calculator

**URL**: `/hvac/manual-j-calculator.html`
**Target keyword**: Manual J calculator

**Calculator inputs**:
- Climate zone: dropdown (1-8 with city examples)
- Floor area (sq ft): default 2000
- Ceiling height (ft): default 8
- Window area (sq ft): default 200
- Insulation level: dropdown (Poor/Average/Good/Excellent)
- Number of occupants: default 4
- Direction of largest window: dropdown (N/S/E/W)

**Calculator outputs**:
- Cooling load (BTU/h)
- Heating load (BTU/h)
- Recommended equipment tonnage
- Equipment size range (min-max tons)

**Simplified formula**: Uses climate zone design temps + area-based load factors + window/insulation multipliers. (Not full ACCA Manual J, but a reasonable estimate with clear disclaimer.)

**Reference table**: Climate zone design temperatures (heating/cooling) for major US cities

**FAQ topics**: What is Manual J, why it matters, who needs it, how accurate is a simplified estimate, professional vs DIY

---

### Page 3: BTU Calculator for HVAC

**URL**: `/hvac/btu-calculator.html`
**Target keyword**: BTU calculator HVAC

**Calculator inputs**:
- Room area (sq ft): default 500
- Ceiling height (ft): default 8
- Insulation quality: dropdown (Poor/Average/Good)
- Window area (sq ft): default 40
- Climate zone: dropdown (1-8)
- Sun exposure: dropdown (Low/Medium/High)
- Number of occupants: default 2

**Calculator outputs**:
- Required BTU/h
- Recommended AC tonnage
- BTU per sq ft ratio

**Formula**: Base BTU = area × BTU-per-sqft-factor, adjusted by ceiling, windows, insulation, sun, occupants.

**Reference table**: BTU/sq ft quick reference by room type and climate

**FAQ topics**: What is BTU, BTU vs tons, how many BTU per sq ft, oversizing risks

---

### Page 4: Refrigerant Charge Calculator

**URL**: `/hvac/refrigerant-charge-calculator.html`
**Target keyword**: refrigerant charge calculator

**Calculator inputs**:
- Refrigerant type: dropdown (R-410A, R-22, R-32, R-134a)
- Metering device: dropdown (TXV, Fixed Orifice/Piston)
- Measured superheat (°F): default 12
- Measured subcooling (°F): default 10
- Target superheat (°F): auto-filled based on metering device
- Target subcooling (°F): auto-filled based on metering device
- Line set extra length (ft): default 0

**Calculator outputs**:
- Charge status: "ADD refrigerant" / "REMOVE refrigerant" / "Charge is OK"
- Superheat deviation from target
- Subcooling deviation from target
- Line set charge adjustment (oz)

**Reference table**: Target superheat/subcooling values by metering device type and refrigerant

**FAQ topics**: How to check charge, TXV vs fixed orifice method, when to add/remove, superheat vs subcooling method

---

### Page 5: Glycol Freeze Point Calculator

**URL**: `/hvac/glycol-freeze-point-calculator.html`
**Target keyword**: glycol freeze point calculator

**Calculator inputs**:
- Glycol type: dropdown (Ethylene Glycol, Propylene Glycol)
- Concentration (%): range slider 0-70, default 30
- System volume (gallons): default 100

**Calculator outputs**:
- Freeze point (°F)
- Burst point (°F)
- Glycol volume needed (gallons)
- Water volume needed (gallons)
- Specific gravity
- Viscosity increase factor

**JS data**: Concentration-to-freeze-point lookup table for both glycol types (use polynomial curve fit or table interpolation).

**Reference table**: Concentration vs freeze point vs burst point for both ethylene and propylene glycol

**FAQ topics**: Ethylene vs propylene glycol, how to measure concentration, burst vs freeze point, when to use glycol

---

## SEO Checklist (per page)

Every page must pass all items before shipping:

- [ ] H1 contains target keyword
- [ ] Title tag: keyword + "Calculator" + "Free"
- [ ] Meta description: keyword + value prop, under 150 chars
- [ ] URL slug contains keyword (hyphenated)
- [ ] 3 JSON-LD schemas: WebApplication + Article + FAQPage
- [ ] 1000-1500 words educational content
- [ ] 3-5 FAQ items matching FAQPage schema
- [ ] At least 1 reference data table (HTML table)
- [ ] At least 1 worked example
- [ ] Image alt text contains keyword
- [ ] Mobile responsive
- [ ] Page load < 2s (Lighthouse Performance > 90)
- [ ] Canonical URL set
- [ ] Internal links: bottom of page links to other 4 HVAC calculators
- [ ] App Store CTA with UTM parameters

---

## Codex Prompt Template (Web adaptation)

```bash
codex exec \
  -C "$PROJECT_DIR" \
  --full-auto \
  --ephemeral \
  "$(cat <<'PROMPT'
## Context
Static HTML + CSS + vanilla JS website for SEO landing pages.
Shared CSS is in ../styles.css (already created, do not modify).
Each page is a standalone HTML file.

## Current Task
Create {path} — a {keyword} calculator landing page.

## Requirements
1. Follow the HTML template structure exactly (see below)
2. Calculator: real-time calculation on input change, no Calculate button
3. Default values pre-filled in all inputs
4. Invalid input shows "—" instead of NaN
5. Unit toggle: Imperial ↔ Metric for all values
6. inputmode="decimal" on number inputs
7. 3 JSON-LD schemas: WebApplication, Article, FAQPage
8. Educational content: 1000-1500 words, professional English for practitioners
9. At least 1 worked example + 1 reference data table
10. 3-5 FAQ items matching the FAQPage schema entries
11. Internal links to other 4 HVAC calculator pages at bottom
12. App CTA card below calculator results

## Calculator Specifications
{specific inputs, outputs, formula, data tables}

## Template Structure
{HTML template from design doc}

## Do NOT
- Modify styles.css or any other file
- Use any third-party library or CDN
- Add a "Calculate" button (real-time only)
- Use alert() or console.log() in production code
PROMPT
)"
```

## Gemini Review Template (Web adaptation)

```bash
cat hvac/superheat-subcooling-calculator.html | gemini -p "$(cat <<'PROMPT'
You are an SEO and web quality reviewer. Review this HTML landing page:

1. **SEO Compliance**:
   - Title tag contains keyword + "Calculator" + "Free"?
   - Meta description under 150 chars with keyword?
   - H1 contains target keyword?
   - Canonical URL set?
   - All 3 JSON-LD schemas present and valid (WebApplication, Article, FAQPage)?
   - FAQ schema entries match visible FAQ content?

2. **Calculator Logic**:
   - Real-time calculation (no button)?
   - Default values pre-filled?
   - Invalid input handled (no NaN)?
   - Unit toggle works?
   - Formula is mathematically correct?

3. **Content Quality**:
   - Word count 1000-1500?
   - Professional tone for practitioners?
   - At least 1 worked example?
   - At least 1 reference data table?
   - 3-5 FAQ items?

4. **HTML Quality**:
   - Mobile responsive meta tag?
   - Semantic HTML (article, section, nav)?
   - No inline styles (uses shared CSS)?
   - Accessible (alt text, labels, ARIA)?

Report issues only. If everything passes, say "LGTM".
PROMPT
)" -o text
```

---

## Verification Plan

1. **Per-page**: Open in browser, test calculator with default + edge values, check mobile view
2. **SEO**: Validate JSON-LD with Google Rich Results Test (or manual schema check)
3. **Performance**: Lighthouse audit for each page (target > 90)
4. **Cross-page**: Verify all internal links work, consistent styling across 5 pages
5. **Deployment**: Push to GitHub, verify pages load at dust.github.io/hvac/*

---

## Google Analytics & Tracking

- GA4 measurement tag in shared header (or each page head)
- App Store CTA links include UTM: `?utm_source=web&utm_medium=calculator&utm_campaign={keyword}`
- GA4 Measurement ID: **TBD** (user to provide, or create new property)

---

## Assets Needed

- `assets/app-store-badge.svg` — Apple's official "Download on the App Store" badge
- App Store URLs for 5 iOS Toolkit Apps — **TBD** (user to provide)
- GA4 Measurement ID — **TBD** (user to provide)
