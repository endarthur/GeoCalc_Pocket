# Build Instructions for "Drop Test"

This document explains how to build the book in various formats.

---

## Prerequisites

### Required Software

#### 1. Pandoc
**Installation:**

**Ubuntu/Debian:**
```bash
sudo apt-get update
sudo apt-get install pandoc pandoc-citeproc
```

**macOS:**
```bash
brew install pandoc pandoc-citeproc
```

**Arch Linux:**
```bash
sudo pacman -S pandoc
```

**Windows:**
- Download installer from https://pandoc.org/installing.html

#### 2. LaTeX (for PDF generation)

**Ubuntu/Debian:**
```bash
sudo apt-get install texlive-full
# Or minimal install:
sudo apt-get install texlive-latex-base texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended
```

**macOS:**
```bash
brew install --cask mactex
# Or minimal:
brew install --cask basictex
```

**Arch Linux:**
```bash
sudo pacman -S texlive-most texlive-lang
```

**Windows:**
- Install MiKTeX from https://miktex.org/

#### 3. Make (build automation)

**Ubuntu/Debian/Arch:**
```bash
# Usually pre-installed
sudo apt-get install make  # if not installed
```

**macOS:**
```bash
xcode-select --install  # Installs command-line tools including make
```

**Windows:**
- Install via MSYS2 or WSL
- Or use build.sh script (Git Bash)

### Verify Installation

```bash
# Check pandoc
pandoc --version

# Check LaTeX
xelatex --version

# Check make
make --version
```

---

## Quick Start

### Build All Formats
```bash
cd book/
make all
```

This will generate:
- `output/pdf/book.pdf`
- `output/epub/book.epub`
- `output/html/book.html`

### Build Specific Format

```bash
# PDF only
make pdf

# EPUB only
make epub

# HTML only
make html

# DOCX (for editing)
make docx
```

### View Output

**macOS:**
```bash
open output/pdf/book.pdf
```

**Linux:**
```bash
xdg-open output/pdf/book.pdf
```

**Windows:**
```bash
start output/pdf/book.pdf
```

---

## Detailed Build Options

### PDF Generation

**Command:**
```bash
make pdf
```

**What it does:**
1. Reads `metadata.yaml` for book settings
2. Concatenates all chapter files in order
3. Runs pandoc with XeLaTeX engine
4. Generates table of contents
5. Numbers sections
6. Applies book template (if present)
7. Outputs to `output/pdf/book.pdf`

**Custom options:**
```bash
# Build with custom template
pandoc metadata.yaml chapters/*.md \
  -o output/pdf/book.pdf \
  --pdf-engine=xelatex \
  --template=templates/custom_template.tex

# Build with different page size
pandoc metadata.yaml chapters/*.md \
  -o output/pdf/book.pdf \
  --pdf-engine=xelatex \
  -V geometry:paperwidth=8.5in \
  -V geometry:paperheight=11in
```

### EPUB Generation

**Command:**
```bash
make epub
```

**What it does:**
1. Reads `metadata.yaml`
2. Concatenates chapters
3. Generates EPUB3 format
4. Includes table of contents
5. Embeds cover image (if present)
6. Outputs to `output/epub/book.epub`

**Custom options:**
```bash
# Build with custom CSS
pandoc metadata.yaml chapters/*.md \
  -o output/epub/book.epub \
  --css=templates/epub_style.css \
  --epub-cover-image=images/cover.jpg
```

### HTML Generation

**Command:**
```bash
make html
```

**What it does:**
1. Reads `metadata.yaml`
2. Concatenates chapters
3. Generates standalone HTML file
4. Embeds images (self-contained)
5. Applies CSS styling
6. Outputs to `output/html/book.html`

**Custom options:**
```bash
# Multi-page HTML
pandoc metadata.yaml chapters/*.md \
  -o output/html/ \
  --standalone \
  --split-level=1

# With custom CSS
pandoc metadata.yaml chapters/*.md \
  -o output/html/book.html \
  --css=templates/web_style.css
```

---

## Word Count

Check progress:
```bash
make wordcount
```

Output example:
```
Word count by chapter:
00_preface.md: 1234
01_the_garage_years.md: 3456
02_revelation.md: 4567
...
---
Total words: 45678
```

---

## Cleaning Up

Remove all generated files:
```bash
make clean
```

This deletes everything in `output/` directory.

---

## Manual Build (Without Make)

If you can't use Make, here are the direct pandoc commands:

### PDF
```bash
pandoc metadata.yaml chapters/*.md chapters/appendices/*.md \
  -o output/pdf/book.pdf \
  --pdf-engine=xelatex \
  --toc \
  --number-sections \
  --resource-path=.:images
```

### EPUB
```bash
pandoc metadata.yaml chapters/*.md chapters/appendices/*.md \
  -o output/epub/book.epub \
  --toc \
  --epub-cover-image=images/cover.jpg \
  --resource-path=.:images
```

