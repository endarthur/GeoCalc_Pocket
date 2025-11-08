# Appendix A: Technical Specifications

## Game Boy Hardware

### DMG-01 (Original Game Boy, 1989)

**CPU:**
- Sharp LR35902 (Z80-derivative)
- Clock: 4.194304 MHz
- 8-bit architecture
- No hardware multiply/divide
- 1KB boot ROM (Nintendo logo check)

**Memory:**
- Work RAM: 8 KB ($C000-$DFFF)
- Video RAM: 8 KB ($8000-$9FFF)
- Sprite OAM: 160 bytes
- I/O Registers: 128 bytes
- High RAM: 127 bytes (fast access)

**Display:**
- Resolution: 160 × 144 pixels
- 4 shades: white, light gray, dark gray, black
- Reflective LCD (no backlight)
- Tile-based rendering
- 20 × 18 visible tiles (8×8 pixels each)
- Up to 40 sprites (8×8 or 8×16)

**Audio:**
- 4 channels (2 square, 1 wave, 1 noise)
- Stereo output
- Not used by GeoCalc (to save battery)

**Power:**
- 4× AA batteries
- 6V DC input (optional)
- Current draw: ~70mA typical
- Battery life: 15-30 hours (game dependent)

**Dimensions:**
- 90mm × 148mm × 32mm
- Weight: 220g with batteries

**Link Port:**
- 6-pin connector
- Serial communication (8192 Hz)
- Used for GeoCalc DX PC link

---

## GeoCalc Memory Map

### Cartridge (MBC1, 128 KB ROM + 32 KB SRAM)

**ROM Bank 0** (Fixed, $0000-$3FFF):
```
$0000-$00FF: Interrupt vectors
$0100-$014F: Header
$0150-$3FFF: Core engine
```

**ROM Banks 1-7** (Switchable, $4000-$7FFF):
```
Bank 1: Projection math
Bank 2: Tools (plot, rotate)
Bank 3: UI & data
Bank 4: Help screens
Bank 5: Lookup tables
Bank 6: Analysis
Bank 7: Reserved
```

**Work RAM** ($C000-$DFFF, 8 KB):
```
$C000-$CC3F: Shadow buffer (3,136 bytes)
$CC40-$CE0F: Measurement data (400 bytes)
$CE10-$D143: Lookup tables (820 bytes)
$D144-$D243: Stack (256 bytes)
$D244-$DFFF: Variables & temp (3,328 bytes)
```

**Cartridge SRAM** ($A000-$BFFF, 32 KB):
```
$A000-$A7FF: Saved measurements (2,048 bytes)
$A800-$AFFF: Project metadata (2,048 bytes)
$B000-$BFFF: Reserved (28,672 bytes)
```

---

## Data Structures

### Measurement Entry (4 bytes)

```c
typedef struct {
    uint8_t trend;   // 0-255 (maps to 0-359°)
    uint8_t plunge;  // 0-90
    uint8_t type;    // Bits 0-1: type (00=pole, 01=plane, 10=line)
                     //      2: selected flag
                     //      3-7: reserved
    uint8_t flags;   // Color, confidence, custom
} Measurement;
```

### Project Header

```c
typedef struct {
    char magic[4];        // "GCPK"
    uint8_t version;      // Format version (1)
    uint16_t count;       // Number of measurements
    char name[16];        // Project name
    char location[32];    // Location
    char date[8];         // YYYYMMDD
    char notes[64];       // Field notes
    uint16_t checksum;    // Data integrity
} ProjectHeader;
```

---

## Projection Mathematics

### Equal-Area (Schmidt) Projection

**Input:** Trend T (0-359°), Plunge P (0-90°)

**Algorithm:**
1. Convert to lower hemisphere (if needed)
2. Calculate radial distance:
   ```
   r = √2 × sin((90° - P) / 2)
   ```
3. Convert to screen coordinates:
   ```
   x = center_x + r × sin(T)
   y = center_y - r × cos(T)
   ```

**Implementation (8.8 fixed-point):**
```c
// Lookup tables (pre-computed)
int16_t sin_table[360];  // 8.8 fixed-point
int16_t radius_table[91]; // Pre-computed radii

// Plot function
void plot_pole(uint8_t trend, uint8_t plunge) {
    // Get radius from table
    int16_t r = radius_table[plunge];

    // Get sin/cos from tables
    int16_t sin_t = sin_table[trend];
    int16_t cos_t = sin_table[(trend + 90) % 360];

    // Calculate screen position (fixed-point multiply)
    int16_t x = STEREONET_CENTER_X + ((r * sin_t) >> 8);
    int16_t y = STEREONET_CENTER_Y - ((r * cos_t) >> 8);

    // Plot pixel
    plot_pixel(x, y);
}
```

**Accuracy:**
- Angular: ±1°
- Screen: ±1 pixel (≈1.6° at edge)

---

## Performance Metrics

### CPU Cycles (4.194 MHz)

| Operation | Cycles | Time | Frames |
|-----------|--------|------|--------|
| Plot single point | ~2,000 | 0.5ms | <1 |
| Plot great circle | ~41,400 | 10ms | 2-3 |
| Rotate 100 points | ~352,000 | 84ms | 5-8 |
| Full screen redraw | ~490,000 | 117ms | 7 |

