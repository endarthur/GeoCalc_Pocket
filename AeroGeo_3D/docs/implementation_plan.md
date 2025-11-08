# AeroGeo 3D - Implementation Plan

**Project:** AeroGeo 3D for Nintendo 3DS
**Timeline:** 6 months (with dedicated developer)
**Team Size:** 1-2 developers
**Status:** Planning Phase / Research Project

---

## Overview

This implementation plan outlines a streamlined approach to developing AeroGeo 3D as a research/passion project. The focus is on proving the concept of stereoscopic aerial photo interpretation on 3DS hardware, with realistic scope for a small team.

**Philosophy:** This is a "20% time" project—built for exploration and learning, not commercial success. The goal is to create something functional and useful for a small audience while having fun with unique hardware.

---

## Phase 0: Setup & Proof of Concept (Weeks 1-2)

### Goals
- Set up 3DS development environment
- Verify toolchain works
- Display an image on 3DS hardware
- Validate 3D stereo display

### Tasks

#### 1. Development Environment Setup (Week 1)
- [ ] Install devkitPRO (pacman-based install)
  ```bash
  sudo (dkp-)pacman -S 3ds-dev
  ```
- [ ] Install devkitARM, libctru, citro2d, citro3d
- [ ] Set up Citra emulator (for 2D testing)
- [ ] Acquire Nintendo 3DS hardware (used market: $80-150)
- [ ] Install homebrew launcher (Luma3DS + FBI)
- [ ] Set up 3dslink for wireless deployment

#### 2. "Hello World" on Hardware (Week 1)
- [ ] Build and run basic Hello World example
- [ ] Deploy to emulator (Citra)
- [ ] Deploy to real hardware via 3dslink
- [ ] Verify console output and graphics

**Deliverable:** ROM boots on hardware

#### 3. Basic Image Display (Week 2)
- [ ] Load JPEG/PNG image file
- [ ] Display on top screen
- [ ] Implement simple controls (D-pad to pan)
- [ ] Test on hardware

**Files:** `main.c`, `image_loader.c`

#### 4. Stereo Display Test (Week 2)
- [ ] Load stereo pair (left/right images)
- [ ] Render left image to left eye buffer
- [ ] Render right image to right eye buffer
- [ ] Test 3D effect on hardware (critical!)
- [ ] Document optimal 3D slider settings

**This is the make-or-break test:** If 3D stereo works well, project is viable.

**Deliverable:** Proof-of-concept showing 3D aerial photos on 3DS

### Acceptance Criteria
- ✓ Development environment functional
- ✓ Can build and deploy to hardware
- ✓ Stereo 3D display works acceptably
- ✓ No major technical blockers identified

### Risk Assessment
**If 3D stereo is uncomfortable or doesn't work well:** Re-evaluate project viability. May need to pivot to 2D-only mode or abandon project.

---

## Phase 1: Core Image Viewer (Weeks 3-6)

### Goals
- Robust image loading from SD card
- Pan and zoom controls
- Optimized performance
- Basic UI framework

### Tasks

#### 1. File Management (Week 3)
- [ ] Implement SD card file browser
- [ ] Load images from SD card
- [ ] Support JPEG and PNG formats
- [ ] Handle large files (10-20 MB)
- [ ] Error handling (missing files, corrupt images)

**Files:** `file_browser.c`, `sd_manager.c`

#### 2. Image Rendering Optimization (Week 4)
- [ ] Texture streaming (load visible region only)
- [ ] Implement tiled loading for large images
- [ ] Optimize memory usage
- [ ] Test with high-resolution aerial photos (5000×5000+ pixels)

**Files:** `image_renderer.c`, `texture_manager.c`

#### 3. Pan & Zoom (Week 5)
- [ ] Circle Pad for panning
- [ ] L/R buttons for zoom in/out
- [ ] Smooth scrolling (60 fps target)
- [ ] Zoom levels: 0.5x, 1x, 2x, 4x, 8x
- [ ] Clamp to image boundaries

