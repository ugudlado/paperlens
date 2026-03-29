# Research Made Easy

Make research papers genuinely understandable — not by turning text into diagrams, but by explaining concepts in plain language, connecting them to real-world analogies, and letting users interact with the content at their own depth.

## Vision

Research papers are hard because of jargon, implicit assumptions, and abstract concepts that don't connect to anything familiar. Diagrams of the same words in circles don't help. What helps is:

- **Plain-language explanations** — "what does this actually mean?"
- **Real-world analogies** — "attention mechanism is like a spotlight deciding which words to focus on"
- **Jargon detection** — automatically identify and define technical terms inline
- **Progressive depth** — ELI5 → intermediate → expert explanations
- **"Why does this matter?"** — context for each section's contribution
- **Interactive Q&A** — ask questions about any part of the paper

The app is a **paper reader that makes you smarter**, not a diagram generator.

## Core Features

### 1. Smart Paper Reader (built)
- Paste paper text, auto-detect sections (Abstract, Methods, Results, etc.)
- Sidebar outline for navigation
- Section summaries with key concepts extracted

### 2. Highlight-to-Explain (priority)
- Select any text in the paper → get an instant plain-language explanation
- Explanations include real-world analogies when possible
- Adjustable depth: ELI5 / Standard / Technical
- Explanations are context-aware (grounded in the full paper, not generic)

### 3. Jargon Detector
- Automatically scan section text for technical terms and acronyms
- Show inline definitions on hover (tooltip-style)
- "Jargon density" indicator per section — helps users know which sections need more help
- Build a paper-specific glossary from detected terms

### 4. Real-World Analogies
- For each key concept, generate a relatable analogy
- Show analogies alongside the original text (side-by-side or toggle)
- Examples: "encoder-decoder is like a translator who first understands the meaning, then expresses it in another language"

### 5. Section Insights
- "Why this matters" — one-line context for each section's role in the paper
- "Key contribution" — what's new or different vs. prior work
- "In plain English" — full section rewritten for a general audience
- "Related to" — connections to other sections in the paper

### 6. Concept Map (existing, supplementary)
- Force-directed graph of key terms and relationships (already built)
- Useful as a navigation aid, not the primary understanding tool
- Clicking a concept highlights it in the text and shows its explanation

## Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
| Structure | HTML5 | Semantic markup, single `index.html` entry point |
| Styling | CSS3 | Custom properties for theming, no frameworks |
| Logic | Vanilla JS | ES modules, no build step, no bundler |
| Visualization | SVG | For concept map (supplementary) |
| Icons | None (CSS/SVG only) | No icon libraries |

**Hard constraints**: No frameworks, no npm, no build tools. Every file runs directly in the browser. Open `index.html`, serve via any static server.

## Architecture

```
index.html              # Entry point — app shell, layout
css/
  variables.css         # Design tokens (colors, spacing, typography)
  base.css              # Reset, body, global styles
  components.css        # Component-scoped styles
  visualizations.css    # Styles for concept map (supplementary)
  reader.css            # Styles for reading mode — highlights, tooltips, explanations
js/
  app.js                # Main entry — state management, event bus, routing
  parser.js             # Extracts sections from pasted paper text
  summarizer.js         # Heuristic text analysis — concepts, relationships
  visualizer.js         # Concept map orchestration (supplementary)
  explainer.js          # Core: generates explanations, analogies, definitions
  jargon.js             # Detects technical terms, acronyms, builds glossary
  components/
    paper-input.js      # Text input / file upload component
    section-reader.js   # Main reading view — paper text with interactive features
    highlight-panel.js  # Explanation panel for selected text
    jargon-tooltip.js   # Inline tooltip for detected jargon
    glossary.js         # Full paper glossary view
    depth-selector.js   # ELI5 / Standard / Technical toggle
    insight-card.js     # "Why this matters" / "In plain English" cards
    concept-graph.js    # Interactive force-directed concept map (supplementary)
  utils/
    svg-helpers.js      # SVG creation utilities
    layout.js           # Force-directed layout for concept map
    color.js            # Color palette utilities
    dom.js              # DOM manipulation helpers
    text-analysis.js    # NLP-lite: sentence splitting, term extraction, readability scoring
```

## UI Layout