### HTML
```bash
pandoc metadata.yaml chapters/*.md chapters/appendices/*.md \
  -o output/html/book.html \
  --toc \
  --standalone \
  --self-contained \
  --resource-path=.:images
```

### DOCX
```bash
pandoc metadata.yaml chapters/*.md chapters/appendices/*.md \
  -o output/docx/book.docx \
  --toc \
  --resource-path=.:images
```

---

## Troubleshooting

### PDF Build Fails

**Error:** "xelatex not found"
- **Solution:** Install LaTeX (see Prerequisites)

**Error:** "Package xxx.sty not found"
- **Solution:** Install missing LaTeX packages:
  ```bash
  # Ubuntu/Debian
  sudo apt-get install texlive-latex-extra

  # macOS
  sudo tlmgr install <package-name>
  ```

**Error:** Unicode characters not rendering
- **Solution:** Use XeLaTeX (already specified in Makefile)
- Ensure metadata specifies: `pdf-engine: xelatex`

### EPUB Build Issues

**Error:** Cover image not embedding
- **Solution:** Check image path in metadata.yaml
- Ensure image exists: `images/cover.jpg`

**Error:** Images not displaying
- **Solution:** Check resource-path setting
- Verify image paths in markdown are correct

### HTML Build Issues

**Error:** Images missing
- **Solution:** Use `--self-contained` flag (already in Makefile)
- Or copy images/ directory to output/html/

**Error:** CSS not applying
- **Solution:** Check CSS path in metadata or command

### Make Command Not Found (Windows)

**Solution 1:** Install Make via MSYS2
```bash
# In MSYS2 terminal
pacman -S make
```

**Solution 2:** Use Windows Subsystem for Linux (WSL)

**Solution 3:** Run pandoc commands manually (see above)

---

## Advanced Topics

### Custom LaTeX Template

1. Export default template:
```bash
pandoc -D latex > templates/book_template.tex
```

2. Modify template (add custom headers, footers, etc.)

3. Use in build:
```bash
pandoc metadata.yaml chapters/*.md \
  -o output/pdf/book.pdf \
  --template=templates/book_template.tex
```

### Bibliography & Citations

1. Create `references/bibliography.bib`:
```bibtex
@book{groshong1997structural,
  title={Structural Geology of Folds and Faults},
  author={Groshong, Richard H.},
  year={1997},
  publisher={Cambridge University Press}
}
```

2. Cite in chapters:
```markdown
According to @groshong1997structural, ...
```

3. Ensure metadata includes:
```yaml
bibliography: references/bibliography.bib
csl: chicago-author-date.csl
```

4. Build automatically includes citations

### Cross-References

```markdown
See [Chapter 3](#chapter-3) for details.

![Stereonet example](images/stereonet.png){#fig:stereonet}

As shown in Figure [@fig:stereonet]...
```

### Conditional Content

Include content only in certain formats:
```markdown
::: {.content-only format=pdf}
This appears only in PDF output.
:::

::: {.content-only format=html}
This appears only in HTML output.
:::
```

---

## Automation & CI/CD

### GitHub Actions (Example)

Create `.github/workflows/build-book.yml`:
```yaml
name: Build Book

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install Pandoc
        run: |
          sudo apt-get update
          sudo apt-get install -y pandoc texlive-xelatex
      - name: Build PDF
        run: |
          cd book
          make pdf
      - name: Upload Artifact
        uses: actions/upload-artifact@v2
        with:
          name: book-pdf
          path: book/output/pdf/book.pdf
```

---

## Alternative Build Systems

### Quarto (Modern Alternative)

**Install:**
```bash
# Download from https://quarto.org/
```

**Create `_quarto.yml`:**
```yaml
project:
  type: book

book:
  title: "Drop Test"
  author: "Author Name"
  chapters:
    - chapters/00_preface.md
    - chapters/01_the_garage_years.md
    # ... etc
```

**Build:**
```bash
quarto render
```

### Bookdown (R-based)

For academic books with lots of figures and code.

**Not recommended for this project** (narrative non-fiction), but available.

---

## Publishing Checklist

Before final publication:

- [ ] Complete all chapters
- [ ] Proofread all content
- [ ] Generate final PDF
- [ ] Generate final EPUB
- [ ] Test EPUB on Kindle, Apple Books
- [ ] Verify all images display correctly
- [ ] Check table of contents
- [ ] Validate hyperlinks
- [ ] Add cover image
- [ ] Update metadata (ISBN, copyright, etc.)
- [ ] Run final word count
- [ ] Generate print-ready PDF (if applicable)

---

## Additional Resources

- **Pandoc Manual:** https://pandoc.org/MANUAL.html
- **Pandoc Demos:** https://pandoc.org/demos.html
- **LaTeX Documentation:** https://www.latex-project.org/help/documentation/
- **EPUB Spec:** https://www.w3.org/TR/epub-33/

---

## Support

If you encounter issues:

1. Check this document's troubleshooting section
2. Review Pandoc documentation
3. Search GitHub issues for similar problems
4. Ask on Pandoc mailing list or Discord

---

**Last Updated:** 2025-11-08
