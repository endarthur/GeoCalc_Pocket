# GeoCalc Pocket - Technical Design Document

**Platform:** Nintendo Game Boy (DMG-01)
**Target Release:** TBD
**Status:** Planning Phase

---

## Executive Summary

GeoCalc Pocket is a structural geology field tool that runs on the Nintendo Game Boy platform. It provides real-time stereonet projection and analysis capabilities for field geologists, leveraging the Game Boy's proven durability, long battery life, and ubiquitous availability.

**Key Advantages:**
- **Durability:** Game Boy hardware is field-proven and nearly indestructible
- **Battery Life:** 30+ hours on 4 AA batteries
- **Availability:** Hardware is readily available and inexpensive
- **Portability:** Fits in a pocket, works anywhere
- **No learning curve:** Familiar button interface

---

## Hardware Platform

### Nintendo Game Boy (DMG-01) Specifications

**CPU:**
- Sharp LR35902 (Z80-like) @ 4.194 MHz
- 8-bit architecture
- No hardware multiply/divide

**Memory:**
- 8KB Work RAM ($C000-$DFFF)
- 8KB Video RAM ($8000-$9FFF)
- Cartridge ROM: 32KB-2MB (banked)
- Cartridge SRAM: 0-32KB (for save data)

**Display:**
- 160×144 pixels
- 4 shades of gray
- 20×18 tile background (8×8 pixel tiles)
- 40 hardware sprites (8×8 or 8×16)
- Reflective LCD (no backlight)

**Input:**
- D-pad (4 directions)
- A, B buttons
- START, SELECT buttons

**Power:**
- 4× AA batteries
- ~30 hours typical use
- ~50 hours with alkaline batteries

**Cartridge:**
- MBC1 or MBC3 memory bank controller
- 128KB ROM (8 banks × 16KB)
- 32KB SRAM (4 banks × 8KB) with battery backup
- Optional: Real-time clock (MBC3)

---

## Core Features

### 1. Stereonet Projection

**Equal-Area (Schmidt) Projection:**
- Display circular stereonet on 112×112 pixel grid
- Plot poles, planes, and lines
- Rotate data in 3D space
- Calculate mean vectors
- Generate density contours

**Mathematical Approach:**
- Pre-computed lookup tables (sin, cos, sqrt)
- Fixed-point arithmetic (8.8 format)
- Optimized for Z80 instruction set

### 2. Data Management

**Measurement Storage:**
- Store 100 measurements in RAM
- Save up to 512 measurements in SRAM
- Each measurement: trend (0-359°), plunge (0-90°), type (pole/plane/line)
- Project metadata: location, date, notes

**Data Entry:**
- D-pad: Adjust trend/plunge values
- A/B: Confirm/cancel
- Fast increment (hold for acceleration)
- Visual feedback during entry

### 3. Visualization

**Stereonet View:**
- Real-time plotting
- Great circle generation for planes
- Color-coded point types (using shades)
- Grid overlay
- Coordinate display

**Data Table View:**
- List all measurements
- Scroll through entries
- Select/highlight measurements
- Edit existing data
- Delete entries

### 4. Analysis Tools

**Rotation:**
- Rotate data around trend, plunge axes
- Live preview of rotation
- Undo/reset capability

**Statistics:**
- Mean vector calculation
- Best-fit great circle
- Density contouring (simple grid)
- Angular measurements

---

## Technical Architecture

### Memory Layout

```
ROM Bank 0 (16KB, fixed @ $0000-$3FFF):
  - Boot code & initialization
  - Main loop & state machine
  - Input handler
  - VRAM management & rendering pipeline
  - Core utilities

ROM Bank 1-7 (switchable @ $4000-$7FFF, 16KB each):
  Bank 1: Projection math & lookup tables
  Bank 2: Tool implementations (plot, rotate, etc.)
  Bank 3: UI & data management
  Bank 4: Help screens & tutorial
  Bank 5: Extended lookup tables
  Bank 6: Analysis tools (statistics, contouring)
  Bank 7: Reserved for future features

VRAM ($8000-$9FFF, 8KB):
  $8000-$8C2F: Stereonet tiles (196 tiles)
  $8C30-$8FFF: Font & UI tiles (60 tiles)
  $9800-$9BFF: Background tilemap
  $9C00-$9FFF: Window tilemap (for UI overlay)

Work RAM ($C000-$DFFF, 8KB):
  $C000-$CC3F: Shadow buffer (3,136 bytes)
  $CC40-$CE0F: Measurement data (100 entries × 4 bytes)
  $CE10-$D143: Runtime lookup tables
  $D144-$D243: Stack (256 bytes)
  $D244-$DFFF: System variables & temp storage

Cartridge SRAM ($A000-$BFFF, 32KB):
  $A000-$A7FF: Saved measurements (512 entries)
  $A800-$AFFF: Project metadata
  $B000-$BFFF: Reserved for expansion
```

