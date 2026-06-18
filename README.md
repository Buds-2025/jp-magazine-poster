# jp-magazine-poster

Japanese magazine style poster skill for generating fixed-layout HTML and PNG posters from a topic, source text, or provided images.

## What It Does

- Uses 8 bundled vertical poster layouts in white and midnight themes.
- Fills title, golden quote, detail text, and image zones without changing the visual layout rules.
- Exports one HTML file and one high-resolution PNG per poster.
- Supports user-provided images, generated bitmap images, and rasterized PNG fallback art.
- Validates punctuation, quote style, text overflow, image zones, SVG leakage, and poster dimensions.

## Use Rules

Before generating posters, ask the user exactly one form-style intake round:

- Summarize the known request.
- Ask about theme, poster count, image method, template selection, copy tone, output path, and constraints.
- Do not keep asking follow-up questions.
- If information is still missing after that one round, choose reasonable defaults and record assumptions.

Final rendered HTML must not reference `.svg` files or `data:image/svg+xml`. Any SVG-source artwork must be rasterized into PNG before being placed into image zones.

Image sourcing priority is strict:

1. Use user-provided image paths or URLs.
2. Call a system-available model or tool to generate theme-matched bitmap images and save them under the output directory.
3. Draw polished SVG artwork only when user images are absent and bitmap generation is unavailable, fails, or cannot return project-local files. Record the reason before using this fallback.

## Basic Usage

Install dependencies:

```bash
npm install
```

Render posters:

```bash
node scripts/render_poster.js --spec input.json --out output/<run-name>
```

Validate output:

```bash
node scripts/validate_poster.js --html output/<run-name>/*.html
```

## Directory Tree

```text
jp-magazine-poster/
|-- SKILL.md
|-- README.md
|-- package.json
|-- package-lock.json
|-- agents/
|   `-- openai.yaml
|-- assets/
|   `-- templates/
|       |-- white/
|       |   `-- index.html
|       `-- midnight/
|           `-- index.html
|-- references/
|   `-- template-contract.md
`-- scripts/
    |-- annotate_templates.js
    |-- render_poster.js
    `-- validate_poster.js
```

`node_modules/` is intentionally not committed. Recreate it with `npm install`.
