# GeoCalc Pocket - Implementation Plan

**Project:** GeoCalc Pocket for Nintendo Game Boy
**Timeline:** 6-9 months (with dedicated team)
**Team Size:** 3-4 developers
**Status:** Planning Phase

---

## Overview

This document outlines a phased approach to developing GeoCalc Pocket, from initial prototype to field-tested product. Each phase includes specific deliverables, acceptance criteria, and estimated timelines.

---

## Phase 0: Setup & Preparation (Weeks 1-2)

### Goals
- Establish development environment
- Validate toolchain
- Create initial project structure
- Build "Hello World" on real hardware

### Tasks

#### 1. Development Environment Setup
- [ ] Install RGBDS (assembler, linker, tools)
- [ ] Install BGB emulator with debugger
- [ ] Set up Python environment for tool scripts
- [ ] Configure version control (Git)
- [ ] Acquire Game Boy flash cartridge for testing
- [ ] Acquire test hardware (DMG, Pocket, Color)

#### 2. Learning & Research
- [ ] Complete Game Boy programming tutorials
- [ ] Study Pan Docs (technical reference)
- [ ] Review existing Game Boy homebrew projects
- [ ] Implement basic examples (sprite movement, SRAM save/load)
- [ ] Research stereonet projection algorithms

#### 3. Project Structure
- [ ] Create directory structure
- [ ] Set up Makefile for automated builds
- [ ] Create constants.inc with system definitions
- [ ] Implement basic boot code template
- [ ] Write documentation templates

#### 4. Validation
- [ ] Build and run "Hello World" on emulator
- [ ] Flash to cartridge and test on real hardware
- [ ] Verify SRAM save/load works
- [ ] Test on multiple Game Boy models

### Deliverables
- Working development environment
- Basic ROM that boots on hardware
- Project structure in place
- Team familiar with tools

### Acceptance Criteria
- ✓ ROM runs on real Game Boy
- ✓ SRAM saves data between power cycles
- ✓ Build process is automated
- ✓ All team members can build & test

---

## Phase 1: Core Engine (Weeks 3-8)

### Goals
- Implement shadow buffer rendering system
- Create fixed-point math library
- Generate lookup tables
- Build stereonet projection engine

### Tasks

#### 1. Rendering Pipeline (Week 3-4)
- [ ] Implement shadow buffer system (3,136 bytes in RAM)
- [ ] Write dirty tile tracking
- [ ] Create VBlank tile copy routine
- [ ] Optimize for ~26 tiles/frame throughput
- [ ] Test with simple patterns

**Files:** `render.asm`, `shadow.asm`

#### 2. Math Library (Week 4-5)
- [ ] Implement 8.8 fixed-point add/subtract
- [ ] Implement 8.8 fixed-point multiply
- [ ] Implement 8.8 fixed-point divide
- [ ] Write conversion routines (fixed ↔ integer)
- [ ] Create unit tests (Python test harness)

**Files:** `math.asm`, `tests/test_math.py`

#### 3. Lookup Table Generation (Week 5)
- [ ] Write Python script for sin/cos table (360 entries)
- [ ] Generate radius table for plunge 0-90
- [ ] Create sqrt approximation table
- [ ] Validate against reference implementation
- [ ] Export to assembly include files

**Files:** `tools/gentables.py`, `assets/data/tables.inc`

#### 4. Projection Engine (Week 6-8)
- [ ] Implement equal-area projection algorithm
- [ ] Write point-to-screen coordinate conversion
- [ ] Optimize for Z80 instruction set
- [ ] Test accuracy (compare to Python reference)
- [ ] Plot test points and verify positions

**Files:** `projection.asm`, `plot.asm`

#### 5. Basic Stereonet Display
- [ ] Create stereonet tile graphics (112×112 grid)
- [ ] Generate circular boundary
- [ ] Draw grid lines (10° or 15° spacing)
- [ ] Render stereonet to screen
- [ ] Test visibility in various conditions

**Files:** `assets/tiles/stereonet.png`, converted tiles

### Deliverables
- Working shadow buffer system
- Accurate projection mathematics
- Stereonet displays correctly
- Can plot test points

### Acceptance Criteria
- ✓ Shadow buffer renders without flicker
- ✓ Projection accuracy ±1° (validated)
- ✓ Performance: plot point in <20ms
- ✓ Stereonet visible on real hardware in sunlight
- ✓ Math passes all unit tests

---

## Phase 2: User Interface (Weeks 9-12)

### Goals
- Implement input handling
- Create UI state machine
- Build data entry system
- Design menus

### Tasks

