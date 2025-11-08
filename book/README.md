# Drop Test: How a Game Boy Became the Most Reliable Tool in Geology

**Working Title:** Drop Test: How a Game Boy Became the Most Reliable Tool in Geology
**Status:** Planning / Research Phase
**Build System:** Pandoc + Make (with Quarto support option)

---

## About This Book

This book tells the story of GeoCalc Pocket and GeoStructure Systems—a small company that built professional geological instruments on Nintendo Game Boy hardware. It's a tale of constraint-driven innovation, field geology, stubborn engineers, and the unlikely intersection of video games and science.

**Genre:** Tech history / Business narrative (think *Soul of a New Machine* meets *The Innovators*)

**Target Audience:**
- Geologists and earth scientists
- Retro computing enthusiasts
- Indie developers and entrepreneurs
- Readers interested in niche technology stories
- Engineering and business students

**Themes:**
- Constraint-driven innovation
- Field reliability vs. sophisticated features
- Finding unconventional solutions
- Building for a niche market
- The human side of technology development

---

## Book Structure

### Part I: The Garage Years (1987-1992)
**Chapters 1-3**
- Company founding and early struggles
- The FieldLogger disasters
- Bob's nephew drops a Game Boy
- The revelation and pivot

### Part II: Development Hell (1992-1994)
**Chapters 4-7**
- Learning Z80 assembly the hard way
- The tile crisis and technical constraints
- Manufacturing nightmare
- Launch and the Nintendo letter

### Part III: The Field Years (1994-2000)
**Chapters 8-11**
- Field testing and validation
- The thesis save story
- Evolution of the product line
- Competition and market challenges

### Part IV: Legacy (2000-2013)
**Chapters 12-15**
- Platform transitions (Palm, smartphones)
- The passion project (AeroGeo 3D)
- Impact on geology education
- What it all meant

### Part V: Technical Deep Dive (Appendices)
**Appendices A-E**
- Game Boy architecture
- Stereonet mathematics
- Code samples
- Product specifications
- Timeline

---

## Directory Structure

```
book/
├── README.md (this file)
├── metadata.yaml (book metadata for pandoc)
├── Makefile (build automation)
├── build.sh (alternative build script)
├── chapters/ (book chapters in Markdown)
│   ├── 00_preface.md
│   ├── 01_the_garage_years.md
│   ├── 02_revelation.md
│   ├── 03_prototype.md
│   ├── 04_development_hell.md
│   ├── 05_launch.md
│   ├── 06_field_testing.md
│   ├── 07_evolution.md
│   ├── 08_the_thesis_save.md
│   ├── 09_competition.md
│   ├── 10_transitions.md
│   ├── 11_passion_project.md
│   ├── 12_legacy.md
│   ├── 13_what_they_built.md
│   ├── 99_epilogue.md
│   └── appendices/
│       ├── a_technical_specs.md
│       ├── b_mathematics.md
│       ├── c_code_samples.md
│       ├── d_timeline.md
│       └── e_sources.md
├── references/ (reference materials)
│   ├── interviews/ (interview notes, if any)
│   ├── documents/ (historical documents)
│   ├── specifications/ (technical specs)
│   └── bibliography.bib (citations)
├── images/ (figures, photos, diagrams)
│   ├── photos/
│   ├── diagrams/
│   ├── screenshots/
│   └── charts/
├── templates/ (pandoc/LaTeX templates)
│   ├── book_template.tex
│   ├── epub_template.html
│   └── pdf_metadata.yaml
├── tools/ (build scripts and utilities)
│   ├── preprocessor.py (if needed)
│   └── wordcount.sh
└── output/ (generated files)
    ├── pdf/
    ├── epub/
    ├── html/
    └── docx/
```

---

## Build System

### Option 1: Pandoc + Make (Recommended)

**Why Pandoc:**
- Mature, stable, widely used
- Excellent PDF output (via LaTeX)
- Great EPUB support
- Flexible templating
- Large community

**Installation:**
```bash
# Ubuntu/Debian
sudo apt-get install pandoc pandoc-citeproc texlive-full

# macOS
brew install pandoc pandoc-citeproc
brew install --cask mactex

# Arch Linux
sudo pacman -S pandoc texlive-most
```

**Build Commands:**
```bash
# Build all formats
make all

# Build specific format
make pdf
make epub
make html
make docx

# Clean output
make clean

# Word count
make wordcount
```

### Option 2: Quarto (Alternative, Modern)

**Why Quarto:**
- Modern, actively developed
- Better figure/table handling
- Built-in cross-referencing
- Nice HTML output
- Good for code examples