**Files:** `input.c`, `camera.c` (viewport control)

#### 4. Stereo Alignment Tools (Week 6)
- [ ] Manual alignment controls (nudge left/right images)
- [ ] Save alignment settings per stereo pair
- [ ] Visual indicators for misalignment
- [ ] Calibration wizard

**Files:** `stereo_calibration.c`

#### 5. Bottom Screen UI (Week 6)
- [ ] Basic info display (filename, zoom level, coordinates)
- [ ] Simple touch button framework
- [ ] Mode indicators

**Files:** `ui.c`, `touch_input.c`

### Deliverables
- Functional stereo image viewer
- Smooth pan/zoom
- Stereo alignment tools
- Basic UI

### Acceptance Criteria
- ✓ Load and display stereo pairs from SD card
- ✓ Pan/zoom is smooth (≥30 fps)
- ✓ Stereo alignment is adjustable
- ✓ Can view high-res aerial photos comfortably

---

## Phase 2: Annotation Tools (Weeks 7-12)

### Goals
- Touch-based drawing tools
- Feature annotation (polyline, point, polygon)
- Save/load annotations
- Export to standard formats

### Tasks

#### 1. Coordinate System (Week 7)
- [ ] Implement screen-to-image coordinate transformation
- [ ] Handle georeferencing (if image has world file)
- [ ] Convert between pixel, geographic, and projected coordinates
- [ ] Test accuracy with known datasets

**Files:** `coordinates.c`, `georef.c`

#### 2. Cursor System (Week 8)
- [ ] Visual cursor on top screen
- [ ] Circle Pad controls cursor position
- [ ] D-pad for fine adjustments
- [ ] Coordinate display (lat/lon or pixel x,y)

**Files:** `cursor.c`

#### 3. Polyline Tool (Week 9)
- [ ] Select polyline tool from bottom screen
- [ ] Place points with A button
- [ ] Draw line segments between points
- [ ] Complete polyline (B button or close loop)
- [ ] Undo last point
- [ ] Visual feedback

**Files:** `annotation_tools.c`, polyline module

#### 4. Point & Polygon Tools (Week 10)
- [ ] Point tool (single-click placement)
- [ ] Polygon tool (similar to polyline but closes)
- [ ] Symbol selection (icon for different feature types)
- [ ] Color coding

**Files:** `annotation_tools.c`

#### 5. Feature Attributes (Week 11)
- [ ] Attribute entry screen (bottom screen)
- [ ] Feature type (fault, fold, contact, etc.)
- [ ] Strike/dip fields (if applicable)
- [ ] Confidence level
- [ ] Notes (short text field, virtual keyboard)

**Files:** `attributes.c`, `keyboard.c` (or use system keyboard)

#### 6. Save/Load Annotations (Week 12)
- [ ] Define annotation file format (JSON or custom)
- [ ] Save annotations to SD card
- [ ] Load annotations with image
- [ ] Edit existing annotations
- [ ] Delete annotations

**Files:** `annotation_io.c`

### Deliverables
- Complete annotation toolset
- Attribute management
- Persistent storage of annotations

### Acceptance Criteria
- ✓ Can trace features accurately
- ✓ Attributes are saved correctly
- ✓ Annotations reload properly
- ✓ Interface is intuitive (tested on users)

---

## Phase 3: Data Export & Integration (Weeks 13-16)

### Goals
- Export to GIS formats
- Integration with desktop software
- Validation with real geological data

### Tasks

#### 1. Shapefile Export (Week 13-14)
- [ ] Implement Shapefile writer (.shp, .shx, .dbf)
- [ ] Export polylines as line features
- [ ] Export points as point features
- [ ] Export polygons as polygon features
- [ ] Include attributes in DBF table
- [ ] Test import in QGIS and ArcGIS