### Data Structures

```assembly
; Measurement (4 bytes)
Measurement:
    .trend:  db    ; 0-255 (0-359°, scaled)
    .plunge: db    ; 0-90
    .type:   db    ; Bits 0-1: type (00=pole, 01=plane, 10=line)
                   ;      2: selected flag
                   ;      3-7: reserved/user flags
    .flags:  db    ; Color, confidence, notes index

; Project header (in SRAM)
ProjectHeader:
    .magic:      db "GCPK"    ; Magic bytes
    .version:    db 1          ; Format version
    .count:      dw            ; Number of measurements
    .name:       ds 16         ; Project name
    .location:   ds 32         ; Location description
    .date:       ds 8          ; Date (YYYYMMDD)
    .notes:      ds 64         ; Field notes
```

### Rendering Pipeline

**Shadow Buffer Technique:**
1. Maintain shadow buffer in Work RAM (matches VRAM tiles 1:1)
2. Perform all drawing operations to shadow buffer
3. Track "dirty" tiles that need updating
4. During VBlank, copy dirty tiles to VRAM
5. Maximum ~26 tiles per frame (60 Hz)

**Benefits:**
- Eliminates VRAM access restrictions
- Reduces flickering
- Allows complex drawing operations
- Simplifies code logic

### Projection Mathematics

**Equal-Area Projection Algorithm:**

```
For line with trend T, plunge P:
1. Convert to lower hemisphere (if needed)
2. Calculate radial distance: r = sqrt(2) × sin((90-P)/2)
3. Calculate screen position:
   x = center_x + r × sin(T)
   y = center_y - r × cos(T)
```

**Implementation:**
- Use 8.8 fixed-point math
- Pre-computed sine/cosine table (360 entries)
- Pre-computed radius table (91 entries for plunge 0-90)
- Approximate sqrt(2) as 1.414 (fixed-point: 362/256)

**Accuracy:**
- Angular precision: ±1°
- Screen precision: ±1 pixel (≈1.6° at edge)
- Sufficient for field geology

---

## User Interface Design

### Screen Layout

```
┌────────────────────────────────┐
│ GEOCALC v1.0    [Mode]    N:42 │ ← Status bar (18px)
├────────────────────────────────┤
│                                │
│                                │
│      [Stereonet Display]       │ ← Main area (112×112px)
│         112×112 pixels         │
│                                │
│                                │
├────────────────────────────────┤
│ T:045 P:32  Type:POLE    [▲▼] │ ← Info bar (14px)
└────────────────────────────────┘
```

### Control Scheme

**Stereonet Mode:**
- D-pad: Pan/navigate cursor
- A: Add measurement at cursor
- B: Toggle modes
- SELECT: Tool menu
- START: Main menu

**Data Entry Mode:**
- D-pad Up/Down: Adjust trend
- D-pad Left/Right: Adjust plunge (faster)
- A: Confirm entry
- B: Cancel
- Hold D-pad: Fast increment

**Data Table Mode:**
- D-pad Up/Down: Scroll list
- A: Select/edit measurement
- B: Return to stereonet
- SELECT: Delete measurement
- START: Menu

**Menu System:**
- D-pad: Navigate options
- A: Select
- B: Back/cancel

### Visual Feedback

**Field Usability:**
- Large, clear text (minimum 8×8 font)
- High contrast (dark on light for sunlight)
- Distinct symbols for measurement types
- Visual cursor position indicators
- Confirmation prompts for destructive operations

---

## Development Tools & Workflow

### Toolchain

**Assembler:**
- RGBDS (Rednex Game Boy Development System)
- Modern, well-documented Z80 assembler
- Active community support
- Cross-platform (Windows, Mac, Linux)

**Emulator:**
- BGB (highly accurate debugging emulator)
- Built-in debugger, memory viewer, tile viewer
- Fast iteration during development

