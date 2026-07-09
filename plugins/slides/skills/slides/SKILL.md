---
name: slides
description: "Create browser-based HTML slide presentations with page-by-page navigation like PowerPoint. Uses a modular architecture with individual HTML files per slide, supporting multiple layout templates and themes. Use this skill when the user needs to create any type of presentation or slideshow."
---

# HTML Slides Guide

## Overview

HTML Slides is a browser-based presentation format with the following features:
- Opens directly in any browser, no software installation needed
- Supports keyboard, mouse, and touch navigation
- Modular architecture with separate files per slide, easy to maintain
- Customizable themes

## File Structure

```
project/
├── index.html          # Main controller (navigation logic, style references)
├── styles/
│   ├── base.css        # Base styles (required)
│   └── theme-*.css     # Theme files (choose one)
├── slides/
│   ├── 01_cover.html   # Individual slides
│   ├── 02_xxx.html
│   └── ...
└── images/             # Image assets (optional)
```

## Stage Model and Scaling Rules

Generated decks must use a fixed 16:9 design stage instead of viewport-sized slides.

- The default logical stage is `1600x900`, defined by `--stage-width` and `--stage-height` in `styles/base.css`.
- `index.html` wraps slide fragments with `.viewport-shell > .slide-stage > .slides-container`.
- Browser resize uses this calculation:

```javascript
scale = Math.min(viewportWidth / 1600, viewportHeight / 900)
```

- The calculated value is applied to `.slide-stage` through `--stage-scale` and `transform: scale(...)`.
- Navigation controls, page indicators, dots, and progress stay outside the stage so they remain usable at small viewport sizes.
- Slide fragments must still contain only one root `<div class="slide ...">...</div>` element.
- Mobile portrait uses a separate scrolling strategy; do not force tiny 16:9 text on narrow phones.

## Layout Rules

Pick a layout class based on the slide's information density:

| Class | Use For | Behavior |
|-------|---------|----------|
| `.center-layout` | cover, ending, quote, sparse emphasis pages | Centers content visually in the stage |
| `.chapter-layout` | section dividers | Centers a large title and short transition text |
| `.dense-layout` | tables, dashboards, detailed comparison pages | Reduces padding and title size while preserving readable body/table text |
| `.wide-layout` | grids, split layouts, dashboards, wide images | Expands max content width |
| `.fit-content` | normal structured pages | Creates a controlled vertical content area |
| `.balanced-grid` | 4-card grids | Uses equal rows/columns for stable card sizing |

Do not make every slide `justify-content: center`. Use centered layouts only for presentation-style pages with little content. Dense pages, long content, tables, dashboards, and split layouts should align from the top with controlled spacing.

### Presentation-Style Pages

Use for cover, section divider, ending, quote, and sparse message slides:

- Large title, short supporting text, generous whitespace.
- Use `.center-layout` or `.chapter-layout`.
- Keep content max width around `1000-1120px`.
- Avoid long paragraphs and more than 3-4 visual elements.

### Information-Dense Pages

Use for tables, dashboards, comparison grids, timelines, and detailed content:

- Use `.dense-layout` when a page contains tables or many small data points.
- Keep body text readable; do not shrink table text below the readable threshold.
- Split overly dense material across multiple slides instead of relying on very small fonts.
- Use `.wide-layout` for multi-column layouts.

## Creation Workflow

1. **Copy the controller file** — from `assets/index.html`
2. **Copy style files** — from `assets/styles/`: `base.css` + a theme file
3. **Create slides** — reference layout templates in `assets/slides/`
4. **Configure the slide list** — edit the `slideFiles` array in `index.html`

## Layout Templates

All templates are located in the `assets/slides/` directory:

| Template | Purpose |
|----------|---------|
| `cover.html` | Cover page |
| `section.html` | Section divider / chapter page |
| `content.html` | General content (title + body text) |
| `list.html` | Numbered list layout |
| `grid.html` | Grid card layout |
| `split.html` | Two-column layout |
| `image.html` | Image showcase layout |
| `table.html` | Table layout |
| `roadmap.html` | Roadmap / timeline layout |
| `metrics.html` | Metrics / dashboard layout |
| `quote.html` | Quote / emphasis layout |
| `stats.html` | Statistics / key metrics layout |
| `ending.html` | Closing page |

