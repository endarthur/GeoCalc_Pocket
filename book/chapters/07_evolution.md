# Evolution

## 1996: The Link Cable Revolution

The feature request was constant: "How do I get this data to my computer?"

Manual transcription worked. Barely. Type each measurement from the GeoCalc screen into Excel. For 50 measurements, this took 20 minutes. Tedious but doable.

For 100 measurements? Annoying.

For 500 measurements across multiple projects? Soul-crushing.

James became obsessed with solving this.

The Game Boy had a link port. Originally designed for connecting two Game Boys for multiplayer games. Six pins on the side of the unit. Serial communication protocol.

"We can use this," James said.

"For what?" Bob asked.

"PC connection. We build an adapter cable. Game Boy link port on one end, PC serial port on the other. Upload data directly."

"That's not how the link port works."

"We make it work."

James spent two months in early 1996 designing the adapter.

**The technical challenge:**

The Game Boy link port used 5V TTL logic. PC serial ports (RS-232) used ±12V signaling. You couldn't just connect them. You needed level shifting.

His solution:
```
Game Boy Link Port (6-pin)
  ↓
Custom cable
  ↓
MAX232 chip (voltage level shifter)
  ↓
DB-9 serial connector
  ↓
PC COM port
```

**Manufacturing cost:**
- MAX232 chip: $2.00
- Resistors, capacitors: $1.50
- Custom cable: $3.00
- DB-9 connector: $2.00
- PCB assembly: $4.00
- **Total: $12.50 per unit**

The software side: Kevin wrote "GeoCalc Manager" in Visual Basic 4.0. A Windows 95/98 application that could:
- Upload data from cartridge (12 seconds for 100 measurements)
- Export to CSV, TXT
- Export to StereoWin format (popular stereonet software)
- Basic visualization
- Print reports

**GeoCalc DX launched in June 1996.**

Price: $349.95 (included link cable adapter and software).

This was the breakthrough.

Users could now:
1. Collect data in field
2. Return to office
3. Plug GeoCalc into PC
4. Upload all data in seconds
5. Export to their preferred analysis software

No manual transcription. No typos. No tedium.

The thesis save happened during GeoCalc DX beta testing. Jennifer Wu's data was recoverable because she had the link cable to extract it.

**Sales:**
- 1996: 2,100 units
- 1997: 3,400 units
- 1998: 4,200 units

The link cable was the killer feature. Even users with original GeoCalc Pocket bought DX just for the data export.

## 1998: Enter the Color

The Game Boy Color launched in 1998. Nintendo's upgrade to the original Game Boy:
- Color display (32,768 colors vs 4 shades of gray)
- 2× speed mode (optional double CPU speed)
- 32 KB RAM (vs 8 KB)
- Backward compatible with original Game Boy games

GeoStructure's decision: support it.

**GeoCalc Color launched November 1998.**

**New features:**

**Color-coded data:**
- Poles: Red
- Planes: Blue
- Lines: Green
- Mean vector: Orange
- Grid: Gray

"Color should convey information, not just look pretty," Lisa said.

The palette was carefully chosen for clarity, even for colorblind users.

**200 measurement capacity:**
With 32 KB RAM (vs 8 KB), they could store twice as many measurements.

**Real-time clock:**
The MBC3 cartridge chip included an RTC. Every measurement got timestamped automatically:
- Date
- Time
- Session counter

Export format now included:
```
#, Strike, Dip, Type, Date, Time
01, 045, 32, POLE, 1999-08-15, 14:23
02, 132, 67, PLANE, 1999-08-15, 14:31
```

Useful for field notebooks: "All measurements from August 15."

**Backward compatibility:**
The cartridge worked in original Game Boy (grayscale, no RTC, 100 measurements). Enhanced on GBC.

Price: $349.95 (same as DX, but with color and more features).

**Sales:**
- 1998: 1,800 units
- 1999: 5,200 units (printer support boost)
- 2000: 6,100 units (peak sales year)

## 1999: The Printer

