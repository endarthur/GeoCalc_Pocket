# Technical Reference

**Source:** Design documents, technical specifications
**Purpose:** Reference for technical details in book chapters

---

## Game Boy Hardware Specifications

### CPU
- **Processor:** Sharp LR35902 (Z80-like)
- **Clock Speed:** 4.194304 MHz (~4.19 MHz)
- **Architecture:** 8-bit
- **Instruction Set:** Z80-compatible with some differences
- **No hardware multiply/divide**

### Memory
- **Work RAM:** 8 KB ($C000-$DFFF)
- **Video RAM:** 8 KB ($8000-$9FFF)
- **Cartridge ROM:** 32 KB - 2 MB (banked)
- **Cartridge SRAM:** 0 - 32 KB (battery-backed for saves)

### Display
- **Resolution:** 160 × 144 pixels
- **Colors:** 4 shades of gray (greenish tint on original)
- **Tile-based:** 20 × 18 tiles (8×8 pixels each)
- **Sprites:** 40 objects (8×8 or 8×16 pixels)
- **Screen Type:** Reflective LCD (no backlight on DMG)

### Tile System
- **Total tiles available:** 256 unique 8×8 tiles
- **Tile data:** 16 bytes per tile (2 bits per pixel × 64 pixels)
- **Background map:** 32×32 tiles (256 bytes visible at once)
- **VRAM access:** Restricted during active display (Mode 3)

### Cartridge Types
- **MBC1:** Memory Bank Controller 1 (basic banking)
  - Up to 2 MB ROM, 32 KB SRAM
  - Used for GeoCalc Pocket, DX
- **MBC3:** With real-time clock
  - Used for GeoCalc Color
  - RTC for timestamping measurements
- **MBC5:** Enhanced banking (not used by GeoCalc)

### Input
- **D-pad:** Up, Down, Left, Right
- **Buttons:** A, B, Start, Select
- **No analog controls**

### Power
- **Battery:** 4× AA batteries
- **Battery Life:** 15-30 hours typical (varies by game)
- **Voltage:** ~6V from batteries, regulated to 5V internally

### Audio
- **4 channels:** 2 square waves, 1 wave, 1 noise
- **Not used** by GeoCalc (silence to save power)

---

## GeoCalc Memory Layout

### ROM Bank 0 (Fixed, $0000-$3FFF)
```
$0000-$00FF: Interrupt vectors
$0100-$014F: Header (title, checksums, etc.)
$0150-$3FFF: Core engine
  - Main loop
  - Input handling
  - VRAM management
  - Shadow buffer routines
```

### ROM Banks 1-7 (Switchable, $4000-$7FFF)
```
Bank 1: Projection math & lookup tables
Bank 2: Tool implementations (plot, rotate, etc.)
Bank 3: UI & data management
Bank 4: Help screens
Bank 5: Extended lookup tables
Bank 6: Analysis tools (statistics, etc.)
Bank 7: Reserved/future features
```

### Work RAM ($C000-$DFFF, 8 KB)
```
$C000-$CC3F: Shadow buffer (3,136 bytes)
$CC40-$CE0F: Measurement data (100 entries × 4 bytes = 400 bytes)
$CE10-$D143: Lookup tables (820 bytes)
$D144-$D243: Stack (256 bytes)
$D244-$DFFF: System variables & temp space
```

### VRAM ($8000-$9FFF, 8 KB)
```
$8000-$8C2F: Stereonet tiles (196 tiles × 16 bytes = 3,136 bytes)
$8C30-$8FFF: Font & UI tiles (60 tiles × 16 bytes = 960 bytes)
$9800-$9BFF: Background tilemap (1,024 bytes)
$9C00-$9FFF: Window tilemap (1,024 bytes)
```

### Cartridge SRAM ($A000-$BFFF, 32 KB)
```
$A000-$A7FF: Saved measurements (512 entries × 4 bytes = 2,048 bytes)
$A800-$AFFF: Project metadata (2,048 bytes)
$B000-$BFFF: Reserved (16,384 bytes)
```

---

## Data Structures

### Measurement Entry (4 bytes)
```c
struct Measurement {
    uint8_t trend;   // 0-255 (maps to 0-359°)
    uint8_t plunge;  // 0-90°
    uint8_t type;    // Bits: 0-1=type, 2=selected, 3-7=reserved
    uint8_t flags;   // User-defined (color, confidence, etc.)
};
```

### Project Header (in SRAM)
```c
struct ProjectHeader {
    char magic[4];        // "GCPK"
    uint8_t version;      // Format version
    uint16_t count;       // Number of measurements
    char name[16];        // Project name
    char location[32];    // Location description
    char date[8];         // Date (YYYYMMDD)
    char notes[64];       // Field notes
    uint16_t checksum;    // Data integrity check
};
```

---

## Projection Mathematics

### Equal-Area (Schmidt) Projection

**Input:** Trend (T) and Plunge (P) in degrees

**Output:** Screen coordinates (x, y) in pixels

**Algorithm:**
1. Convert to lower hemisphere (if P < 0, flip)
2. Calculate radial distance from center:
   ```
   r = √2 × sin((90° - P) / 2)
   ```
3. Calculate screen position:
   ```
   x = center_x + r × sin(T)
   y = center_y - r × cos(T)
   ```