#### 1. Input System (Week 9)
- [ ] Write button polling routine
- [ ] Implement button state tracking (pressed, held, released)
- [ ] Add auto-repeat for held buttons
- [ ] Create input event queue
- [ ] Test responsiveness

**Files:** `input.asm`

#### 2. UI State Machine (Week 10)
- [ ] Define UI states (stereonet, data entry, table, menu)
- [ ] Implement state transitions
- [ ] Create state-specific rendering
- [ ] Add mode indicators to screen
- [ ] Test state flow

**Files:** `ui.asm`

#### 3. Data Entry Interface (Week 11)
- [ ] Design entry screen layout
- [ ] Implement trend/plunge adjustment (D-pad)
- [ ] Add measurement type selection
- [ ] Create visual feedback
- [ ] Add confirmation/cancel
- [ ] Test with gloves (field simulation)

**Files:** `ui.asm`, data entry module

#### 4. Menu System (Week 12)
- [ ] Design main menu
- [ ] Implement menu navigation
- [ ] Add file operations (new, save, load, delete)
- [ ] Create settings menu (if needed)
- [ ] Add help/about screen

**Files:** `menu.asm`

#### 5. Font & Graphics
- [ ] Design minimal font (uppercase, numbers, symbols)
- [ ] Create UI tiles (cursors, icons, borders)
- [ ] Generate tile data
- [ ] Implement text rendering
- [ ] Test readability

**Files:** `assets/tiles/font.png`, `tools/mkfont.py`

### Deliverables
- Complete UI framework
- Functional data entry
- Working menu system
- Readable text/graphics

### Acceptance Criteria
- ✓ All buttons respond correctly
- ✓ Data entry is intuitive (tested on team)
- ✓ Menu navigation is smooth
- ✓ Font is readable on hardware
- ✓ UI state transitions are bug-free

---

## Phase 3: Data Management (Weeks 13-16)

### Goals
- Implement measurement storage
- Create data table view
- Build SRAM save/load
- Add editing/deletion

### Tasks

#### 1. Data Structures (Week 13)
- [ ] Define Measurement structure (4 bytes)
- [ ] Create measurement array (100 entries in RAM)
- [ ] Implement add/edit/delete operations
- [ ] Write data validation
- [ ] Test capacity limits

**Files:** `data.asm`

#### 2. Data Table View (Week 14)
- [ ] Design table layout (scrollable list)
- [ ] Implement scrolling
- [ ] Add measurement selection
- [ ] Show trend/plunge/type for each entry
- [ ] Highlight current selection

**Files:** `ui.asm`, table view module

#### 3. SRAM Save/Load (Week 15)
- [ ] Define project header format
- [ ] Implement save to SRAM
- [ ] Implement load from SRAM
- [ ] Add save validation (checksums)
- [ ] Handle corrupted save data gracefully
- [ ] Test on real hardware (power cycling)

**Files:** `sram.asm`

#### 4. Measurement Editing (Week 16)
- [ ] Add "edit measurement" workflow
- [ ] Allow modification of existing entries
- [ ] Implement delete confirmation
- [ ] Add bulk operations (clear all, etc.)
- [ ] Test edge cases

**Files:** `data.asm`, UI integration

### Deliverables
- Reliable data storage (RAM + SRAM)
- Data table view
- Save/load functionality
- Edit/delete capabilities

### Acceptance Criteria
- ✓ Can store 100 measurements in RAM
- ✓ SRAM reliably saves/loads data
- ✓ No data loss after power cycle
- ✓ Table view scrolls smoothly
- ✓ Edit/delete works correctly

---

## Phase 4: Plotting & Visualization (Weeks 17-20)

### Goals
- Implement interactive plotting
- Draw great circles
- Add cursor/crosshair
- Optimize rendering

### Tasks

#### 1. Point Plotting (Week 17)
- [ ] Plot pole to stereonet
- [ ] Plot line to stereonet
- [ ] Use distinct symbols for types
- [ ] Handle overlapping points
- [ ] Test with real data

**Files:** `plot.asm`

#### 2. Great Circle Rendering (Week 18)
- [ ] Implement great circle algorithm
- [ ] Generate circle points
- [ ] Draw smooth curves
- [ ] Optimize for performance (<50ms)
- [ ] Handle edge cases (vertical planes)

**Files:** `plot.asm`, great circle module

#### 3. Cursor & Selection (Week 19)
- [ ] Add crosshair cursor
- [ ] D-pad navigation on stereonet
- [ ] Display cursor coordinates
- [ ] "Plot at cursor" functionality
- [ ] Visual feedback