Each template is an HTML fragment containing only a `<div class="slide">...</div>` structure.

## Template Examples

Cover:

```html
<div class="slide slide-cover center-layout">
    <div class="content">
        <p class="eyebrow">Presentation</p>
        <h1>Presentation Title</h1>
        <p class="subtitle">A concise subtitle that frames the deck.</p>
    </div>
</div>
```

Section divider:

```html
<div class="slide chapter-layout">
    <div class="content">
        <p class="eyebrow">Section 01</p>
        <h1>Section Title</h1>
        <p class="subtitle">One sentence that explains the transition.</p>
    </div>
</div>
```

Content:

```html
<div class="slide">
    <div class="content fit-content">
        <h2>Page Title</h2>
        <p>Body text goes here.</p>
    </div>
</div>
```

Grid cards:

```html
<div class="slide wide-layout">
    <div class="content fit-content">
        <h2>Grid Layout Title</h2>
        <div class="balanced-grid">
            <div class="card"><h3>Card One</h3><p>Description.</p></div>
            <div class="card"><h3>Card Two</h3><p>Description.</p></div>
            <div class="card"><h3>Card Three</h3><p>Description.</p></div>
            <div class="card"><h3>Card Four</h3><p>Description.</p></div>
        </div>
    </div>
</div>
```

Split layout:

```html
<div class="slide wide-layout">
    <div class="content fit-content">
        <h2>Split Layout Title</h2>
        <div class="split">
            <div class="split-left"><h3>Left</h3><p>Primary content.</p></div>
            <div class="split-right"><h3>Right</h3><p>Supporting content.</p></div>
        </div>
    </div>
</div>
```

Table:

```html
<div class="slide dense-layout wide-layout">
    <div class="content fit-content">
        <h2>Table Title</h2>
        <div class="table-container">
            <table class="table">...</table>
        </div>
        <p class="table-note">For more than 6 columns, summarize key comparisons in cards or split across slides.</p>
    </div>
</div>
```

Roadmap / timeline:

```html
<div class="slide wide-layout">
    <div class="content fit-content">
        <h2>Roadmap</h2>
        <div class="timeline">
            <div class="timeline-step"><h3>Discover</h3><p>Frame the problem.</p></div>
            <div class="timeline-step"><h3>Design</h3><p>Shape the solution.</p></div>
            <div class="timeline-step"><h3>Build</h3><p>Implement and measure.</p></div>
            <div class="timeline-step"><h3>Scale</h3><p>Operationalize and iterate.</p></div>
        </div>
    </div>
</div>
```

Metrics / dashboard:

```html
<div class="slide dense-layout wide-layout">
    <div class="content fit-content">
        <h2>Metrics Dashboard</h2>
        <div class="dashboard-grid">
            <div class="metric-card"><div class="metric-number">42%</div><div class="metric-label">Primary outcome</div></div>
            <div class="metric-card"><div class="metric-number">3.8x</div><div class="metric-label">Efficiency gain</div></div>
        </div>
    </div>
</div>
```

## Table Guidance

- Prefer 3-6 columns and 4-8 rows per table slide.
- Use `dense-layout wide-layout` for tables.
- Keep `.table` at a readable font size; the base CSS uses `--readable-table-font: 22px` on the 1600px stage.
- Tables should scroll inside `.table-container` if needed instead of shrinking indefinitely.
- For wide data, create a summary-card slide or split the table across multiple slides.
- Avoid putting large paragraphs inside table cells.

## Themes

All themes are in the `assets/styles/` directory. Switch themes by changing the CSS file reference in `index.html`.

### Basic Themes

| Theme File | Style |
|------------|-------|
| `theme-dark.css` | Dark (deep blue background) |
| `theme-light.css` | Light (white background) |