**Installation:**
```bash
# Download from https://quarto.org/docs/get-started/
# Or via package manager
```

**Build with Quarto:**
```bash
quarto render book.qmd --to pdf
quarto render book.qmd --to epub
quarto render book.qmd --to html
```

### Recommended: Start with Pandoc, Migrate to Quarto if Needed

---

## Output Formats

### PDF (Print-Ready)
- **Engine:** Pandoc → LaTeX → PDF
- **Template:** Custom LaTeX template (professional book layout)
- **Features:**
  - Page numbers
  - Chapter headings
  - Table of contents
  - Footnotes
  - Index (if needed)
- **Target:** 6×9" trade paperback format

### EPUB (E-Reader)
- **Engine:** Pandoc → EPUB3
- **Template:** Custom HTML/CSS
- **Features:**
  - Reflowable text
  - Embedded images
  - Linked table of contents
  - Metadata (author, title, ISBN)
- **Target:** Kindle, Apple Books, Kobo

### HTML (Web Version)
- **Engine:** Pandoc → HTML5
- **Template:** Custom CSS (responsive design)
- **Features:**
  - Single-page or multi-page
  - Navigation menu
  - Syntax highlighting (code samples)
  - Interactive elements (if desired)
- **Target:** Free online reading

### DOCX (Editing)
- **Engine:** Pandoc → DOCX
- **Purpose:** For editors, reviewers, beta readers
- **Features:** Comments, track changes

---

## Writing Workflow

### 1. Research & Notes
- Consolidate information from design document
- Identify gaps, research needed
- Create chapter outlines

### 2. First Draft
- Write chapters in Markdown
- Don't worry about formatting
- Focus on story and content

### 3. Revision
- Structural edits
- Character development
- Technical accuracy review
- Fact-checking

### 4. Copy Editing
- Grammar, spelling, punctuation
- Consistency (style guide)
- Readability

### 5. Layout & Design
- Finalize templates
- Format code samples
- Prepare images/diagrams
- Build final PDF/EPUB

### 6. Proofreading
- Final read-through
- Check formatting in all outputs
- Verify links, references

---

## Markdown Conventions

### Chapter Files
```markdown
# Chapter Title {#chapter-id}

Opening paragraph...

## Section Heading {#section-id}

Content...

### Subsection

More content...
```

### Cross-References
```markdown
See [Chapter 3](#chapter-id) for details.
As shown in [Figure 4.2](#fig-stereonet)...
```

### Figures
```markdown
![Stereonet projection example](images/stereonet.png){#fig-stereonet width=80%}
```

### Code Blocks
````markdown
```c
// Code example
void plot_point(int x, int y) {
    // implementation
}
```
````

### Footnotes
```markdown
This is a statement.^[This is a footnote.]
```

### Citations
```markdown
According to @groshong1997structural, ...

Multiple citations [@chen1994geocalc; @kuwahara1995field]
```

---

## Metadata (metadata.yaml)

```yaml
---
title: "Drop Test"
subtitle: "How a Game Boy Became the Most Reliable Tool in Geology"
author: "Your Name"
date: "2025"
lang: en-US
documentclass: book
geometry: "paperwidth=6in, paperheight=9in, margin=0.75in"
fontsize: 11pt
linestretch: 1.2
toc: true
toc-depth: 2
lot: false
lof: false
bibliography: references/bibliography.bib
csl: chicago-author-date.csl
---
```

---

## Makefile (Example)