**Files:** `export_shapefile.c`

**Library:** May use existing library or implement subset of format

#### 2. KML/KMZ Export (Week 14)
- [ ] Generate KML (XML format)
- [ ] Include placemarks, polylines, polygons
- [ ] Embed attributes in description
- [ ] Test in Google Earth

**Files:** `export_kml.c`

#### 3. CSV Export (Week 15)
- [ ] Export point data to CSV
- [ ] Include lat/lon, attributes
- [ ] Simple format for spreadsheet import

**Files:** `export_csv.c`

#### 4. GeoCalc Integration (Week 15)
- [ ] Export strike/dip measurements to GeoCalc format
- [ ] Create direct link to stereonet apps
- [ ] Test workflow: aerial interpretation → stereonet analysis

**Files:** `export_geocalc.c`

#### 5. Validation & Testing (Week 16)
- [ ] Test with real aerial photo datasets
- [ ] Compare to manual stereoscope interpretation
- [ ] Validate georeferencing accuracy
- [ ] Import exports into GIS, verify coordinates

### Deliverables
- Working export functionality
- GIS integration validated
- Real-world testing complete

### Acceptance Criteria
- ✓ Exports open correctly in QGIS/ArcGIS
- ✓ Coordinates are georeferenced properly
- ✓ Attributes transfer correctly
- ✓ Workflow is practical for real use

---

## Phase 4: Polish & User Experience (Weeks 17-20)

### Goals
- Refine UI/UX
- Add help system
- Performance optimization
- Prepare example datasets

### Tasks

#### 1. UI Polish (Week 17)
- [ ] Improve visual design
- [ ] Better icons and graphics
- [ ] Consistent color scheme
- [ ] Smooth animations
- [ ] User-friendly error messages

**Files:** `ui.c`, graphics assets

#### 2. Help & Tutorial (Week 18)
- [ ] In-app help screens
- [ ] Quick start guide
- [ ] Annotated example walkthrough
- [ ] Tooltips for tools

**Files:** `help.c`, help content

#### 3. Performance Optimization (Week 19)
- [ ] Profile code (identify bottlenecks)
- [ ] Optimize rendering pipeline
- [ ] Reduce memory usage
- [ ] Faster image loading
- [ ] Target: <2 second load time for typical stereo pair

**Files:** Various, optimization pass

#### 4. Example Datasets (Week 20)
- [ ] Curate 3-5 example stereo pairs
- [ ] Classic structural features (folds, faults)
- [ ] Include pre-made annotations as reference
- [ ] Write interpretive guides
- [ ] Package with app for educational use

**Files:** `assets/examples/`

### Deliverables
- Polished, user-friendly application
- Example datasets for learning
- Complete documentation

### Acceptance Criteria
- ✓ App is intuitive for new users
- ✓ Performance is acceptable (<2s loads)
- ✓ Examples are educational and clear
- ✓ Help system is useful

---

## Phase 5: Testing & Release (Weeks 21-24)

### Goals
- Beta testing with geologists
- Bug fixing
- Documentation finalization
- Limited release

### Tasks

#### 1. Internal Testing (Week 21)
- [ ] Comprehensive testing of all features
- [ ] Edge case testing
- [ ] Stability testing (long sessions)
- [ ] Test on multiple 3DS models (3DS, XL, New 3DS)
- [ ] Document bugs

#### 2. Beta Testing (Week 22-23)
- [ ] Recruit 5-10 beta testers (geologists, educators)
- [ ] Distribute beta build
- [ ] Gather feedback via survey
- [ ] Identify critical issues
- [ ] Collect feature requests

**Beta testers:**
- University professors (structural geology)
- Field geologists
- GIS specialists
- Former GeoCalc users

#### 3. Bug Fixing & Iteration (Week 23)
- [ ] Address critical bugs
- [ ] Implement high-value feedback
- [ ] Regression testing
- [ ] Final stability pass