**Files:** `ui.asm`, cursor module

#### 4. Rendering Optimization (Week 20)
- [ ] Profile rendering performance
- [ ] Optimize hot paths
- [ ] Reduce tile updates
- [ ] Implement dirty region tracking
- [ ] Test frame rate

**Files:** Various, performance tuning

### Deliverables
- Interactive stereonet plotting
- Great circle visualization
- Cursor navigation
- Smooth, responsive rendering

### Acceptance Criteria
- ✓ Plot single point: <20ms
- ✓ Great circle: <50ms
- ✓ Cursor responds instantly to input
- ✓ Rendering is flicker-free
- ✓ Handles 100 points without slowdown

---

## Phase 5: Analysis Tools (Weeks 21-24)

### Goals
- Implement 3D rotation
- Calculate statistics
- Add density contouring (optional)
- Build analysis toolset

### Tasks

#### 1. 3D Rotation (Week 21-22)
- [ ] Implement rotation matrices
- [ ] Rotate around trend axis
- [ ] Rotate around plunge axis
- [ ] Live preview during rotation
- [ ] Optimize for 100-point dataset

**Files:** `rotate.asm`

#### 2. Statistical Calculations (Week 23)
- [ ] Calculate mean vector
- [ ] Compute best-fit great circle
- [ ] Calculate angular relationships
- [ ] Display results on screen

**Files:** `stats.asm`

#### 3. Density Contouring (Week 24, Optional)
- [ ] Implement simple density grid
- [ ] Count points in grid cells
- [ ] Visualize concentration
- [ ] Test with real data

**Files:** `stats.asm`, contouring module

#### 4. Tool Integration
- [ ] Add rotation controls to UI
- [ ] Display statistics in info panel
- [ ] Integrate all analysis tools
- [ ] Test workflows

**Files:** UI integration

### Deliverables
- Working rotation tool
- Statistical calculations
- (Optional) Density contouring
- Integrated analysis features

### Acceptance Criteria
- ✓ Rotation: <200ms for 100 points
- ✓ Mean vector is accurate
- ✓ Tools are accessible from UI
- ✓ Results match manual calculations

---

## Phase 6: Polish & Optimization (Weeks 25-28)

### Goals
- Bug fixing
- Performance optimization
- UX refinement
- Documentation

### Tasks

#### 1. Bug Fixing (Week 25-26)
- [ ] Review all known issues
- [ ] Test edge cases systematically
- [ ] Fix crashes and glitches
- [ ] Validate all math functions
- [ ] Stress test with large datasets

#### 2. Performance Tuning (Week 27)
- [ ] Profile code with BGB debugger
- [ ] Optimize slow functions
- [ ] Reduce ROM/RAM usage if needed
- [ ] Test battery life on hardware
- [ ] Ensure 30+ hour runtime

#### 3. UX Refinement (Week 27)
- [ ] Gather feedback from test users
- [ ] Improve control responsiveness
- [ ] Enhance visual feedback
- [ ] Simplify workflows
- [ ] Test with non-expert users

#### 4. Help System (Week 28)
- [ ] Create in-app help screens
- [ ] Write concise instructions
- [ ] Add quick reference
- [ ] Design graphics/diagrams

**Files:** `assets/help/`, help module

#### 5. Documentation (Week 28)
- [ ] Write user manual
- [ ] Document file formats
- [ ] Create quick-start guide
- [ ] Write developer notes

**Files:** `docs/user_manual.md`

### Deliverables
- Stable, bug-free ROM
- Optimized performance
- Polished UX
- Complete documentation

### Acceptance Criteria
- ✓ No known crashes
- ✓ Meets all performance targets
- ✓ Battery life ≥30 hours
- ✓ User manual is complete
- ✓ Help system is useful

---

## Phase 7: Field Testing (Weeks 29-32)

### Goals
- Real-world validation
- Field usability testing
- Data accuracy verification
- Durability assessment

### Tasks

#### 1. Laboratory Testing (Week 29)
- [ ] Compare to reference stereonet software
- [ ] Validate projection accuracy
- [ ] Test SRAM reliability (100+ power cycles)
- [ ] Verify calculations against known data
- [ ] Document any discrepancies

#### 2. Field Testing - Round 1 (Week 30)
- [ ] Deploy to 3-5 beta testers (geologists)
- [ ] Use at real outcrops
- [ ] Collect actual field data
- [ ] Compare to manual measurements
- [ ] Gather usability feedback

**Test Sites:**
- Local geology (Golden, CO area)
- University field camps
- Known structural features