```makefile
# Variables
CHAPTERS := $(wildcard chapters/*.md)
APPENDICES := $(wildcard chapters/appendices/*.md)
ALL_MD := $(CHAPTERS) $(APPENDICES)
METADATA := metadata.yaml
OUTPUT_DIR := output
IMAGES := images

# Targets
.PHONY: all pdf epub html docx clean wordcount

all: pdf epub html

pdf: $(OUTPUT_DIR)/pdf/book.pdf

epub: $(OUTPUT_DIR)/epub/book.epub

html: $(OUTPUT_DIR)/html/book.html

docx: $(OUTPUT_DIR)/docx/book.docx

# Build PDF
$(OUTPUT_DIR)/pdf/book.pdf: $(METADATA) $(ALL_MD)
	mkdir -p $(OUTPUT_DIR)/pdf
	pandoc $(METADATA) $(CHAPTERS) $(APPENDICES) \
		-o $@ \
		--pdf-engine=xelatex \
		--toc \
		--number-sections \
		--filter pandoc-citeproc \
		--template=templates/book_template.tex \
		--resource-path=.:$(IMAGES)

# Build EPUB
$(OUTPUT_DIR)/epub/book.epub: $(METADATA) $(ALL_MD)
	mkdir -p $(OUTPUT_DIR)/epub
	pandoc $(METADATA) $(CHAPTERS) $(APPENDICES) \
		-o $@ \
		--toc \
		--epub-cover-image=images/cover.jpg \
		--resource-path=.:$(IMAGES)

# Build HTML
$(OUTPUT_DIR)/html/book.html: $(METADATA) $(ALL_MD)
	mkdir -p $(OUTPUT_DIR)/html
	pandoc $(METADATA) $(CHAPTERS) $(APPENDICES) \
		-o $@ \
		--toc \
		--standalone \
		--self-contained \
		--css=templates/style.css \
		--resource-path=.:$(IMAGES)

# Build DOCX
$(OUTPUT_DIR)/docx/book.docx: $(METADATA) $(ALL_MD)
	mkdir -p $(OUTPUT_DIR)/docx
	pandoc $(METADATA) $(CHAPTERS) $(APPENDICES) \
		-o $@ \
		--toc \
		--resource-path=.:$(IMAGES)

# Word count
wordcount:
	@wc -w $(CHAPTERS) $(APPENDICES) | tail -1

# Clean generated files
clean:
	rm -rf $(OUTPUT_DIR)/*
```

---

## Getting Started

### 1. Set Up Environment
```bash
# Install pandoc
sudo apt-get install pandoc pandoc-citeproc texlive-full

# OR on macOS
brew install pandoc pandoc-citeproc
brew install --cask mactex
```

### 2. Create Initial Chapters
```bash
# Copy content from design document into chapter files
# Start with chapter outlines
```

### 3. Test Build
```bash
# Try building PDF
make pdf

# Check output
open output/pdf/book.pdf  # macOS
xdg-open output/pdf/book.pdf  # Linux
```

### 4. Iterate
- Write chapters in Markdown
- Build frequently to check formatting
- Revise and refine

---

## Style Guide

### Voice & Tone
- **Narrative:** Story-driven, like *Soul of a New Machine*
- **Technical detail:** Balanced—accessible but not dumbed-down
- **Human focus:** Characters matter as much as technology
- **Humor:** Present but not forced (Bob's quotes, field mishaps)
- **Respect:** For the craft, the people, the geology

### Chapter Structure
- Opening hook (anecdote, quote, scene)
- Context and background
- Main narrative
- Technical details (as needed, not overwhelming)
- Character moments
- Conclusion that leads to next chapter

### Technical Sections
- Explain concepts clearly (assume intelligent but non-expert reader)
- Use analogies when helpful
- Code samples should be minimal, well-commented
- Diagrams for complex ideas
- Sidebars for deep dives

---

## To-Do List (Book Project)

- [ ] Complete chapter outlines
- [ ] Extract content from design document into chapters
- [ ] Research gaps (interviews, if possible)
- [ ] Create diagrams and figures
- [ ] Write first draft of all chapters
- [ ] Technical review (geology accuracy)
- [ ] Structural edit (narrative flow)
- [ ] Copy edit (grammar, style)
- [ ] Design book cover
- [ ] Finalize LaTeX template
- [ ] Build final PDF
- [ ] Build EPUB
- [ ] Beta reader review
- [ ] Final proofreading
- [ ] Publication plan

---

## Resources

### Pandoc Documentation
- https://pandoc.org/MANUAL.html
- https://pandoc.org/epub.html
- https://pandoc.org/demos.html

### Book Writing with Pandoc
- "Sustainable Authorship in Plain Text" (kieranhealy.org)
- "Writing a Book with Pandoc" (kdheepak.com)
- Pandoc Book Template (GitHub)

### LaTeX Book Classes
- Memoir class (comprehensive)
- KOMA-Script (scrbook)
- Tufte-book (beautiful margins)

### Style Guides
- *Chicago Manual of Style* (standard for non-fiction)
- *On Writing Well* by William Zinsser
- *The Elements of Style* by Strunk & White

---

## Contact & Collaboration

**Author:** [Your Name]
**Email:** [Your Email]
**GitHub:** [Repository URL]

**Beta Readers:** Contact if interested in reviewing drafts
**Technical Reviewers:** Geologists and Game Boy programmers welcome

---

## License

**Book Content:** Copyright © 2025 [Your Name]. All rights reserved.

**Code Samples:** MIT License (free to use)

**Build System:** MIT License (templates, Makefile, etc.)

---

**Last Updated:** 2025-11-08
**Status:** Research and planning phase
**Estimated Completion:** TBD
