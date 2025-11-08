# AeroGeo 3D - Technical Design Document

**Platform:** Nintendo 3DS / 3DS XL / New 3DS
**Target Release:** TBD (Experimental/Research Project)
**Status:** Planning Phase

---

## Executive Summary

AeroGeo 3D is a stereoscopic aerial photo interpretation and structural geology analysis tool for the Nintendo 3DS platform. It leverages the 3DS's unique stereoscopic 3D display to provide true depth perception for aerial photograph analysis—replicating the classic stereoscope experience in a portable, digital format with modern annotation and export capabilities.

**Key Innovation:**
The 3DS's autostereoscopic (glasses-free) 3D display allows geologists to view stereo aerial photo pairs with genuine depth perception, enabling:
- Identification of topographic features
- Structural geology interpretation (faults, folds, joints)
- Terrain analysis
- Fracture mapping
- Educational demonstrations

---

## Hardware Platform

### Nintendo 3DS Specifications

**CPU:**
- **ARM11 MPCore (Application):** 268 MHz (4 cores, 2 active for apps)
- **ARM9 (System):** 134 MHz
- Significantly more powerful than Game Boy

**Memory:**
- **Application RAM:** 64 MB (128 MB on New 3DS)
- **VRAM:** 6 MB
- Ample space for image data and processing

**Displays:**
- **Top Screen:** 800×240 pixels (400×240 per eye in 3D mode)
  - Autostereoscopic 3D (parallax barrier)
  - 3D depth slider (adjustable)
  - 3.53" diagonal (4.88" on XL)
- **Bottom Screen:** 320×240 pixels
  - Resistive touch screen
  - Single stylus interface

**Input:**
- Circle Pad (analog stick)
- D-pad
- A, B, X, Y buttons
- L, R shoulder buttons
- Touch screen + stylus
- Home, Start, Select buttons

**Sensors:**
- Gyroscope
- Accelerometer
- 3D depth slider position

**Cameras:**
- **Outer Cameras:** 2× VGA (640×480), stereoscopic pair
- **Inner Camera:** VGA (640×480)
- Could capture stereo photos in field

**Storage:**
- SD card support (up to 32 GB SDHC, 2 TB SDXC on New 3DS)
- Load large aerial photo datasets

**Wireless:**
- Wi-Fi
- StreetPass (passive data exchange)
- Potential for field data sharing

**Battery:**
- 3-5 hours typical use
- Rechargeable lithium-ion
- Less than Game Boy, but acceptable

---

## Core Features

### 1. Stereoscopic Aerial Photo Viewing

**The Killer Feature:**
Display georeferenced stereo aerial photo pairs with true stereoscopic 3D depth perception—like a traditional mirror stereoscope, but digital.

**Workflow:**
1. Load left/right stereo aerial photo pair
2. Calibrate stereo alignment
3. Adjust 3D depth slider for comfortable viewing
4. View terrain in 3D with natural depth cues
5. Annotate features directly on image

**Image Requirements:**
- Georeferenced stereo pair (GeoTIFF, JPEG + world file, etc.)
- Overlapping aerial photographs (60-70% overlap)
- Compatible with USGS, NRCS, or commercial aerial imagery

**Advantages over Physical Stereoscope:**
- Digital zoom and pan
- Annotations saved with image
- Export to GIS software
- Multiple datasets on SD card
- No bulky equipment

### 2. Touch-Screen Annotation

**Bottom Screen:** Control panel and toolbox
**Top Screen:** Image display (view only, annotate via cursor)

**Annotation Tools:**
- **Polyline:** Trace faults, contacts, fold axes
- **Point:** Mark locations of interest
- **Polygon:** Outline areas (landslide zones, etc.)
- **Text:** Add labels and notes

**Attributes:**
- Feature type (fault, fold, contact, joint)
- Strike/dip estimation
- Confidence level
- Custom notes

**Controls:**
- Touch screen: Select tool, adjust parameters
- Circle Pad: Move cursor on top screen
- A button: Confirm point/placement
- B button: Cancel/undo
- L/R: Zoom in/out

### 3. Orientation Measurement

**From Aerial Geometry:**
Extract strike/dip of planar features from stereo aerial photos using parallax measurements.

**Method:**
1. Trace linear feature (fault, ridge, etc.)
2. Measure apparent dip from parallax
3. Calculate true orientation
4. Export to stereonet software

**Limitations:**
- Less accurate than field measurements
- Best for regional features
- Requires clear topographic expression

### 4. Export & Integration

**Export Formats:**
- **Shapefile:** Vector annotations with attributes
- **KML/KMZ:** Google Earth compatibility
- **CSV:** Point data for spreadsheet/GIS
- **GeoCalc format:** Direct import to stereonet app (iOS/Android)