Sarah had been asking about printing since 1994.

"When can we print in the field?"

"When Nintendo releases a printer."

In February 1998, Nintendo released the Game Boy Printer. A thermal printer that connected via link cable. Printed 160×144 pixel images on thermal paper.

GeoStructure immediately started development.

**GeoCalc Color v2.0 - Free firmware update** (users mailed in cartridges, GeoStructure reprogrammed and returned).

**Print format:**
```
┌────────────────────────────────┐
│ GEOCALC COLOR - Site: BH-23    │
│ Date: 1999-08-15  14:45        │
├────────────────────────────────┤
│                                │
│      [Stereonet graphic]       │
│         112×112 pixels         │
│                                │
├────────────────────────────────┤
│ Points: 47  Mean: 135/32       │
│                                │
│ 001: 045/32  POLE  14:23       │
│ 002: 132/67  PLANE 14:31       │
│ ... (continues)                │
├────────────────────────────────┤
│ Rotation: T000 P00             │
└────────────────────────────────┘
```

Print time: ~3 seconds
Paper consumption: ~15cm per page

**The workflow:**
1. Collect measurements at outcrop
2. Plot on GeoCalc
3. Print stereonet
4. Tape printout into field notebook
5. Digital data in cartridge, physical copy in notebook

Triple redundancy. If one failed, two backups remained.

**Sarah's field test (Maroon Bells, Colorado, July 1999):**

She plotted 23 measurements of foliation. Printed the stereonet at the outcrop. Taped it into her field notebook. Took a photo for documentation.

Her report:

> "This is it. This is what I've been waiting for. The digital data lives on the cartridge. The printout lives in my notebook. The photo lives in my camera. Triple redundancy. If I lose any one, I still have the data. This is how field geology should work."

**Marketing impact:**

Print-at-outcrop became THE selling point. Sales doubled in 1999-2000.

**Testimonial from Dr. Patricia Morrison (UC Berkeley):**
> "My students can plot data, see the pattern, print it, and tape it in their notebooks—all without leaving the outcrop. They understand stereonets viscerally now. This is transformative for teaching."

## 2001: The GBA Advance

The Game Boy Advance launched in 2001. Major upgrade:
- 240×160 screen (50% larger, color)
- ARM7 processor (much faster)
- 32 KB RAM
- Shoulder buttons (L/R)

**GeoCalc Advance launched June 2001.**

**New features:**

**Larger stereonet:**
144×144 pixels (vs 112×112), more detail

**Faster processing:**
The ARM processor rendered great circles instantly. Rotation of 100 points: smooth 60fps.

**500 measurement capacity:**
With better RAM management, they could store 5× more data.

**Better graphics:**
Anti-aliased lines, smoother rendering.

**Shoulder buttons:**
L/R for quick tool switching. More ergonomic.

**The tradeoff:**
Battery life dropped to 15 hours (vs 30 on original GB).

"Still acceptable," Sarah said. "Bring spare AAs."

Price: $349.95

**Sales:** Steady but not explosive. Universities bought it. Professional geologists upgraded. But the GBC version was good enough for many users.

## 2003: The Ultimate Field Tool

The Game Boy Advance SP changed everything.

**Why the SP was different:**

1. **Clamshell design** - Screen protected when closed
2. **Frontlit screen** - Readable in dark (caves, mines, dawn/dusk)
3. **Rechargeable battery** - No more AA logistics
4. **10-hour battery** - Full field day on one charge
5. **Compact** - Fits in shirt pocket

**GeoCalc Advance SP Edition launched March 2003.**

Price: $399.95 (included GBA SP console + GeoCalc cartridge)

**Sarah's field test (abandoned mine, April 2003):**

> "Tested in an abandoned mine at 2 AM (don't ask why). The frontlight means I can actually SEE the screen without juggling my headlamp. Took measurements in complete darkness. This is what we should have had all along.
>
> Battery lasted 9.5 hours of continuous use. Survived being dropped twice (once onto rocks, once into mud). Screen was still visible in direct sunlight at noon.
>
> If you're a serious field geologist and you do structural analysis, this is the version to get. Period."