**Testing:**
- Real hardware testing essential
- Use flash cartridge (EverDrive GB, etc.)
- Test on multiple Game Boy variants (DMG, Pocket, Color)

**Graphics:**
- GBTD (Game Boy Tile Designer)
- GBMB (Game Boy Map Builder)
- Custom Python scripts for data generation

**Math Tools:**
- Python for lookup table generation
- Verify against reference implementation
- Fixed-point conversion utilities

### Build Process

```bash
# Assemble source files
rgbasm -o main.o src/main.asm
rgbasm -o math.o src/math.asm
rgbasm -o ui.o src/ui.asm

# Link into ROM
rgblink -o geocalc.gb main.o math.o ui.o

# Fix ROM header
rgbfix -v -p 0xFF geocalc.gb

# Test in emulator
bgb geocalc.gb

# Flash to cartridge for hardware testing
```

### Testing Strategy

**Unit Testing:**
- Automated tests for math functions
- Compare output against Python reference
- Verify fixed-point precision

**Integration Testing:**
- Test complete workflows
- Verify SRAM save/load
- Check memory usage

**Field Testing:**
- Real outcrop data collection
- Usability in various conditions:
  - Bright sunlight
  - Cold weather (gloves)
  - Rain/humidity
  - Rough handling
- Compare results to manual stereonet plotting

---

## Performance Targets

### Timing Requirements

**User Experience:**
- Plot single point: <20ms (instant)
- Plot great circle: <50ms (no noticeable delay)
- Rotate 100 points: <200ms (acceptable lag)
- Screen redraw: <150ms (smooth transition)
- Menu response: <10ms (instant)

**Battery Life:**
- Target: 30+ hours on alkaline AAs
- Minimize screen updates
- Efficient algorithms (fewer CPU cycles)
- No busy-waiting loops

### Memory Budget

**ROM Usage:**
- Code: ~60KB
- Lookup tables: ~15KB
- Graphics tiles: ~4KB
- Help/tutorial: ~10KB
- Reserved: ~30KB
- Total: 128KB (fits in 8 banks)

**RAM Usage:**
- Shadow buffer: 3,136 bytes (fixed)
- Measurement data: 400 bytes (100 entries)
- Lookup tables: ~800 bytes
- System variables: ~500 bytes
- Stack: 256 bytes
- Free space: ~3KB
- Total: ~8KB

**SRAM Usage:**
- Saved measurements: 2KB (512 entries)
- Project metadata: 1KB
- Reserved: 29KB
- Total: 32KB

---

## File Organization

### Source Code Structure

```
GeoCalc_Pocket/
├── docs/
│   ├── technical_design.md (this file)
│   ├── implementation_plan.md
│   ├── math_reference.md
│   └── user_manual.md
├── src/
│   ├── main.asm           # Entry point, main loop
│   ├── init.asm           # Initialization code
│   ├── input.asm          # Button input handling
│   ├── render.asm         # VRAM/rendering pipeline
│   ├── shadow.asm         # Shadow buffer operations
│   ├── projection.asm     # Stereonet projection math
│   ├── plot.asm           # Plotting functions
│   ├── rotate.asm         # 3D rotation
│   ├── stats.asm          # Statistical calculations
│   ├── ui.asm             # UI state machine
│   ├── menu.asm           # Menu system
│   ├── data.asm           # Data management
│   ├── sram.asm           # Save/load to SRAM
│   ├── utils.asm          # Utility functions
│   ├── math.asm           # Fixed-point math
│   └── constants.inc      # Constants & macros
├── assets/
│   ├── tiles/             # Graphics tiles
│   ├── maps/              # Tilemaps
│   └── data/              # Lookup tables
├── tools/
│   ├── gentables.py       # Generate lookup tables
│   ├── mkfont.py          # Font converter
│   └── validate.py        # Test harness
├── tests/
│   └── test_math.py       # Unit tests
├── Makefile
└── README.md
```

---

## Challenges & Solutions

### Challenge 1: Limited Tile Budget

**Problem:** Stereonet needs ~196 unique tiles, leaving only 60 for UI, fonts, symbols.

**Solution:**
- Minimal font (uppercase only, 36 tiles: A-Z, 0-9)
- Reuse tiles where possible
- Procedural generation of simple patterns
- Accept reduced graphical flair for functionality