#### 3. Environmental Testing (Week 31)
- [ ] **Sunlight:** Test screen visibility in direct sun
- [ ] **Cold:** Use with gloves, test at low temps (-10°C)
- [ ] **Heat:** Test battery life at high temps (40°C)
- [ ] **Humidity:** Use in rain/wet conditions
- [ ] **Shock:** Drop test from 6+ feet multiple times
- [ ] **Dust:** Exposure to field dust/dirt

#### 4. Field Testing - Round 2 (Week 32)
- [ ] Deploy to larger group (10-15 users)
- [ ] Multi-day field sessions
- [ ] Real project data collection
- [ ] Long-term battery test
- [ ] Stress test SRAM (100+ measurements)

#### 5. Feedback Integration
- [ ] Collect all feedback
- [ ] Prioritize issues
- [ ] Fix critical bugs
- [ ] Implement high-value improvements
- [ ] Iterate on UX

### Deliverables
- Field-tested ROM
- Validation data
- User feedback
- Durability report

### Acceptance Criteria
- ✓ Accurate within ±1° of manual methods
- ✓ Survives 6-foot drop test
- ✓ Readable in bright sunlight
- ✓ Usable with gloves in cold weather
- ✓ Positive user feedback (>80% satisfaction)
- ✓ Battery life ≥30 hours in field use

---

## Phase 8: Manufacturing & Release (Weeks 33-36)

### Goals
- Prepare for production
- Design packaging
- Create marketing materials
- Launch product

### Tasks

#### 1. ROM Finalization (Week 33)
- [ ] Freeze code
- [ ] Final testing pass
- [ ] Fix ROM header (checksums, etc.)
- [ ] Create gold master ROM
- [ ] Archive source code

#### 2. Cartridge Manufacturing (Week 34-35)
- [ ] Select PCB manufacturer
- [ ] Order components (MBC1, ROM chips, SRAM, batteries)
- [ ] Program ROM chips
- [ ] Assemble cartridges
- [ ] Test batch (QA sample)

**Components per cartridge:**
- PCB
- MBC1 chip
- 128KB ROM chip
- 32KB SRAM chip
- CR2032 battery
- Cartridge shell
- Label

#### 3. Packaging Design (Week 34)
- [ ] Design cartridge label
- [ ] Create box/case design
- [ ] Print user manual
- [ ] Include field use quick reference card
- [ ] Design professional packaging

#### 4. Quality Assurance (Week 35)
- [ ] Test every manufactured cartridge
- [ ] Verify SRAM save/load
- [ ] Check for ROM corruption
- [ ] Validate on multiple Game Boy models
- [ ] Set acceptance criteria

#### 5. Marketing Materials (Week 36)
- [ ] Create product website
- [ ] Write press release
- [ ] Produce demo video
- [ ] Design brochure
- [ ] Prepare conference demos

#### 6. Launch (Week 36)
- [ ] Announce product
- [ ] Begin taking orders
- [ ] Ship to pre-orders
- [ ] Attend geology conferences
- [ ] Gather initial customer feedback

### Deliverables
- Manufactured cartridges (batch 1: 100-500 units)
- Professional packaging
- User manual
- Marketing materials
- Product launch

### Acceptance Criteria
- ✓ Cartridges pass QA (>99% yield)
- ✓ Packaging is professional
- ✓ Manual is clear and comprehensive
- ✓ First batch ships on time
- ✓ Positive reception at launch

---

## Milestones Summary

| Phase | Timeline | Key Deliverable | Status |
|-------|----------|-----------------|--------|
| 0 | Week 1-2 | Dev environment ready | ⬜ Not Started |
| 1 | Week 3-8 | Core engine working | ⬜ Not Started |
| 2 | Week 9-12 | UI functional | ⬜ Not Started |
| 3 | Week 13-16 | Data management complete | ⬜ Not Started |
| 4 | Week 17-20 | Plotting implemented | ⬜ Not Started |
| 5 | Week 21-24 | Analysis tools ready | ⬜ Not Started |
| 6 | Week 25-28 | Polished ROM | ⬜ Not Started |
| 7 | Week 29-32 | Field tested | ⬜ Not Started |
| 8 | Week 33-36 | Product launched | ⬜ Not Started |

**Total Timeline:** 36 weeks (~9 months)

---

## Resource Requirements

### Personnel

**Minimum Team (3 people):**
1. **Lead Developer:** Game Boy programming, math, optimization
2. **UI/UX Developer:** Interface design, graphics, usability
3. **Domain Expert:** Structural geology, testing, validation

