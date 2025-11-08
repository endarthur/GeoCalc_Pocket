# Book Consistency Issues - Audit Document

## CRITICAL ISSUES FOUND

### 1. Maggie's PhD Status (CONFIRMED INCONSISTENCY)

**Location:** `book/chapters/01_the_garage_years.md`

**Line 79:** "By December, Maggie had submitted her dissertation and rented the garage."
- STATUS: She FINISHED her PhD ✓

**Line 233:** "But they'd already invested three years and $68,000. Maggie had quit her PhD."
- STATUS: Says she QUIT ✗

**FIX NEEDED:** Change line 233 to reflect that she finished her PhD

---

### 2. Development Timeline Inconsistency (POTENTIAL ISSUE)

**Chapter 3 - The Prototype:**
- January 3, 1992: Bob buys Game Boys
- 8 weeks later (March 1992): Prototype complete
- **Line 417:** "Six months," Bob said. "December 1993. We can have a real product by then."

**PROBLEM:** From March 1992 to December 1993 is 21 months, not 6 months.

**Chapter 4 - Development:**
- **Line 7:** "March 1993, nine months into development"
- If 9 months into development = March 1993
- Then development started = June 1992

**ANALYSIS:**
- Prototype: March 1992
- Full development starts: June 1992 (after securing funding?)
- 9 months later: March 1993 (hiring crisis)
- Actual launch: February 12, 1994

**LIKELY ERROR:** Chapter 3, line 417 should say "December **1992**" not "December **1993**"
- March 1992 + 6 months = September 1992
- Saying "December 1992" gives them 9 months (more realistic estimate)
- They end up being late, shipping in Feb 1994 (realistic for software projects)

---

### 3. Kevin Join Date

**Chapter 9, line 41:** "Kevin had been with the company since 1992"
**Chapter 4:** Kevin is hired during the development (which started June 1992)

**CONSISTENT** - Just need to verify Kevin joined mid-1992

---

## TIMELINE RECONSTRUCTION

### Confirmed Dates:
- **September 1985:** Founders meet at Colorado School of Mines
- **December 1985:** Decide to start company
- **Late 1986:** Maggie submits dissertation, Bob quits postdoc
- **January 1987:** First product ships (FieldLogger I)
- **March 1987:** Opening scene (4 months from bankruptcy)
- **October 1988:** StrikeCalc ships (disaster)
- **November 1988:** Denny's meeting, rename to GeoStructure Systems
- **November 1990:** FieldLogger II ships
- **March 1991:** Crisis point again
- **Easter 1991:** Bob sees Danny's beat-up Game Boy
- **December 26, 1991:** Danny drops Game Boy (THE REVELATION)
- **January 3, 1992:** Bob buys two Game Boys
- **January 1992:** Pitch meeting at climbing gym
- **January-March 1992:** 8-week prototype sprint
- **March 1992:** Prototype demo
- **June 1992:** Full development begins (after securing funding)
- **Mid-1992:** Hire Kevin Park, Sarah Martinez, Lisa Rodriguez
- **September 1993:** The one-byte bug crisis
- **January 1994:** Cartridges arrive
- **February 12, 1994:** GeoCalc Pocket launches

### Years Active:
- **1994-2013:** GeoCalc product line
- **2004:** Trimble acquisition ($23M)
- **2010:** AGU Conference (15 years of GeoCalc)
- **2011:** Kevin starts AeroGeo 3D
- **2012:** AeroGeo 3D launches at AGU
- **2023:** Bob's blog post (32 years since the drop)

---

## OTHER CONSISTENCY CHECKS NEEDED

### Character Ages (need to verify):
- Bob's age throughout the story
- Maggie's age (52 in 2010, so born ~1958, would be 29 in 1987 - PhD finishing age seems right)
- Danny: 9 in 1991, so 41 in 2023 ✓

### Technical Consistency:
- Game Boy specs consistent? (need to verify)
- Memory layouts consistent? (need to verify)

### Product Launch Dates:
- GeoCalc Pocket: 1994 ✓
- GeoCalc DX: 1996 (mentioned in Chapter 7)
- GeoCalc Color: 1998 ✓
- GeoCalc Advance: 2001 ✓
- GeoCalc Advance SP: 2003 ✓

---

## PRIORITY FIXES

1. **HIGH PRIORITY:** Fix Maggie PhD contradiction (line 233, Chapter 1)
2. **MEDIUM PRIORITY:** Fix "December 1993" to "December 1992" (line 417, Chapter 3)
3. **LOW PRIORITY:** Verify all other timeline details

---

*Created: 2025-11-08*
*Status: In Progress*