### Challenge 2: Slow CPU for Floating-Point Math

**Problem:** Z80-like CPU has no floating-point hardware. Projection math is complex.

**Solution:**
- Fixed-point arithmetic (8.8 or 16.8 format)
- Pre-computed lookup tables (sin, cos, sqrt)
- Optimize hot paths in assembly
- Accept minor precision loss (±1°)

### Challenge 3: VRAM Access Restrictions

**Problem:** Can't write to VRAM during active display (causes corruption).

**Solution:**
- Shadow buffer in Work RAM
- Track dirty tiles
- Batch update during VBlank
- Limit to ~26 tiles/frame

### Challenge 4: Readability in Sunlight

**Problem:** Original Game Boy screen washes out in bright sunlight.

**Solution:**
- High contrast design (dark on light)
- Thicker grid lines (more visible)
- Larger symbols/text
- Test in actual field conditions

### Challenge 5: Glove-Friendly Interface

**Problem:** Geology is often done in cold weather with gloves on.

**Solution:**
- Large "hit zones" for selections
- Forgiving input (hard to mis-press)
- Visual feedback for all actions
- Minimal precision required for navigation
- Field test with work gloves

---

## Future Enhancements

### Version 1.x Features

**Potential additions:**
- Enhanced density contouring
- Multiple dataset support
- Data labels/annotations
- Undo/redo stack
- Customizable grid spacing
- User preferences

### Link Cable Support (v2.0)

**Serial Data Transfer:**
- Connect two Game Boys
- Share measurement data
- Collaborative field work

### PC Link (v2.0 - GeoCalc DX)

**Game Boy ↔ PC Communication:**
- Upload data to computer
- Export to common formats (CSV, StereoWin)
- Backup/restore projects
- Print reports

**Hardware needed:**
- Custom link cable adapter
- MAX232 level shifter
- PC software (Windows)

### Color Version (v3.0)

**Game Boy Color Enhancements:**
- Color-coded measurement types
- Better screen visibility
- Real-time clock (MBC3)
- Timestamp measurements
- Increased RAM (200 measurements)

### Printer Support (v3.5)

**Game Boy Printer Integration:**
- Print stereonet at outcrop
- Include data table
- Tape into field notebook
- Physical backup of digital data

---

## Success Criteria

**Technical:**
- ✓ Boots on real hardware
- ✓ Accurate projection (±1°)
- ✓ Stable (no crashes)
- ✓ Fast response (<200ms for operations)
- ✓ Reliable SRAM save/load
- ✓ 30+ hour battery life

**Usability:**
- ✓ Intuitive controls (learn in <10 minutes)
- ✓ Readable in sunlight
- ✓ Usable with gloves
- ✓ Comparable accuracy to manual plotting

**Field Testing:**
- ✓ Survives drop test (6+ feet)
- ✓ Works in temperature range (-10°C to 40°C)
- ✓ Water-resistant (not waterproof, but survives rain)
- ✓ Used successfully in real field work
- ✓ Validated against manual stereonet plotting

---

## References

### Game Boy Programming

- **Pan Docs:** Complete Game Boy technical reference
- **RGBDS Documentation:** https://rgbds.gbdev.io/
- **GB Dev Wiki:** https://gbdev.gg8.se/wiki/
- **Awesome Game Boy Development:** Curated resources

### Structural Geology

- **Groshong, R.H.** (2006) *3-D Structural Geology*
- **Marshak, S. & Mitra, G.** (1988) *Basic Methods of Structural Geology*
- **Ragan, D.M.** (2009) *Structural Geology: An Introduction to Geometrical Techniques*

### Stereonet Software

- **StereoWin:** Windows stereonet software (reference)
- **Stereonet 9:** Mac stereonet application
- Historical manual plotting techniques

---

## Conclusion

GeoCalc Pocket represents a unique approach to field geology instrumentation: leveraging existing robust hardware (Game Boy) rather than building custom solutions. The constraints of the platform (limited CPU, memory, display) are offset by its advantages (durability, battery life, cost, availability).

By focusing on core functionality (accurate stereonet projection, reliable data storage, field-friendly interface), GeoCalc Pocket can provide genuine value to structural geologists while remaining practical to develop and manufacture.

The technical challenges are significant but solvable. The market opportunity is real. The tool serves a genuine need.

**Next step:** Proceed to implementation plan.
