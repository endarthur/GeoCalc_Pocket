# Professional Book Template

## Overview

This is a **custom LaTeX template** designed specifically for "Drop Test" that produces **professional, publisher-quality** typography suitable for print or digital distribution.

---

## Design Features

### Typography & Layout

**Fonts:**
- **Body text:** Palatino (classic, readable serif)
- **Headers:** Helvetica (clean sans-serif)
- **Code:** Courier New (monospace for technical content)
- **Old-style numerals** in body text for elegance

**Page Layout (6×9" Trade Paperback):**
- **Two-sided printing** (different left/right margins)
- **Inner margin:** 0.875" (wider for binding)
- **Outer margin:** 0.625" (narrower, more text per page)
- **Professional binding gutter** for perfect binding
- **Chapters open on right-hand pages** (publishing standard)

**Paragraph Formatting:**
- 1.5em indentation (standard book style)
- First paragraph after headings is flush-left (no indent)
- No space between paragraphs (classic book style)
- Widow and orphan control (no single lines at page breaks)

### Chapter Design

**Beautiful Chapter Openings:**
```
[Large chapter number in gray, 72pt]

CHAPTER TITLE
─────────────────────────
First paragraph starts here...
```

Features:
- Dramatic **72pt chapter numbers** in dark gray
- **Sans-serif chapter titles** (Huge size)
- Horizontal rule under title
- Plenty of white space

**Optional drop caps** at chapter starts:
```
L ettrine-style drop caps (3 lines tall)
  for the first letter of chapters
```

### Running Headers

**Left pages (even):** Chapter title (italics)
**Right pages (odd):** "Drop Test" (book title)

Clean, minimal style with no header rules.

Page numbers in footers (outside corners).

### Part Dividers

For the book's four parts:
```
        PART I
    ────────────
  The Garage Years
```

Centered, dramatic, lots of white space.

### Table of Contents

**Clean, professional styling:**
- Sans-serif chapter titles (bold)
- Regular sections with dotted leaders
- Page numbers aligned right
- Two-level depth (chapters + sections)

### Code Blocks (for Appendix)

Technical code with professional styling:
- Light gray background
- Line numbers
- Syntax-aware formatting
- Proper monospace font
- Frame border

Example:
```c
// Code looks like this
void function() {
    // Professional formatting
}
```

### Front Matter

**Professionally structured:**
1. **Half-title page** (book title only, very minimal)
2. **Title page** (full title, subtitle, author, year)
3. **Copyright page** (ISBN, publisher info, disclaimers)
4. **Dedication page** (optional, centered, italics)
5. **Table of Contents**

### Special Elements

**Scene breaks:** Three centered asterisks with spacing
**Block quotes:** Indented, italicized
**Emphasis:** Proper italic/bold handling
**Hyperlinks:** Invisible in print, active in PDF

---

## Typography Standards

This template follows **professional publishing standards:**

✅ Proper ligatures (fi, fl, etc.)
✅ Optical kerning and microtypography
✅ Hanging punctuation (microtype package)
✅ Smart quotes and dashes
✅ Old-style numerals in text
✅ Widow/orphan prevention
✅ Consistent spacing
✅ Balanced page layout

---

## What It Looks Like

### Chapter Opening Example:

```
                                                72

                        THE REVELATION

─────────────────────────────────────────────────────

Bob Kuwahara's nephew Danny was nine years old and terrible
at Tetris.

Bob watched from the kitchen doorway as Danny mashed buttons...
```

### Running Headers Example:

```
THE REVELATION                                          45

[Body text continues...]


46                                            Drop Test

[Body text continues...]
```

---

## How to Use

### Build with Custom Template

The template is **automatically applied** when you build:

```bash
cd book/
make pdf
```

The Makefile uses `metadata.yaml` which specifies:
```yaml
template: templates/book_template.tex
```

### Manual Build

If building manually:
```bash
pandoc metadata.yaml chapters/*.md \
  -o output/pdf/book.pdf \
  --pdf-engine=xelatex \
  --template=templates/book_template.tex
```

---

## Customization

### Change Fonts

Edit these lines in `book_template.tex`:

```latex
\setmainfont{Palatino}     % Body text
\setsansfont{Helvetica}    % Headers
\setmonofont{Courier New}  % Code
```

### Change Colors

Edit the color definitions:

```latex
\definecolor{chaptercolor}{RGB}{51, 51, 51}  % Chapter numbers
\definecolor{linkcolor}{RGB}{0, 51, 102}     % Links
```

### Adjust Margins

Edit geometry settings:

```latex
\geometry{
    inner=0.875in,   % Binding side
    outer=0.625in,   % Outside edge
    ...
}
```

### Disable Drop Caps

Remove `\dropcap{L}` commands from chapter starts, or just don't use them.

---

## Print vs Digital

**For Print (paperback):**
- Two-sided layout ✓
- Inner/outer margins optimized ✓
- Black links (invisible) ✓
- High-quality typography ✓

**For Digital (PDF/eBook):**
- Hyperlinks active ✓
- Bookmarks in PDF ✓
- Searchable text ✓
- Copy/paste enabled ✓

---

## Dependencies

**Required LaTeX packages** (all included in full TeX distributions):

- `fontspec` - Font selection
- `microtype` - Microtypography
- `geometry` - Page layout
- `fancyhdr` - Headers/footers
- `titlesec` - Title formatting
- `lettrine` - Drop caps
- `listings` - Code blocks
- `hyperref` - PDF hyperlinks

**Install via:**
```bash
# Full TeX Live (recommended)
sudo apt-get install texlive-full

# Or minimal + needed packages
sudo apt-get install texlive-latex-base texlive-latex-extra
```

---

## Comparison to Default

**Default Pandoc Template:**
- Basic layout
- Plain chapter headings
- Minimal styling
- Computer Modern font
- Simple page numbers
- No special front matter

**Custom Template:**
- Professional book layout ✨
- Dramatic chapter openings
- Beautiful typography
- Palatino/Helvetica fonts
- Running headers
- Complete front matter (half-title, copyright, dedication)
- Print-ready quality

---

## Estimated Page Count

With this template, **~75,000 words** produces approximately:

**290-310 pages** (6×9" format)

Breakdown:
- Front matter: 6-8 pages
- Main text: 250-270 pages (~270 words/page)
- Epilogue: 18-20 pages
- Appendix: 15-20 pages

Perfect length for trade paperback!

---

## File Structure

```
book/templates/
├── book_template.tex      # Main LaTeX template
├── TEMPLATE_README.md     # This file
└── style.css              # HTML/EPUB styles (separate)
```

---

## Troubleshooting

### "Font not found" errors

Make sure fonts are installed:
```bash
# Check available fonts
fc-list | grep -i palatino
fc-list | grep -i helvetica
```

On some systems, use these alternatives:
- Palatino → "TeX Gyre Pagella"
- Helvetica → "TeX Gyre Heros"
- Courier → "TeX Gyre Cursor"

### Build fails

Check XeLaTeX is installed:
```bash
xelatex --version
```

Enable verbose output:
```bash
pandoc ... --verbose
```

### Margins look wrong

Make sure using two-sided:
```yaml
classoption:
  - twoside
```

---

## Credits

Template design inspired by:
- Classic O'Reilly book layouts
- Tufte-LaTeX (Edward Tufte's design principles)
- Memoir class best practices
- Traditional book typography standards

Optimized for narrative non-fiction with technical elements.

---

**Status:** Ready to use
**Quality:** Professional, publisher-grade
**Print-ready:** Yes (6×9" trade paperback)
**Digital-ready:** Yes (PDF with hyperlinks and bookmarks)

🎨 **Beautiful typography for a beautiful story!**