### Reading Mode (primary)
```
+------------------------------------------+
|  Header: App title + paper title         |
+----------+-------------------------------+
|          |                               |
|  Paper   |   Section Reader              |
|  Outline |   (paper text with inline     |
|  (left)  |    jargon highlights,         |
|          |    selectable for explain)     |
|          |                               |
+----------+-------------------------------+
|  Insight Panel: explanation / analogy /  |
|  glossary / "why this matters"           |
+------------------------------------------+
```

- **Left sidebar**: Section outline (clickable navigation)
- **Main panel**: The actual paper text, rendered with:
  - Jargon terms highlighted (hover for definition)
  - Text is selectable — selecting triggers the explanation panel
  - Section insights shown above the text ("Why this matters: ...")
- **Bottom panel**: Context-sensitive — shows explanations, analogies, glossary depending on what the user interacts with

### Concept Map Mode (secondary, toggle)
- Accessible via a tab/toggle in the main panel
- Force-directed graph of key concepts (already built)
- Clicking a node returns to reading mode focused on that concept

## Design Principles

1. **Understanding over visualization** — Explain, don't just rearrange words spatially
2. **Progressive disclosure** — Start simple, let users dig deeper
3. **Grounded in the paper** — Explanations reference the actual text, not generic definitions
4. **Real-world connections** — Every abstract concept should map to something familiar
5. **Interactive, not passive** — Users drive what gets explained by selecting and asking
6. **Accessibility** — Keyboard navigable, sufficient contrast, screen reader labels
7. **Offline-capable** — Core functionality runs client-side

## Explanation Strategy

Since this is vanilla JS with no backend/API:

- **Heuristic explanations**: Use pattern matching, term frequency, sentence position, and pre-built explanation templates for common academic patterns
- **Jargon detection**: Maintain a curated dictionary of common technical terms across domains (ML, biology, physics, etc.) + detect acronyms and defined-in-paper terms
- **Analogy generation**: Use template-based analogies for common patterns (e.g., "X processes Y" → "like a factory that takes raw materials and produces finished goods")
- **Readability scoring**: Flesch-Kincaid or similar to identify which sentences need simplification

Future enhancement: optional API integration for LLM-powered explanations (but core must work offline).

## Conventions

- **No classes for JS hooks** — Use `data-*` attributes for JS bindings, classes for styling only
- **Event delegation** — Attach listeners to container elements, not individual items
- **Module pattern** — Each JS file exports a single default object or function
- **CSS custom properties** — All colors, spacing, and typography via `var(--token-name)`
- **Mobile responsive** — Sidebar collapses to top nav on small screens
- **State in JS, not DOM** — Maintain app state in a plain object, render from state

## Development

```bash
# Dev server → https://paperlens.localhost
pnpm dev
```

No build step. Edit files, refresh browser.

## Key UX Principle: Persistent, Not Ephemeral

Like AlgoViz where every algorithm has a persistent visual walkthrough, Research Made Easy should present **persistent annotations** alongside the paper text. Explanations, analogies, and insights should be always-visible companions to the text, not temporary pop-ups that vanish on click-away.

Each section should have its annotations rendered inline — "In plain English", "Why this matters", "Key contribution" — always visible as part of the reading experience. The user scrolls through a pre-annotated paper, not an interactive quiz.

## What's Built

- App shell with 3-panel layout (header, sidebar, main, detail)
- CSS design system with light/dark mode
- Paper parser detecting numbered, ALL CAPS, common, and underlined headings
- Paper input with drag-and-drop .txt support
- Sidebar outline navigation with active state and jargon density indicators
- Section Reader: readable text with inline jargon detection (150+ terms, dotted underline + hover tooltip with definition + domain badge)
- Reader/Concept Map toggle (concept graph is supplementary)
- Highlight-to-Explain: select text → bottom panel shows explanation with depth selector (ELI5/Standard/Technical) and real-world analogy
- Heuristic explainer engine with pattern rewriting, jargon substitution, analogy templates
- Force-directed concept graph (SVG, draggable, hover highlights)

## What's Next

1. **Section Insights (persistent)** — Always-visible annotation cards per section: "In plain English", "Why this matters", "Key contribution". Rendered inline above/below each section's text, not in a separate panel.
2. **Glossary View** — Full paper glossary built from all detected jargon terms across all sections
3. **Paragraph-level annotations** — Each paragraph gets its own persistent simplified version (togglable)