**Ideal Team (4 people):**
- Add: Project Manager / QA Tester

### Hardware

**Development:**
- 3-4× Game Boy DMG units ($50-100 each used)
- 1× Game Boy Pocket ($40-60 used)
- 1× Game Boy Color ($60-80 used)
- 2× Flash cartridges ($50-100 each)
- 1× Cartridge reader/writer ($60)

**Total: ~$500-700**

**Testing:**
- Field testing equipment (compasses, notebooks, etc.)
- Camera for documentation
- Environmental test setup (temperature chamber if possible)

### Software

**Free/Open Source:**
- RGBDS (free)
- BGB emulator (free)
- Python (free)
- Git (free)
- GBTD/GBMB or modern alternatives (free)

**No licensing costs**

### Manufacturing (Batch 1: 500 units)

**Per-unit cost estimate:**
- PCB: $4
- MBC1 chip: $3
- ROM chip: $5
- SRAM chip: $4
- Battery: $1
- Shell + label: $4
- Assembly: $4
- **Total: ~$25/unit**

**Batch 1 costs:**
- Manufacturing: $12,500 (500 units)
- Packaging: $2,000
- Manuals: $1,500
- Initial inventory: ~$16,000

### Development Budget

**Labor:** (not included, varies by team)

**Equipment:** $1,000
**Prototypes:** $2,000
**Manufacturing:** $16,000
**Marketing:** $3,000
**Contingency:** $3,000

**Total: ~$25,000 for initial product launch**

---

## Risk Management

### Technical Risks

**Risk:** Math accuracy insufficient
- **Mitigation:** Extensive testing against reference implementation
- **Backup:** Increase fixed-point precision if needed

**Risk:** Performance too slow
- **Mitigation:** Profile early, optimize hot paths
- **Backup:** Reduce feature set if necessary

**Risk:** SRAM unreliable
- **Mitigation:** Test extensively on real hardware
- **Backup:** Use checksums, auto-save frequently

### Schedule Risks

**Risk:** Development takes longer than planned
- **Mitigation:** Build buffer into schedule (9 months vs 6)
- **Backup:** Cut optional features (density contouring, etc.)

**Risk:** Field testing reveals major issues
- **Mitigation:** Test early and often
- **Backup:** Allocate time for redesign in Phase 7

### Manufacturing Risks

**Risk:** Component availability
- **Mitigation:** Source components early, order extras
- **Backup:** Alternative components (different MBC chip, etc.)

**Risk:** Quality issues in batch
- **Mitigation:** Rigorous QA, test every cartridge
- **Backup:** Rework or replace defective units

### Market Risks

**Risk:** Low adoption
- **Mitigation:** Beta testing validates market need
- **Backup:** Start with small batch (100-200 units)

**Risk:** Nintendo legal issues
- **Mitigation:** No use of Nintendo trademarks, clear disclaimers
- **Backup:** Consult lawyer if needed

---

## Success Metrics

### Development Success

- ✓ ROM boots reliably on all GB models
- ✓ Meets all performance targets
- ✓ Passes accuracy validation
- ✓ No critical bugs
- ✓ Completed on schedule

### Field Success

- ✓ Survives environmental testing
- ✓ Accurate within ±1° vs manual methods
- ✓ Positive user feedback (>80%)
- ✓ Battery life ≥30 hours
- ✓ Usable in real field conditions

### Commercial Success

- ✓ First batch (500 units) sells within 12 months
- ✓ <5% return rate
- ✓ Positive reviews in geology journals/magazines
- ✓ Adoption by university programs
- ✓ Inquiries about v2.0 features

---

## Next Steps

1. **Assemble team** (developers, domain expert)
2. **Set up development environment** (Phase 0)
3. **Begin Phase 1** (core engine development)
4. **Establish regular check-ins** (weekly progress reviews)
5. **Create project tracker** (GitHub issues, Trello, etc.)
6. **Set milestones** in project management tool

---

## Conclusion

This implementation plan provides a structured path from concept to product launch for GeoCalc Pocket. The 9-month timeline is aggressive but achievable with a dedicated team. The phased approach allows for course correction and ensures that each component is thoroughly tested before moving forward.

**Key success factors:**
- Strong Game Boy programming skills
- Domain expertise in structural geology
- Rigorous testing at every phase
- Field validation before launch
- Realistic scope management

**This is an ambitious project, but the payoff—a genuine field tool that serves a real need—makes it worthwhile.**

---

**Status:** Ready to proceed to Phase 0
**Next Document:** Math Reference (projection algorithms, fixed-point details)
**Team:** TBD
**Start Date:** TBD