#### 4. Documentation (Week 24)
- [ ] User manual (PDF)
- [ ] Installation guide (homebrew setup)
- [ ] Workflow tutorials
- [ ] FAQ
- [ ] Technical documentation (for developers)

**Files:** `docs/user_manual.md`, `docs/install.md`

#### 5. Release Preparation (Week 24)
- [ ] Finalize build
- [ ] Create .3dsx and .cia files
- [ ] Package example datasets
- [ ] Prepare distribution (GitHub, GBAtemp, etc.)
- [ ] Write announcement post

### Deliverables
- Stable release build
- Complete documentation
- Distribution package

### Acceptance Criteria
- ✓ No critical bugs
- ✓ Positive beta tester feedback (>70% satisfaction)
- ✓ Documentation is complete
- ✓ Ready for public release

---

## Milestones Summary

| Phase | Timeline | Key Deliverable | Status |
|-------|----------|-----------------|--------|
| 0 | Week 1-2 | Stereo 3D proof of concept | ⬜ Not Started |
| 1 | Week 3-6 | Image viewer working | ⬜ Not Started |
| 2 | Week 7-12 | Annotation tools complete | ⬜ Not Started |
| 3 | Week 13-16 | Export & GIS integration | ⬜ Not Started |
| 4 | Week 17-20 | Polished UX | ⬜ Not Started |
| 5 | Week 21-24 | Beta tested & released | ⬜ Not Started |

**Total Timeline:** 24 weeks (~6 months)

---

## Resource Requirements

### Personnel

**Minimum Team (1 person):**
- Developer with C programming experience
- Interest in 3DS homebrew
- Basic understanding of geology/GIS helpful

**Ideal Team (2 people):**
- Developer (C programming, graphics)
- Geologist (domain expertise, testing)

### Hardware

**Required:**
- 1-2× Nintendo 3DS or 3DS XL ($80-150 each, used)
- OR 1× New Nintendo 3DS/XL ($120-200 used)
- SD card (4+ GB)
- PC for development (Windows, Mac, or Linux)

**Optional:**
- Additional 3DS models for compatibility testing
- Stereoscope (for comparison testing)

**Total: ~$200-400**

### Software

**All Free/Open Source:**
- devkitPRO (free)
- Citra emulator (free)
- Image editing tools (GIMP, etc., free)
- GIS software for testing (QGIS, free)

**No licensing costs**

### Data

**Aerial Photos:**
- USGS Earth Explorer (free)
- NRCS NAIP imagery (free)
- State geological surveys (free)
- Educational institutions (may have archives)

**No data costs**

### Development Budget

**Labor:** (passion project, unpaid or 20% time)

**Equipment:** $300 (3DS hardware, SD cards)
**Testing:** $100 (miscellaneous)
**Contingency:** $100

**Total: ~$500**

**This is an extremely low-budget project—viable as a hobbyist/research endeavor.**

---

## Risk Management

### Technical Risks

**Risk:** 3D stereo viewing is uncomfortable or doesn't work well
- **Mitigation:** Validate in Phase 0 (proof of concept)
- **Backup:** Implement 2D-only mode as fallback

**Risk:** Image file sizes too large for 3DS RAM
- **Mitigation:** Implement tiled loading, texture streaming
- **Backup:** Downscale images, accept resolution loss

**Risk:** Performance too slow
- **Mitigation:** Optimize early and often, profile code
- **Backup:** Reduce feature set, accept limitations

**Risk:** Shapefile export is complex to implement
- **Mitigation:** Use existing library or implement simplified version
- **Backup:** Focus on KML/CSV, which are simpler formats

### Schedule Risks

**Risk:** Development takes longer than 6 months
- **Mitigation:** This is a passion project, flexible timeline
- **Backup:** Release MVP (minimum viable product) earlier, iterate

