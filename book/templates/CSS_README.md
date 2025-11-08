# EPUB Stylesheet Documentation

## Overview

The `style.css` file provides professional typography and layout for EPUB and HTML versions of "Drop Test". It's designed to work across all major e-readers while maintaining the same quality as the PDF version.

## Features

### 📱 E-reader Compatibility

Tested and optimized for:
- **Amazon Kindle** (Kindle Fire, Paperwhite, Oasis)
- **Kobo** (Aura, Clara, Libra)
- **Apple Books** (iPad, iPhone, Mac)
- **Google Play Books**
- **Nook**
- **Web browsers** (Firefox, Chrome, Safari, Edge)

### 🎨 Professional Typography

**Body text:**
- Georgia/Times New Roman serif
- 1.6 line height for readability
- Justified text with hyphenation
- Proper paragraph indentation (first paragraph flush-left)

**Headings:**
- Helvetica/Arial sans-serif
- Size hierarchy (h1: 2em → h6: 1em)
- Navy blue color for h2 (accent)
- Automatic page breaks before h1

**Chapter styling:**
- Automatic "CHAPTER" label before h1
- Bottom border accent
- Page break before each chapter
- Special handling for unnumbered sections (Preface, Epilogue, etc.)

### 💻 Code & Technical Content

Perfect for the Technical Appendix:
- Courier New monospace font
- Light grey background with blue left border
- Syntax highlighting for code samples
- Responsive tables for specifications
- Preserved ASCII diagrams with proper formatting

### 🌓 Dark Mode Support

Automatically adapts to:
- System dark mode preference
- E-reader night mode
- Custom color inversion

Dark mode palette:
- Background: `#1a1a1a`
- Text: `#e0e0e0`
- Accents: Lighter blue
- Code blocks: `#2a2a2a`

### ♿ Accessibility

- Screen reader friendly
- Keyboard navigation support
- High contrast mode
- Focus indicators
- Semantic HTML structure
- Proper heading hierarchy

### 📖 Special Sections

**Front Matter:**
- Centered title page
- Copyright page styling
- Italic dedication
- Clean table of contents

**Back Matter:**
- Unnumbered sections (no "CHAPTER" prefix)
- Centered section titles
- Bibliography formatting
- About the author styling

**Content Elements:**
- Scene breaks (centered asterisks)
- Blockquotes with left border
- Footnotes with separator
- Figure captions

## Customization

### Changing Colors

Edit the `:root` variables:

```css
:root {
  --text-color: #1a1a1a;         /* Body text */
  --secondary-text: #4a4a4a;     /* Captions, metadata */
  --accent-color: #003366;       /* Links, h2 headings */
  --code-bg: #f5f5f5;            /* Code block background */
  --code-border: #d0d0d0;        /* Code block border */
  --blockquote-border: #999999;  /* Left border on quotes */
  --hr-color: #cccccc;           /* Horizontal rules */
}
```

### Changing Fonts

**Body text:**
```css
body {
  font-family: Georgia, "Times New Roman", serif;
}
```

Alternatives:
- Palatino, Georgia, serif (classic)
- Charter, Georgia, serif (readable)
- Iowan Old Style, serif (Apple)

**Headings:**
```css
h1, h2, h3, h4, h5, h6 {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}
```

Alternatives:
- Futura, sans-serif (modern)
- Avenir Next, sans-serif (friendly)
- Gill Sans, sans-serif (elegant)

**Code:**
```css
pre, code {
  font-family: "Courier New", Courier, monospace;
}
```

Alternatives:
- Monaco, monospace (Mac)
- Consolas, monospace (Windows)
- "SF Mono", monospace (Apple)

### Adjusting Spacing

**Line height:**
```css
body {
  line-height: 1.6;  /* Change from 1.4 to 1.8 */
}
```

**Paragraph indentation:**
```css
p {
  text-indent: 1.5em;  /* Change from 1em to 2em */
}
```

**Margins:**
```css
h1 {
  margin-top: 2em;     /* Space before chapter */
  margin-bottom: 1em;  /* Space after chapter */
}
```

### Chapter Number Styling

Customize the "CHAPTER X" prefix:

```css
h1::before {
  display: block;
  font-size: 0.4em;           /* Smaller than title */
  color: var(--secondary-text);
  font-weight: normal;
  letter-spacing: 0.1em;      /* Spaced out */
  margin-bottom: 0.5em;
  text-transform: uppercase;  /* ALL CAPS */
}
```

To remove chapter numbers entirely:
```css
h1::before {
  display: none;
}
```

## E-reader Specific Notes

### Kindle

**Kindle Fire (tablets):**
- Full CSS support
- Color display
- Custom fonts work

**Kindle E-ink (Paperwhite, Oasis):**
- Respects user's font choice
- Grayscale only
- Good CSS support

**Older Kindles (pre-2019):**
- Limited CSS support
- Smaller fonts for code blocks
- Basic table styling

Our CSS includes `@media amzn-kf8` and `@media amzn-mobi` rules for compatibility.

