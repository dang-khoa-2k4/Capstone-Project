# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A Vietnamese-language LaTeX capstone thesis on robot motion planning, focused on the Graph of Convex Sets (GCS) framework combined with Optimal Control Problems (OCP) using Multiple Shooting. The thesis is for HCMUT (Ho Chi Minh City University of Technology), Semester 252.

## Build Commands

There is no Makefile. Compile the thesis using standard LaTeX tools from the `Report Source/` directory:

```bash
# Full compilation (run twice to resolve cross-references)
pdflatex main.tex
biber main
pdflatex main.tex
pdflatex main.tex

# Or with latexmk (recommended — handles reruns automatically)
latexmk -pdf main.tex

# Clean build artifacts
latexmk -c
```

The output is `main.pdf`. Build artifacts (`.aux`, `.bbl`, `.bcf`, `.log`, `.synctex.gz`, etc.) are generated in the root directory alongside `main.tex`.

## Document Architecture

The root entry point is `main.tex`. Each chapter lives in its own subdirectory with a `main.tex` that `\input`s subsection files:

```
main.tex                        ← root document (class, margins, packages, chapter includes)
lib.sty                         ← all custom macros, colors, theorem envs, code highlighting
refs.bib                        ← BibTeX bibliography (IEEE style via biblatex)
introduction/
theoretical-background/         ← convex optimization, graph theory, motion planning, OCP, MIP
related-work/
proposed-method/                ← problem statement, ACD, VCC, GCS, relaxation
experiments/
future-plan/
image/                          ← all figures referenced in text
```

Chapters are included in `main.tex` via `\include{<chapter>/main}`. Subsections within a chapter are pulled in with `\input{<subsection>.tex}` from the chapter's own `main.tex`.

## Key Files

- **`lib.sty`** — Import this at the top of any new standalone `.tex` file. It provides all theorem environments (`definition`, `theorem`, `lemma`, `proposition`), algorithm formatting, TikZ graph libraries, code listings (Python/C/R/Matlab), and Vietnamese language support.
- **`refs.bib`** — Add new references here. Citations use `\cite{}` with the biblatex IEEE backend.
- **`main.tex`** — Document-level settings: margins, font size, line spacing, hyperref colors, title page, and chapter ordering. Edit here to add/remove chapters.

## Language

The thesis is written in Vietnamese. All chapter titles, section headings, captions, and prose are in Vietnamese. When editing `.tex` files, maintain Vietnamese text. LaTeX labels, commands, and `\ref`/`\cite` keys remain in English.

## Figures

All images go in the `image/` directory. Reference them as `\includegraphics{image/<filename>}`. PNG is the predominant format for experimental results; PDF/EPS for vector diagrams.

## Chapters in Scope

- **proposed-method/** — the core research contribution (ACD, VCC, GCS, perspective relaxation). Most active development happens here.
- **experiments/** — simulation results comparing decomposition methods and trajectory optimization.
- `papers/RP/report_v4.tex` is an earlier standalone draft; `main.tex` is the authoritative document.