**Implementation Details:**
- Use 8.8 fixed-point arithmetic (8 bits integer, 8 bits fraction)
- Pre-computed lookup tables for sin, cos (360 entries)
- Pre-computed radius table for plunge 0-90° (91 entries)
- √2 approximated as 1.414 (362/256 in fixed-point)

**Accuracy:**
- Angular precision: ±1°
- Screen precision: ±1 pixel (≈1.6° at stereonet edge)
- Sufficient for field geology applications

---

## Fixed-Point Arithmetic

### 8.8 Format
- **8 bits integer:** -128 to 127 (signed) or 0 to 255 (unsigned)
- **8 bits fraction:** 1/256 precision
- **Example:** 1.5 = 0x0180 (256 + 128 = 384 / 256 = 1.5)

### Operations
```
Addition:     (a + b) >> 0  (no shift needed)
Subtraction:  (a - b) >> 0
Multiply:     (a × b) >> 8  (shift right 8 bits)
Divide:       (a << 8) / b  (shift left 8 before divide)
```

### Lookup Tables
**Sine/Cosine Table:** 360 entries, 8.8 fixed-point
```
sin_table[0] = 0      (sin(0°) = 0.0)
sin_table[90] = 256   (sin(90°) = 1.0 in 8.8)
sin_table[180] = 0    (sin(180°) = 0.0)
sin_table[270] = -256 (sin(270°) = -1.0)
```

**Radius Table:** 91 entries (plunge 0-90°)
```
radius[0] = 256 × √2 × sin(45°) ≈ 362  (plunge 0° → edge)
radius[90] = 0                          (plunge 90° → center)
```

---

## Performance Metrics

### CPU Cycles
- **Clock:** 4.194304 MHz
- **Cycles per frame:** ~70,224 (at 59.7 Hz)
- **VBlank period:** ~4,560 cycles (only time for VRAM writes)

### Operation Costs
```
Plot single point:      ~2,000 cycles  (~0.5ms, <1 frame)
Plot great circle:      ~41,400 cycles (~10ms, 2-3 frames)
Rotate 100 points:      ~352,000 cycles (~84ms, ~8 frames)
Redraw screen (196 tiles): ~117ms (7-8 frames)
```

### Tile Updates
- **Maximum:** ~26 tiles per VBlank (to avoid screen tearing)
- **Strategy:** Dirty tile tracking, batch updates over multiple frames

---

## Link Cable Protocol

### Hardware
- **Game Boy Link Port:** 6-pin connector
- **Signal:** Serial data transfer
- **Voltage:** 5V TTL

### GeoCalc DX Link Cable Adapter
```
Game Boy Link Port → Custom cable → MAX232 (level shifter) → DB-9 serial (RS-232)
```

### Protocol
- **Baud rate:** 9600 bps
- **Data format:** 8-N-1 (8 data bits, no parity, 1 stop bit)
- **Flow control:** None (simple request-response)

### Transfer Speed
- **100 measurements:** ~12 seconds
- **Includes:** Handshake, data verification, checksum

---

## Game Boy Color Enhancements

### Hardware Improvements
- **CPU:** Same (backwards compatible) but 2× speed mode available
- **RAM:** 32 KB (vs 8 KB) → More measurements (200 vs 100)
- **Palette:** 32,768 colors (vs 4 shades) → Color-coded data
- **MBC3 RTC:** Real-time clock for timestamps

### Color Palette (GeoCalc Color)
```
Poles:       Red shades   (#FF0000, #CC0000, #990000)
Planes:      Blue shades  (#0000FF, #0000CC, #000099)
Lines:       Green shades (#00FF00, #00CC00, #009900)
Mean vector: Orange/yellow (#FFAA00)
Grid:        Gray shades  (#666666, #999999)
```

---

## Game Boy Printer

### Specifications
- **Resolution:** 160 × 144 pixels (matches screen)
- **Thermal printer:** Heat-sensitive paper
- **Paper width:** 38mm (~1.5 inches)
- **Print speed:** ~2-3 seconds per page

### Print Format
```
┌────────────────────────────────┐
│ Header (16px)                  │
│ GEOCALC - Project Name         │
├────────────────────────────────┤
│ Main content (112px)           │
│ [Stereonet graphic]            │
├────────────────────────────────┤
│ Data table                     │
│ # | Trend | Plunge | Type      │
├────────────────────────────────┤
│ Footer                         │
│ Mean: 127/45  Count: 42        │
└────────────────────────────────┘
```

**Paper consumption:** ~15cm per page

---

## Product Specifications Summary

| Feature | Pocket | DX | Color | Advance | Advance SP |
|---------|--------|----|----|---------|------------|
| **Platform** | DMG | DMG/Pocket | GBC | GBA | GBA SP |
| **Year** | 1994 | 1996 | 1998 | 2001 | 2003 |
| **Measurements** | 100 | 100 | 200 | 500 | 500 |
| **PC Link** | No | Yes | Yes | Yes | Yes |
| **Printer** | No | No | Yes | Yes | Yes |
| **RTC** | No | No | Yes | Yes | Yes |
| **Screen** | 160×144 4-gray | Same | 160×144 color | 240×160 color | 240×160 frontlit |
| **Battery** | 4×AA 30hr | 4×AA 30hr | 2×AA 15hr | 2×AA 15hr | Li-ion 10hr rechargeable |
| **Price** | $299 | $349 | $349 | $349 | $399 (w/ console) |

---

**Note:** This technical reference is for book appendices and chapter footnotes. Detailed explanations should be accessible to non-programmers, with code samples reserved for appendices.