**Risk:** Beta testing reveals major issues
- **Mitigation:** Internal testing throughout development
- **Backup:** Extend testing phase, delay release

### Adoption Risks

**Risk:** No one uses it
- **Mitigation:** Manage expectations—this is niche
- **Reality:** Even 50-100 users would be success for hobbyist project

**Risk:** 3DS hardware becomes unavailable
- **Mitigation:** Used market is still active (2025)
- **Future:** Port concept to Switch (has stereoscopic cameras, lacks 3D display)

---

## Success Criteria

### Technical Success

- ✓ Stereo 3D viewing is comfortable and useful
- ✓ Can load and display typical aerial photo pairs
- ✓ Annotation tools are functional
- ✓ Export to GIS formats works correctly
- ✓ Stable (no frequent crashes)

### Usability Success

- ✓ Intuitive enough to learn in 20 minutes
- ✓ Comparable experience to physical stereoscope
- ✓ Practical for real geological interpretation
- ✓ Positive user feedback from beta testers

### Community Success

- ✓ 50+ downloads in first year
- ✓ Used in at least one university course
- ✓ Mentioned on GBAtemp or geology forums
- ✓ Inspires discussion of 3D in geology apps

### Personal Success

- ✓ Learn 3DS homebrew development
- ✓ Create something useful and novel
- ✓ Have fun building it
- ✓ Complete a passion project

---

## Post-Release Plans

### Maintenance (Ongoing)

- Bug fixes as reported
- Minor feature additions (if requested)
- Keep compatible with latest Luma3DS

### Potential Version 2.0 Features

**If project is well-received:**
- DEM generation from stereo parallax
- Automated feature detection (machine learning)
- Wi-Fi sync to cloud storage
- 3DS camera integration (capture stereo in field)
- Compass/gyro for orientation sensing

**Realistic Assessment:** Version 1.0 is likely the only version, unless there's unexpected enthusiasm.

### Knowledge Sharing

- Write blog posts about development process
- Share code on GitHub (open source)
- Present at homebrew community events
- Publish academic paper if results are significant

---

## Alternative Paths

### If 3D Stereo Doesn't Work Well

**Plan B:** Pivot to 2D aerial photo viewer/annotator
- Still useful for photo interpretation
- Loses unique 3D selling point
- Simpler implementation

### If Development Stalls

**Minimum Viable Product:**
- Image viewer with stereo display
- Basic pan/zoom
- No annotations (just viewing)
- Release as "proof of concept"

### If Commercial Interest Emerges

**Unlikely, but if it happens:**
- Consider polished release
- Partner with geology equipment vendors
- License to educational institutions
- Port to modern platforms (Switch, tablets)

---

## Next Steps

1. **Acquire 3DS hardware** (if not already owned)
2. **Set up development environment** (Phase 0, Week 1)
3. **Build proof of concept** (Phase 0, Week 2)
4. **Evaluate 3D stereo viability** (GO/NO-GO decision)
5. **If GO:** Proceed to Phase 1
6. **If NO-GO:** Document findings, archive project, or pivot to Plan B

---

## Conclusion

AeroGeo 3D is a technically feasible passion project that leverages the Nintendo 3DS's unique stereoscopic 3D display for geological aerial photo interpretation. The 6-month timeline is achievable for a solo developer or small team, and the resource requirements are minimal.

**This project is about exploration and fun, not profit.** Success is defined by learning, creating something novel, and providing value to a small community of geologists who appreciate both handheld gaming and structural geology.

**The 3DS's autostereoscopic 3D is a rare feature that won't exist in future handhelds.** This is a time-limited opportunity to do something unique with unique hardware.

**If it works, it'll be cool. If it doesn't, it'll be a fun learning experience.** Either way, it's worth trying.

---

**Status:** Ready to proceed to Phase 0
**Go/No-Go Decision Point:** End of Phase 0 (Week 2)
**Next Document:** Development Log (to be created during implementation)