### Theme Factory Themes

Professional themes from the theme-factory skill with curated color palettes and fonts:

| Theme File | Style | Best For |
|------------|-------|----------|
| `theme-ocean-depths.css` | Deep ocean blue | Corporate presentations, financial reports |
| `theme-sunset-boulevard.css` | Warm sunset tones | Creative proposals, marketing decks |
| `theme-forest-canopy.css` | Forest green | Sustainability, environmental topics |
| `theme-modern-minimalist.css` | Gray-white minimal | Tech, design showcases |
| `theme-golden-hour.css` | Golden autumn warmth | Food & beverage, hospitality branding |
| `theme-arctic-frost.css` | Ice blue cool tones | Healthcare, clean technology |
| `theme-desert-rose.css` | Desert rose | Fashion, beauty brands |
| `theme-botanical-garden.css` | Fresh garden | Gardening, natural products |
| `theme-tech-innovation.css` | Tech blue-black | Tech startups, AI presentations |
| `theme-midnight-galaxy.css` | Cosmic deep purple | Entertainment, gaming industry |

## Asset Management

### Images
- Store in the `images/` directory
- Reference using relative paths: `<img src="images/xxx.png">`

### External Data (optional)
To load detailed data in a modal popup:
- Create a `data/` directory for HTML fragments
- Use `openModal('data/xxx.html')` to load them

## Navigation Features

The controller file includes the following navigation:
- **Keyboard**: ← → to navigate, ↑ first slide, ↓ last slide, Space/Enter next slide
- **Mouse**: Bottom buttons, right-side dot navigation
- **Touch**: Swipe left/right
- **Progress**: Top progress bar, page indicator
- **Stage fitting**: 16:9 slide stage automatically scales to the viewport

## Custom Themes

Modify CSS variables in the theme file:

```css
:root {
    --primary: #background-color;
    --accent: #accent-color;
    --text: #text-color;
    --text-muted: #secondary-text;
    --gold: #heading-decoration-color;
}
```

## Math Formulas (LaTeX)

HTML does not natively support LaTeX rendering. Use the **KaTeX** library.

### 1. Include KaTeX in index.html

Add to `<head>`:

```html
<!-- KaTeX -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js"></script>
```

Add rendering logic at the end of `<script>`:

```javascript
// KaTeX rendering
function renderMath() {
    if (typeof renderMathInElement !== 'undefined') {
        renderMathInElement(document.body, {
            delimiters: [
                {left: '$$', right: '$$', display: true},
                {left: '$', right: '$', display: false}
            ],
            throwOnError: false
        });
    }
}
// Call renderMath() after slides have loaded
```

### 2. Formula Syntax

| Type | Syntax | Effect |
|------|--------|--------|
| Inline | `$E = mc^2$` | Embedded in text |
| Block | `$$\mathcal{J}(\theta) = \mathbb{E}[...]$$` | Centered, standalone |

### 3. Caveats

1. **HTML escaping**: `<` is parsed as an HTML tag — use `\lt` instead
   - Bad: `$o_{<t}$`
   - Good: `$o_{\lt t}$`

2. **Pipe symbol**: Use `\mid` for conditional probability bars
   - Bad: `$\pi(y_t | x)$`
   - Good: `$\pi(y_t \mid x)$`

3. **Text mode**: Use `\text{}` for words inside formulas
   - Bad: `$\theta_{old}$` (renders "old" as italic variables)
   - Good: `$\theta_{\text{old}}$`

4. **No PDF images**: HTML `<img>` does not support PDF — convert to PNG/SVG

## Notes

1. Each slide file should only contain `<div class="slide">` content
2. Use `01_`, `02_` prefixes for slide filenames to maintain order
3. The modular architecture requires a local server (`python -m http.server`)
4. Single-file builds can be opened directly in a browser
5. Validate generated decks at `1366x768`, `1920x1080`, and `2560x1440`; inspect cover, section, table, and grid pages for centering, readable scale, overflow, and navigation overlap