**Workflow Integration:**
- Interpret aerial photos in field or office
- Export annotations
- Import to GIS (ArcGIS, QGIS)
- Combine with field data

### 5. Educational Mode

**Teaching Photo Geology:**
- Pre-loaded example stereo pairs
- Guided tutorials
- Quiz mode (identify features)
- Ideal for university field camp preparation

**Example Datasets:**
- Classic fold structures
- Fault zones
- Glacial features
- Sedimentary structures

---

## Technical Architecture

### Development Toolchain

**Primary Tools:**
- **devkitARM:** ARM cross-compiler toolchain
- **libctru:** Core 3DS homebrew library
- **citro2d:** 2D graphics library
- **citro3d:** 3D graphics library (if using GPU for image processing)
- **Bannertool & makerom:** Create installable CIA files

**Build Environment:**
- Cross-platform (Windows, macOS, Linux)
- MSYS2 on Windows, native on Unix-like systems
- Distributed via pacman package manager

**Installation:**
```bash
sudo (dkp-)pacman -S 3ds-dev
```

**Testing:**
- Citra emulator (3DS emulator, limited 3D support)
- Real hardware testing essential (3D display cannot be emulated)
- 3dslink for wireless deployment to hardware

### Memory Architecture

**Application RAM (64 MB baseline, 128 MB New 3DS):**
- Image data: 20-40 MB (compressed aerial photos)
- Annotation data: 1-2 MB
- UI elements: 5 MB
- System overhead: ~10 MB
- Free space: 10-20 MB

**Can load multiple stereo pairs, switch between datasets**

**SD Card Storage:**
- Store datasets on SD (effectively unlimited)
- Load on demand
- Manage photo library

### Graphics Pipeline

**Top Screen (3D Stereo View):**
- Display left/right images to respective eyes
- Parallax barrier creates depth effect
- User adjusts 3D slider for optimal convergence

**Rendering Approach:**
- Load stereo pair as textures
- Render left image to left eye framebuffer
- Render right image to right eye framebuffer
- GPU handles compositing

**Bottom Screen (Touch Interface):**
- 2D UI elements
- Toolbar, controls, metadata display
- Stylus input for tool selection

**Graphics Library:**
- citro2d for 2D UI
- Direct framebuffer access for image display
- Optimize for performance (no lag during pan/zoom)

### File Formats

**Input:**
- **GeoTIFF:** Georeferenced TIFF (read EXIF/geotags)
- **JPEG + world file:** Common aerial photo format
- **Custom format:** Paired stereo images with metadata

**Output:**
- **Shapefile:** Annotated features
- **KML/KMZ:** For Google Earth
- **CSV:** Tabular data
- **JSON:** Metadata and feature attributes

**Configuration:**
- **Project file:** Links stereo pair, stores annotations, preferences
- **Calibration data:** Stereo alignment settings per dataset

### Data Structures

```c
// Stereo Image Pair
typedef struct {
    char left_path[256];
    char right_path[256];
    float geo_bounds[4];  // min_lon, min_lat, max_lon, max_lat
    float scale;          // pixels per meter
    int width;
    int height;
    u8* left_texture;
    u8* right_texture;
} StereoPair;

// Annotation Feature
typedef struct {
    enum FeatureType type;  // FAULT, FOLD, CONTACT, POINT
    int num_points;
    float* coordinates;     // lat/lon pairs
    float strike;
    float dip;
    int confidence;
    char notes[256];
} Feature;

// Project
typedef struct {
    char name[64];
    char location[128];
    StereoPair image_pair;
    Feature* features;
    int num_features;
    time_t created;
    time_t modified;
} Project;
```

---

## User Interface Design

### Screen Layout

**Top Screen (400×240 per eye):**
```
┌────────────────────────────────────────┐
│                                        │
│                                        │
│    [Stereo Aerial Photo Display]      │
│         (3D depth perception)          │
│                                        │
│                                        │
│   Cursor: ◯  Zoom: 1.5x  N↑           │
└────────────────────────────────────────┘
```

**Bottom Screen (320×240 touch):**
```
┌──────────────────────────────────────┐
│ AeroGeo 3D       Dataset: Basin-23   │
├──────────────────────────────────────┤
│ [Tool Palette]                       │
│  ✓ Pan  │ Polyline │ Point │ Polygon │
│  Zoom  │   Text   │ Ruler │  Info   │
├──────────────────────────────────────┤
│ Feature: Fault (Trace)               │
│ Strike: 045°  Dip: 60°  Conf: Med    │
│ [Save] [Undo] [Delete] [Export]      │
├──────────────────────────────────────┤
│ Annotations: 12                      │
│ • Fault_1 (polyline, 340m)           │
│ • Fold_Axis_2 (polyline, 580m)  ⌄    │
└──────────────────────────────────────┘
```

