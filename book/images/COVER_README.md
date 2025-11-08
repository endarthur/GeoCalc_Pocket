# Cover Image for "Drop Test"

## Required for EPUB Build

The EPUB format requires a cover image. Place your cover as:
```
book/images/cover.jpg
```

## Specifications

### For EPUB (E-readers)

**Dimensions:**
- **Recommended:** 1600 × 2400 pixels (2:3 ratio)
- Minimum: 1000 × 1500 pixels
- Maximum: 4000 × 6000 pixels

**Format:**
- JPG or PNG
- RGB color mode
- 72-150 DPI
- File size: Under 2 MB

**Safe area:**
- Keep important text/elements 5% inside edges
- E-readers may crop edges slightly

### Design Recommendations

**Front Cover Elements:**
1. **Title:** "Drop Test" (large, bold, readable at thumbnail size)
2. **Subtitle:** "How a Game Boy Became the Most Reliable Tool in Geology"
3. **Author:** "Kit Larson"
4. **Visual:** Game Boy (DMG-01) + geological context
   - Suggestion: Game Boy on rocky outcrop
   - Suggestion: Game Boy surviving a drop (motion blur)
   - Suggestion: Stereonet display on Game Boy screen
   - Suggestion: Field geologist holding Game Boy

**Typography:**
- Title font: Bold, sans-serif (similar to Helvetica)
- Must be readable at 100 × 150 pixel thumbnail
- High contrast against background

**Color Palette:**
- Consider: Game Boy grey (#8f8f8f), screen green (#9bbc0f)
- Geological earth tones: browns, tans, rust
- Professional, not toy-like

**Mood:**
- Professional + nostalgic
- Technical + rugged
- Science meets consumer electronics

## Temporary Placeholder

If you don't have a cover yet, you can:

1. **Build without cover:**
   ```bash
   # Comment out the cover line in metadata.yaml
   # epub-cover-image: images/cover.jpg
   ```

2. **Use a solid color placeholder:**
   - Create a 1600×2400 solid color image
   - Add title text in white
   - Save as `cover.jpg`

3. **Simple DIY cover:**
   - Use Canva, GIMP, or Photoshop
   - Template: Book cover 6×9" (or 1600×2400px)
   - Add title, subtitle, author
   - Use CC0/free images from Unsplash or Pexels

## Design Services (if needed)

**Budget options:**
- Fiverr: $25-100
- 99designs: $300-500
- Reedsy: $200-500

**DIY tools:**
- Canva (book cover templates)
- Adobe Express
- GIMP (free)
- Affinity Publisher

## Example Concepts

### Concept 1: "The Drop"
- Game Boy in mid-air
- Motion blur trailing
- Concrete/stone background below
- Dramatic perspective from above

### Concept 2: "Field Work"
- Game Boy in weathered field notebook
- Brunton compass beside it
- Rock samples, pencils
- Vintage 1990s photo aesthetic

### Concept 3: "Technical"
- Game Boy screen showing stereonet
- Overlaid with technical diagrams
- Blueprint/schematic aesthetic
- Clean, professional

### Concept 4: "Minimalist"
- Single Game Boy centered
- White or geological texture background
- Clean typography
- Let the iconic device speak

## Current Status

⚠️ **Cover image not yet created**

To build EPUB without errors, either:
- Create a cover image and place it at `book/images/cover.jpg`
- Comment out the `epub-cover-image` line in `metadata.yaml`

## Testing Your Cover

1. **Thumbnail test:** Resize to 100×150px - can you read the title?
2. **Grayscale test:** Convert to B&W - still looks good?
3. **E-reader test:** View on actual Kindle/Kobo if possible
4. **Store test:** Check against other books in genre

## Attribution

If using images/fonts:
- Ensure you have proper licenses
- Game Boy is trademarked by Nintendo
- Consider "inspired by" rather than exact product photos
- CC0 photos are safe for commercial use

## Notes

The EPUB build will fail if `cover.jpg` is missing and referenced in `metadata.yaml`. For now, either create a placeholder or comment out the cover reference to build successfully.

---

**When you have your cover, replace this README with the actual image!**
