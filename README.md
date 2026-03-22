# latex-catppuccin-academic

A modular LaTeX template for academic documents — lecture notes, theses, technical reports — styled with the [Catppuccin](https://catppuccin.com) color palette.

![Preview](figs/preview.png)
<!-- Uncomment the line above once you add a preview screenshot -->

## Features

- **Catppuccin theming throughout**: titles, hyperlinks, code listings, algorithm blocks, and `tcolorbox` environments all follow the [Catppuccin style guide](https://catppuccin.com/style-guide). Multiple flavor files included (`catppuccin-palette.tex`, `catppuccin-latte.tex`, `custom-theme.tex`).
- **Modular architecture**: configuration split into independent files under `config/` — geometry, packages, macros, listings, algorithms, titles, hyperref, and tcolorbox — so each aspect can be modified without touching the rest.
- **Custom math and physics macros**: common operators (`\argmin`, `\argmax`, `\tr`, `\rank`), linear algebra shorthands (`\norm`, `\inner`, `\T`), and physics notation (`\SDE`, `\score`, `\FP`, `\Lap`, `\Div`) ready to use.
- **Catppuccin-styled code listings**: syntax highlighting for Python, C++, and Julia following the official palette mapping (keywords → Mauve, strings → Green, comments → Overlay2, etc.).
- **Catppuccin-styled algorithms**: `algorithm2e` blocks with colored keywords, line numbers, vertical connector lines, and caption rules, all matching the palette.
- **Environment system**: a generator-based architecture (`environments/generator.tex`) for theorem-like environments (definitions, theorems, examples, remarks, etc.) with consistent `tcolorbox` styling.
- **Metadata-driven frontmatter**: a single `metadata.tex` file controls title, subtitle, author, date, dedication, epigraph, acknowledgements, and PDF metadata — no need to edit `main.tex`.
- **Spanish language support**: configured with `babel` (spanish, es-noshorthands) out of the box. Easily switchable to English or other languages.
- **Book class structure**: `\chapter`, `\section` hierarchy with table of contents, bibliography (`natbib`), appendices, and frontmatter/backmatter separation.

## Project structure

```
.
├── main.tex                  # Root document
├── metadata.tex              # Document metadata (title, author, dates, etc.)
├── references.bib            # BibTeX bibliography
├── config/
│   ├── packages.tex          # Package loading
│   ├── geometry.tex          # Page layout, headers/footers
│   ├── macros.tex            # Math and physics macros
│   ├── tcolorbox-config.tex  # Base tcolorbox style
│   ├── titles-config.tex     # Chapter/section title formatting
│   ├── listings-catppuccin.tex  # Code listing styles
│   ├── algorithms.tex        # Algorithm2e styling
│   └── hyperref.tex          # Hyperlink configuration
├── environments/
│   ├── generator.tex         # Environment factory (tcolorbox-based)
│   ├── instances.tex         # Concrete environments (theorem, definition, etc.)
│   └── specials.tex          # Special-purpose environments
├── themes/
│   ├── catppuccin-palette.tex   # Main Catppuccin color definitions
│   ├── catppuccin-latte.tex     # Latte (light) flavor
│   ├── custom-theme.tex         # User-customizable theme
│   └── tema-base.tex            # Base theme layer
├── frontmatter/
│   ├── titlepage.tex         # Title page layout
│   ├── preliminary.tex       # Dedication, epigraph, acknowledgements
│   └── preface.tex           # Preface
├── chapters/
│   └── chapter1.tex          # Example chapter
├── backmatter/
│   └── appendixA.tex         # Example appendix
├── figs/                     # Figures directory
└── code/                     # Code snippets directory
```

## Quick start

1. **Clone the repository**
   ```bash
   git clone git@github.com:Zessinthel/latex-catppuccin-academic.git
   cd latex-catppuccin-academic
   ```

2. **Edit `metadata.tex`** with your document information (title, author, etc.)

3. **Choose a theme** by editing the `\input{themes/...}` line in `main.tex`

4. **Compile**
   ```bash
   pdflatex main.tex
   bibtex main
   pdflatex main.tex
   pdflatex main.tex
   ```
   Or with `latexmk`:
   ```bash
   latexmk -pdf main.tex
   ```

## Requirements

A full TeX Live or MiKTeX installation with the following packages (all standard): `tcolorbox`, `algorithm2e`, `listings`, `tikz`, `natbib`, `titlesec`, `fancyhdr`, `hyperref`, `microtype`, `booktabs`, `inconsolata`.

## Customization

**Switching color flavors**: replace the `\input{themes/catppuccin-palette.tex}` line in `main.tex` with any other theme file, or create your own by defining the `Ctp*` color commands.

**Adding chapters**: create a new `.tex` file in `chapters/` and add `\include{chapters/chapterN}` in `main.tex`.

**Extending macros**: add commands to `config/macros.tex`. The existing set covers linear algebra, optimization, and stochastic calculus notation.

## License

This template is released under the [MIT License](LICENSE). The Catppuccin color palette is © Catppuccin contributors, used under their license terms.

## Author

**Antonio Casanova** — [GitHub](https://github.com/Zessinthel) · [GitLab](https://gitlab.com/Zessinthel)