### Control Scheme

**Navigation (Top Screen):**
- **Circle Pad:** Pan image
- **L/R buttons:** Zoom in/out
- **D-pad:** Fine cursor movement
- **A button:** Place point/confirm

**Tool Selection (Bottom Screen):**
- **Touch:** Select tool from palette
- **Stylus:** More precise than finger

**Annotation Workflow:**
1. Select tool (touch bottom screen)
2. Navigate to feature (Circle Pad / D-pad)
3. Trace/mark feature (A button for points)
4. Set attributes (touch bottom screen)
5. Save feature

**3D Adjustment:**
- **3D Slider (hardware):** User adjusts for comfort
- **Auto-calibration option:** Software assists

---

## Core Workflows

### Workflow 1: Load and View Stereo Pair

1. **Insert SD card** with stereo images
2. **Launch AeroGeo 3D**
3. **Select dataset** from library
4. **Load stereo pair** (5-10 seconds)
5. **Adjust 3D slider** for comfortable viewing
6. **Pan/zoom** to area of interest
7. **View in 3D** with depth perception

### Workflow 2: Trace a Fault

1. **View stereo pair** in 3D
2. **Identify fault** (offset terrain, scarp, etc.)
3. **Select Polyline tool** (touch bottom screen)
4. **Navigate to fault start** (Circle Pad)
5. **Press A** to place first point
6. **Move cursor along fault** (Circle Pad)
7. **Press A** at each vertex
8. **Press B** to complete polyline
9. **Set attributes** (type: fault, confidence, notes)
10. **Save feature**

### Workflow 3: Export Annotations

1. **Open project** with annotations
2. **Select Export** from menu
3. **Choose format** (Shapefile, KML, CSV)
4. **Export to SD card**
5. **Transfer to PC**
6. **Import to GIS** or stereonet software

### Workflow 4: Field Photo Capture (Future Enhancement)

1. **Use 3DS cameras** to capture stereo pair in field
2. **Save as project**
3. **Annotate immediately** or later
4. **Combine with field measurements**
5. **Export integrated dataset**

---

## Development Phases

### Phase 1: Proof of Concept (Weeks 1-4)
- Set up devkitPRO environment
- Display single image on top screen
- Implement basic pan/zoom
- Test on hardware

### Phase 2: Stereo Display (Weeks 5-8)
- Load stereo pair
- Render to left/right eye buffers
- Verify 3D effect on hardware
- Calibration tools

### Phase 3: Touch UI (Weeks 9-12)
- Implement bottom screen UI
- Tool palette
- Touch input handling
- Cursor control from Circle Pad

### Phase 4: Annotation Tools (Weeks 13-18)
- Polyline, point, polygon tools
- Coordinate tracking
- Feature attributes
- Save/load annotations

### Phase 5: Export & Integration (Weeks 19-22)
- Implement export formats
- Georeferencing support
- Test integration with GIS

### Phase 6: Polish & Testing (Weeks 23-26)
- Performance optimization
- UI refinement
- Real-world testing with geologists
- Documentation

**Total Timeline: ~6 months**

---

## Challenges & Solutions

### Challenge 1: Stereo Alignment

**Problem:** Stereo pairs must be precisely aligned for comfortable 3D viewing.

**Solution:**
- Manual alignment controls
- Auto-alignment algorithm (feature matching)
- Allow user fine-tuning
- Save calibration per dataset

### Challenge 2: Large Image Files

**Problem:** High-resolution aerial photos can be 20-50 MB each.

**Solution:**
- Image compression (JPEG)
- Tiled loading (load visible region only)
- LOD (Level of Detail) pyramid
- Streaming from SD card

### Challenge 3: Touch Screen Precision

**Problem:** Resistive touch isn't as precise as modern capacitive screens.

**Solution:**
- Stylus-optimized interface
- Large touch targets
- Zoom for fine-detail work
- Hybrid Circle Pad + touch control

### Challenge 4: Battery Life

**Problem:** 3-5 hours vs Game Boy's 30 hours.

**Solution:**
- Acceptable for office/field camp use
- USB charging (can use power bank)
- Power saving mode (dim screen when idle)
- Not for multi-day field work without charging

### Challenge 5: Emulator Limitations

**Problem:** Citra emulator doesn't support 3D display well.

**Solution:**
- Hardware testing required for 3D calibration
- Use emulator for 2D UI development
- Early hardware prototype essential

---

## Comparison to Traditional Stereoscope