**Marketing ad (GSA Today, full page):**
```
┌─────────────────────────────────┐
│  GeoCalc Advance SP              │
│                                  │
│  "The Last Field Computer        │
│   You'll Ever Need"              │
│                                  │
│  [Photo: SP in a cave, screen    │
│   clearly visible in darkness]   │
│                                  │
│  $399.95 - Includes Console      │
└─────────────────────────────────┘
```

This was peak GeoCalc. The perfect combination of hardware and software.

Sales to professional geologists spiked. Universities standardized on it. It became the field tool for structural geology.

The SP version continued selling until 2010—seven years of production. Longer than any other GeoCalc version.

## The Platform That Never Was

In 2004, Nintendo announced the Nintendo DS. Dual screens, touch screen, wireless.

Initial excitement from the team:
- **Bob:** "Dual screens—one for stereonet, one for data!"
- **Kevin:** "Touch screen—better than D-pad!"
- **James:** "This could be amazing."

Kevin built a proof-of-concept. It worked beautifully.

Top screen: Stereonet
Bottom screen: Data table with stylus input

**But:**

Internal memo from Bob (August 2004):

> We've had a good run with Nintendo hardware. But the DS isn't right for us.
>
> Problems:
> - Battery life worse than GBA SP (3-5 hours vs 10)
> - Dual screens solve a problem we don't have
> - Touch screen is nice but Palm/PocketPC already has it
> - Market has moved on—smartphones are coming
> - We'd be fighting the gaming identity even harder
>
> Our users are moving to Windows Mobile, Palm, and soon smartphones. That's where we should focus development.
>
> DS would be cool. But cool doesn't pay the bills.
>
> Recommendation: Don't develop for DS. Final Game Boy platform is GBA SP. Support it until 2010, then sunset all Nintendo development.

Vote: Unanimous agreement.

The DS would have been fun. But it wasn't the future.

**Final Nintendo Product:** GeoCalc Advance SP (2003-2010)

## By The Numbers (Nintendo Platforms, 1994-2010)

**Total units sold:** 74,580
**Total revenue:** $26.2 million
**Development cost:** ~$500K over 16 years
**Support cost:** ~$2M over 16 years

**Units by product:**
- GeoCalc Pocket (1994-1998): 4,080 units
- GeoCalc DX (1996-2000): 15,000 units
- GeoCalc Color (1998-2004): 24,100 units
- GeoCalc Advance (2001-2008): 19,700 units
- GeoCalc Advance SP (2003-2010): 11,600 units

**Platform breakdown:**
- Original Game Boy: 19,080 units (Pocket + DX)
- Game Boy Color: 24,100 units
- Game Boy Advance: 31,300 units (Advance + SP)

The evolution was steady. Not explosive growth. Not viral adoption. Just solid, sustained sales to a community that needed the tool.

Each version sold better than the last (except SP, which was a subset of Advance market).

Each version refined the concept: more measurements, better display, easier export, more reliability.

The core never changed: plot stereonets in the field on hardware that doesn't break.

That was enough.

## What They Learned

Years later, Bob reflected on the evolution:

"We learned that features matter less than reliability. Users didn't upgrade for fancier algorithms. They upgraded for longer battery life, better displays, easier data export.

"We learned that backward compatibility matters. Users had years of data on old cartridges. We supported those cartridges in new hardware. That loyalty paid off.

"We learned that the platform doesn't matter as much as the execution. Game Boy, GBA, Palm, smartphones—the hardware changed, but the core value stayed the same: reliable field data collection.

"Most importantly, we learned when to stop. The DS would have been fun. But fun isn't a business strategy. We shipped the SP, supported it for seven years, and when the market moved to smartphones, we moved too.

"The Game Boy era ended in 2010. We rode that platform for sixteen years. From a desperate pivot in a failing company to an industry standard tool.

"Not bad for a video game console."

Not bad at all.