**VBlank timing:**
- Active display: 65,664 cycles (144 scanlines)
- VBlank: 4,560 cycles (10 scanlines)
- Total frame: 70,224 cycles (16.67ms @ 59.7 Hz)

**Tile update limit:**
- Max ~26 tiles per VBlank (before tearing)
- Shadow buffer technique allows smooth updates

---

## Product Specifications

### GeoCalc Pocket (1994)

| Spec | Value |
|------|-------|
| Platform | Game Boy DMG-01 |
| ROM | 128 KB (MBC1) |
| SRAM | 32 KB (battery-backed) |
| Measurements | 100 (RAM), 512 (SRAM) |
| Screen | 160×144, 4 shades |
| Battery | 4× AA, 30+ hours |
| Price | $299.95 |

### GeoCalc DX (1996)

Added:
- Link cable to PC adapter (MAX232)
- GeoCalc Manager software (Windows 95/98)
- CSV/TXT export
- Price: $349.95

### GeoCalc Color (1998)

| Spec | Value |
|------|-------|
| Platform | Game Boy Color |
| ROM | 128 KB (MBC3 with RTC) |
| SRAM | 32 KB |
| Measurements | 200 (32 KB RAM) |
| Screen | 160×144, 32,768 colors |
| Features | Timestamps, color-coded |
| Battery | 2× AA, 15+ hours |
| Price | $349.95 |

### GeoCalc Advance (2001)

| Spec | Value |
|------|-------|
| Platform | Game Boy Advance |
| ROM | 256 KB (MBC3) |
| SRAM | 64 KB |
| Measurements | 500 |
| Screen | 240×160, 32,768 colors |
| Stereonet | 144×144 pixels |
| Battery | 2× AA, 15 hours |
| Price | $349.95 |

### GeoCalc Advance SP (2003)

Added:
- Frontlit screen
- Rechargeable Li-ion battery (10 hours)
- Clamshell design
- Includes GBA SP console
- Price: $399.95

---

## Link Cable Protocol

### Hardware

```
Game Boy Link Port (6-pin)
  Pin 1: VCC (5V)
  Pin 2: Serial Out
  Pin 3: Serial In
  Pin 4: Serial Data
  Pin 5: Serial Clock
  Pin 6: GND

Custom Cable
  ↓
MAX232 Level Shifter
  5V TTL ↔ RS-232 ±12V
  ↓
DB-9 Serial Connector
  ↓
PC COM Port (9600 baud, 8-N-1)
```

### Protocol

**Upload command:**
```
PC → GB: "UPLOAD\n"
GB → PC: "READY\n"
GB → PC: [Project Header]
GB → PC: [Measurement Data]
GB → PC: [Checksum]
PC → GB: "OK\n"
```

**Transfer speed:**
- 100 measurements: ~12 seconds
- Includes handshake, verification

---

## Game Boy Printer

### Specifications

- Resolution: 160×144 pixels
- Print method: Thermal
- Paper: 38mm wide thermal roll
- Print speed: 2-3 seconds per page
- Paper consumption: ~15cm per page
- Link cable communication
- Powered by 6× AA batteries

### Print Format (GeoCalc)

```
Header:      16 pixels (project name, date)
Stereonet:   112 pixels (main content)
Data table:  Variable (measurement list)
Footer:      16 pixels (statistics, rotation)
Total:       ~144 pixels (fits one screen)
```

---

## File Formats

### SRAM Save Format

```
[4 bytes] Magic: "GCPK"
[1 byte]  Version: 0x01
[2 bytes] Count: Number of measurements
[16 bytes] Name: Project name (null-terminated)
[32 bytes] Location
[8 bytes]  Date: YYYYMMDD
[64 bytes] Notes
[2 bytes]  Checksum
[N × 4]    Measurements (4 bytes each)
```

### CSV Export Format

```csv
#,Trend,Plunge,Type,Date,Time
1,045,32,POLE,1999-08-15,14:23
2,132,67,PLANE,1999-08-15,14:31
```

### StereoWin Export Format

```
STRIKE DIP TYPE
045 32 P
132 67 B
```

---

## Manufacturing Specifications

### Cartridge Components

**PCB:**
- 4-layer board
- 60mm × 50mm
- ENIG finish (gold contacts)

**ROM Chip:**
- 128 KB Flash EEPROM
- 5V programming voltage
- DIP-32 or TSOP-32 package

**SRAM Chip:**
- 32 KB static RAM
- Battery-backed (CR2032)
- Ultra-low power (<1µA standby)

**MBC Chip:**
- MBC1 or MBC3 (with RTC)
- Memory bank controller
- ROM/SRAM switching logic

**Battery:**
- CR2032 lithium coin cell
- 3V, ~220mAh
- Expected life: 10-20 years

**Shell:**
- ABS plastic
- Custom mold (Game Boy cartridge format)
- Label: Printed vinyl

### Manufacturing Cost (1994)

| Component | Cost |
|-----------|------|
| PCB | $4.00 |
| ROM chip | $5.00 |
| SRAM chip | $4.00 |
| MBC1 chip | $3.00 |
| Battery | $1.00 |
| Shell + label | $4.00 |
| Assembly | $4.00 |
| Testing | $2.00 |
| **Total** | **$27.00** |

Retail price: $299.95
Margin: ~91% (before overhead)

---

**Notes:**
- All specifications verified against original hardware
- Code samples simplified for clarity
- Memory maps accurate to byte-level
- Performance metrics measured on real hardware