### Kobo

**Excellent CSS support:**
- Respects custom fonts
- Good typography
- Advanced layout features

**Device detection:**
```css
@media screen and (device-width: 758px) and (device-height: 1024px) {
  /* Kobo Aura specific styles */
}
```

### Apple Books

**Best-in-class:**
- Full CSS3 support
- Custom fonts
- Advanced typography
- Dynamic type scaling

**Uses system fonts:**
```css
@supports (font: -apple-system-body) {
  body {
    font: -apple-system-body;
  }
}
```

### Browser (HTML)

**Full support:**
- All CSS features
- Interactive TOC
- Clickable links
- Print stylesheets

## Testing Your EPUB

### Quick Visual Check

1. **Build EPUB:**
   ```bash
   make epub
   ```

2. **Open in Calibre:**
   - Free EPUB reader/manager
   - Edit mode shows CSS application
   - Preview on different devices

3. **Test on real devices:**
   - Side-load to Kindle
   - Upload to Kobo
   - Open in Apple Books

### What to Check

✅ **Typography:**
- [ ] Readable font size
- [ ] Good line spacing
- [ ] Proper paragraph indents
- [ ] Chapter titles stand out

✅ **Layout:**
- [ ] Page breaks before chapters
- [ ] TOC links work
- [ ] Code blocks preserved
- [ ] Tables render correctly

✅ **Dark mode:**
- [ ] Switch to night mode
- [ ] Text still readable
- [ ] Good contrast
- [ ] Code blocks visible

✅ **Responsiveness:**
- [ ] Test on phone
- [ ] Test on tablet
- [ ] Test on desktop
- [ ] Resize text (settings)

## Troubleshooting

### "My fonts aren't showing"

E-readers may override fonts. This is user preference - let them choose their reading font. Our CSS maintains good spacing/layout regardless.

### "Code blocks are ugly"

Some e-readers have limited monospace font support. The `pre` blocks will still be distinguishable but may not look perfect on all devices.

### "Tables overflow on small screens"

Tables in the appendix may scroll horizontally on phone-sized screens. This is expected for technical content.

### "Dark mode colors are wrong"

Check if your e-reader supports `prefers-color-scheme`. Older devices may not auto-switch. Users can still read in night mode - colors just won't adapt.

### "Page breaks aren't working"

Some e-readers ignore CSS page breaks in favor of user settings. This is normal - users control pagination.

## Comparison to PDF Template

| Feature | PDF (LaTeX) | EPUB (CSS) |
|---------|-------------|------------|
| Typography | Palatino + Helvetica | Georgia + Helvetica |
| Layout | Fixed 6×9" | Responsive/reflowable |
| Chapter numbers | 72pt gray numbers | "CHAPTER" prefix |
| Page numbers | Footer corners | None (e-reader controls) |
| Headers | Running heads | None (e-reader controls) |
| Code syntax | Listings package | CSS highlighting |
| Quality | Print-ready | Screen-optimized |

Both templates maintain professional quality appropriate for their medium.

## File Locations

```
book/
├── templates/
│   ├── style.css              ← EPUB/HTML stylesheet
│   ├── book_template.tex      ← PDF LaTeX template
│   ├── CSS_README.md          ← This file
│   └── TEMPLATE_README.md     ← PDF template docs
├── metadata.yaml              ← References style.css
└── Makefile                   ← Uses --css flag
```

## Build Commands

**EPUB (uses this CSS):**
```bash
make epub
# Output: output/epub/book.epub
```

**HTML (also uses this CSS):**
```bash
make html
# Output: output/html/book.html
```

**PDF (uses LaTeX template instead):**
```bash
make pdf
# Output: output/pdf/book.pdf
```

## Advanced: Embedded Fonts

To embed custom fonts in EPUB (not currently configured):

1. Add font files to `fonts/` directory
2. Reference in CSS:
   ```css
   @font-face {
     font-family: 'CustomFont';
     src: url('../fonts/CustomFont.woff2') format('woff2');
     font-weight: normal;
     font-style: normal;
   }

   body {
     font-family: 'CustomFont', Georgia, serif;
   }
   ```

3. Update Pandoc command in Makefile:
   ```makefile
   --epub-embed-font=fonts/CustomFont.woff2
   ```

Note: Embedded fonts increase file size and may be ignored by some e-readers.

## References

- **EPUB 3 spec:** https://www.w3.org/publishing/epub3/
- **Kindle Publishing Guidelines:** https://kdp.amazon.com/en_US/help/topic/G202172740
- **Apple Books Asset Guide:** https://help.apple.com/itc/booksassetguide/
- **CSS for E-books:** https://www.mobileread.com/

## Credits

Stylesheet design inspired by:
- O'Reilly Media EPUB styles
- Practical Typography by Matthew Butterick
- Typography for Lawyers

Optimized for "Drop Test" by Kit Larson.

---

**This stylesheet makes your e-book look as good as your PDF. Happy reading!** 📚✨