| Feature | Physical Stereoscope | AeroGeo 3D |
|---------|---------------------|------------|
| **3D Viewing** | Yes (mirrors/lenses) | Yes (autostereoscopic) |
| **Portability** | Bulky, fragile | Pocket-sized, durable |
| **Image Format** | Physical prints only | Digital (unlimited) |
| **Annotations** | Pencil on overlay | Digital, exportable |
| **Zoom** | Fixed magnification | Digital zoom |
| **Cost** | $200-500 (good quality) | ~$100-200 (used 3DS + software) |
| **Setup Time** | 5-10 minutes | 30 seconds |
| **Export** | Manual digitizing | Direct GIS export |
| **Learning Curve** | Moderate | Low (familiar device) |

**Verdict:** AeroGeo 3D provides comparable viewing experience with significant workflow advantages.

---

## Market & Applications

### Target Users

**Primary:**
- Structural geologists (academic)
- Field camp instructors
- Petroleum geology exploration teams
- Mining geologists

**Secondary:**
- Engineering geologists
- Environmental consultants
- Geography/GIS educators
- Hobbyist geologists

### Use Cases

**1. Field Camp Preparation**
- Students study stereo pairs before field work
- Learn photo interpretation
- Practice identifying structures
- Compare to field observations

**2. Regional Mapping**
- Trace large-scale faults and folds
- Map structural domains
- Create preliminary interpretations
- Refine with field data

**3. Teaching Tool**
- Demonstrate stereoscopic viewing to students
- Interactive photo geology exercises
- More engaging than paper prints
- Easy to share datasets

**4. Remote Site Analysis**
- Analyze inaccessible terrain
- Preliminary structural assessment
- Plan field work locations
- Identify key outcrops

### Market Size

**Realistically Small:**
- Niche application (geology + Nintendo enthusiast)
- Educational institutions (10-50 units)
- Retired geologists (nostalgia + utility)
- Research project, not commercial blockbuster

**Estimated Market:**
- 100-300 total units
- Priced at $99-129
- Revenue: $10K-30K
- Goal: Break even, validate concept, have fun

---

## Success Criteria

**Technical:**
- ✓ Comfortable 3D viewing on hardware
- ✓ Load stereo pairs in <10 seconds
- ✓ Smooth pan/zoom (30 fps)
- ✓ Accurate georeferencing
- ✓ Export to standard GIS formats

**Usability:**
- ✓ Intuitive controls (learn in <15 minutes)
- ✓ Comparable to physical stereoscope
- ✓ Stable (no crashes)
- ✓ Useful for real geological work

**Adoption:**
- ✓ Positive feedback from test users
- ✓ Used in at least one university course
- ✓ Demonstrates viable alternative to stereoscopes
- ✓ Inspires similar tools on modern platforms

---

## Future Enhancements

### Version 2.0 Features

**Terrain Analysis:**
- Digital Elevation Model (DEM) generation from stereo
- Automated feature detection
- 3D terrain visualization

**Live Field Capture:**
- Use 3DS cameras to capture stereo photos
- Immediate annotation
- GPS integration (if available)

**Networking:**
- Wi-Fi sync to cloud storage
- Share datasets with team
- Collaborative annotation

**New 3DS Enhancements:**
- Use extra RAM (128 MB vs 64 MB)
- Higher-resolution images
- More annotations per project
- Faster performance (extra CPU cores)

---

## References

### 3DS Development

- **devkitPRO:** https://devkitpro.org/
- **libctru:** https://github.com/devkitPro/libctru
- **3dbrew:** https://www.3dbrew.org/ (homebrew wiki)
- **GBAtemp:** https://gbatemp.net/ (community)

### Photogrammetry & Stereo Viewing

- **Airphoto Interpretation:** American Society for Photogrammetry and Remote Sensing
- **Lillesand & Kiefer:** *Remote Sensing and Image Interpretation*
- **Wolf & Dewitt:** *Elements of Photogrammetry*

### Geological Applications

- **Structural Geology from Aerial Photos:** Classical techniques
- **Digital Terrain Analysis:** Modern GIS-based methods

---

## Conclusion

AeroGeo 3D represents a novel approach to aerial photo interpretation: bringing the classic stereoscope experience into the digital age using Nintendo 3DS hardware. While the market is niche, the technical feasibility is high, and the educational/research value is genuine.

**The 3DS's stereoscopic 3D display is uniquely suited to this application.** No other handheld gaming platform offers glasses-free 3D, making this a time-limited opportunity (3DS is no longer in production, but used units are plentiful).

**This is a passion project with real utility**—similar to the original GeoCalc on Game Boy. It won't be a commercial success, but it could be a valuable tool for a small community of geologists and an excellent learning experience for the developers.

**Next Step:** Proceed to implementation plan.
