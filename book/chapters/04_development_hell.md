# Development Hell

## The Team Expands

"We need more people."

Maggie said it in March 1993, nine months into development. Bob, James, and Mark had gotten the prototype working, but turning it into a shippable product was a different problem entirely.

They needed someone who understood geology. Bob and James were engineers who knew *about* geology. They needed someone who *did* geology, who would use this tool in the field, who could tell them when they were building the wrong thing.

They needed someone who understood users. Someone who could write documentation, design interfaces, explain complex concepts clearly.

And they needed another programmer, because Mark was going back to his real job. He'd done his part—got them off the ground. But he had a mortgage and a family and couldn't keep working for equity and hope.

Maggie found Sarah Blackburn through the geology network. Dr. Sarah Blackburn, age 34, PhD from Stanford, currently at Chevron doing structural analysis for petroleum exploration. Good pay, soul-crushing work. She was tired of computers that only worked at headquarters.

The pitch meeting was at a coffee shop in Golden.

"I want to show you something," Maggie said, pulling out a Game Boy.

Sarah looked skeptical. "You know I don't have kids, right?"

"It's not a game. It's a stereonet plotter."

Maggie demonstrated. Entered measurements, plotted points, showed the data table.

Sarah's skepticism melted into fascination. "This is... wait. Is this actually calculating the equal-area projection?"

"In real time."

"On a Game Boy."

"On a Game Boy."

Sarah took the device, navigated through the menus. Tested the data entry. Plotted a few points from memory—measurements she'd taken last month at an outcrop in Wyoming.

"The accuracy?"

"Plus or minus one degree."

"That's better than my hand plotting."

"That's the idea."

Sarah looked up. "What do you need from me?"

"Product manager. Field tester. Geological consultant. Someone who will break things in creative ways and tell us how to fix them."

"Salary?"

Maggie told her. It was less than half what Chevron paid.

Sarah thought for exactly five seconds. "When do I start?"

"Today, if you can."

Sarah closed the Game Boy, handed it back. "I've been a geologist to see rocks, not PowerPoint slides. I'm in."

---

Lisa Nakamura came via a different route. She'd been freelancing for Nintendo Power, writing strategy guides. She understood Game Boy hardware intimately, could explain complex systems clearly, and had opinions about interface design.

James found her through a friend of a friend. Her portfolio included guides for *Super Mario Land 2*, *Kirby's Dream Land*, and *The Legend of Zelda: Link's Awakening*.

"You've written for Nintendo?" Bob asked during the interview.

"Freelance. Not employee. Different thing."

"Do they care if you work on... unlicensed software?"

"As long as I'm not stealing code or using their trademarks, no. This is just programming, right? Not piracy?"

"Just programming."

Lisa picked up the GeoCalc prototype. "Can I try it?"

She spent ten minutes navigating the interface. Didn't say a word. Just tested everything, pressed every button, found every edge case.

Finally: "Your data entry UI is backwards."

"What?"

"Trend goes up when you press up. But you're thinking of it like adjusting a number. Users will think of it spatially. North is up. You should map the D-pad to cardinal directions for trend adjustment. Press up to increase north-trending, right to increase east-trending."

Bob and James looked at each other.

"We've been building this for nine months," James said slowly. "And we never thought of that."

"That's why you need me," Lisa said. "When do I start?"

---

Kevin Park was 23, fresh out of CU Boulder with a CS degree. His resume mentioned a Game Boy emulator he'd written for fun.

"You wrote a Game Boy emulator?" Bob asked.

"It's not very good. Doesn't do sound. Video timing is off. But it runs *Tetris*."

"You reverse-engineered the Game Boy hardware well enough to write an emulator."

"I mean, yeah? The Pan Docs helped. And I had to read a lot of ROM dumps to figure out edge cases."

"You're hired."

"You haven't asked about my salary requirements."

"We can't afford them anyway. You're still hired."

Kevin started the next week. Mark trained him for two days, then handed over the codebase.

"It's messy," Mark warned. "We were learning as we went. There's no documentation. Comments are sparse. Good luck."

Kevin dug in. Within a week, he'd found three bugs and optimized the rendering pipeline.

"Why were you using two writes here?" he asked Bob. "You can do this in one operation."

Bob looked at the code. Kevin was right.

"I didn't know you could do that."

"It's in the Pan Docs. Chapter 4."

Bob had missed it. Kevin hadn't.

The team was now six: Maggie (CEO), Bob (CTO), James (VP Engineering), Sarah (Product Manager), Lisa (Documentation/UX), Kevin (Lead Programmer).

Budget: $21,000/month burn rate.
Runway: 11 months.
Pressure: Immense.

They had to ship by December.

## The Tile Crisis Revisited

Sarah's first week was spent field testing the prototype.

She took it to outcrops around Golden. Measured orientations with her Brunton compass, entered them into the GeoCalc, plotted them in real time.

She came back with notes. Lots of notes.

"The stereonet is good," she said. "Accuracy is fine. Data entry is awkward but functional. But we have a problem."

"What problem?"

"The numbers are too small."

She showed them a photo. The screen in bright sunlight, the tiny font barely visible.

"In the field, you're wearing sunglasses. Squinting in glare. The contrast isn't great. I can barely read the coordinates."

Lisa looked at the font. "We're using an 8×8 font. We could go to 8×16, double the height."

"How many tiles?" Bob asked.

"Double. Eighty tiles instead of forty."

"We only have sixty tiles available after the stereonet."

"Then we need to reduce the stereonet tiles."

Bob pulled up the tile graphics. They'd already optimized the stereonet to 196 tiles. The absolute minimum.

Or was it?

Lisa studied the design. "What if we generate some tiles procedurally? Simple patterns—horizontal lines, vertical lines, crosshatches. We could encode those in code instead of storing them as tiles."

"That's clever," Kevin said. "We'd save maybe... twenty tiles?"

"Twenty tiles isn't enough," Sarah said. "We need subscript numbers. Degree symbols. Better icons for measurement types. We need at least another forty tiles."

"We don't have another forty tiles."

Sarah picked up the Game Boy. "Then we need to choose: readability or features?"

The room was quiet.

"Readability," Maggie said. "Always readability. If people can't use it in the field, none of the features matter."

So they cut. No lowercase (already planned). No fancy symbols. Every character reconsidered. Did they REALLY need that punctuation mark? That icon?

Lisa designed a minimal font. Brutal efficiency. Every pixel justified.

Final tile budget:
- Stereonet: 196 tiles
- Font: 36 tiles (A-Z, 0-9, essential symbols only)
- UI elements: 24 tiles

Total: 256 tiles. Exactly the limit.

"We're one tile over budget for features," Kevin said.

"Then we cut one tile," Maggie replied.

They did. Some UI element nobody would miss.

The new font was ugly but readable. In sunlight, with sunglasses, it worked.

Field usability won.

## The Shadow Buffer Insight

The performance was bad.

Not terrible. But bad.

Kevin profiled it: plotting 50 points took about 100ms. The screen would freeze noticeably when rotating data. Redrawing the stereonet took 150ms. Users would see flicker and lag.

"We're writing to VRAM too often," Kevin said. "Every frame, we're updating tiles. But VRAM writes during active display cause artifacts. We have to wait for VBlank, but VBlank is only 4,560 clock cycles. We can only update about 26 tiles per frame without tearing."

"Can we update less often?" James asked.

"We are. But the stereonet is 196 tiles. That's eight frames minimum to redraw completely. At sixty frames per second, that's 133 milliseconds. Users will see it."

Bob had an idea. "What if we don't write to VRAM during active display at all?"

"Then where do we draw?"

"Work RAM. We maintain a shadow buffer—an exact copy of the tile data in regular RAM. We do all our drawing there. Then during VBlank, we copy dirty tiles to VRAM."

Kevin thought about it. "That's... 3,136 bytes of RAM for the shadow buffer. We have eight kilobytes total."

"We can spare it."

"But we still have to copy tiles. That's the slow part."

"Right. But now we can optimize which tiles we copy. If a tile hasn't changed, don't copy it. Track dirty flags. Only update what's changed."

Kevin's eyes lit up. "That would work. We'd only update modified tiles. For normal operations, that might be ten to twenty tiles per frame. We could do that in VBlank easily."

"Exactly."

Kevin implemented it in three days. The difference was dramatic.

No more flicker. No more lag. Plotting 100 points was smooth. Rotation was instant. The screen updated at full 60fps with no artifacts.

"This," Kevin said, "is the magic that makes everything else possible."

He was right. The shadow buffer technique would underpin every version of GeoCalc, from the 1994 original through the 2010 GBA SP edition.

It was Bob's best contribution to the technical architecture. Not the projection math, not the data structures, but this: a simple idea that made everything smooth.

## Bob's 4 AM Crisis

November 1992. Three months before deadline. The code wouldn't compile cleanly.

Not big errors. Little ones. Mysterious ones. A VRAM corruption bug that surfaced randomly. Memory alignment issues. Stack overflow in rare edge cases.

Bob hadn't slept more than four hours a night in weeks.

At 4 AM on a Tuesday, something broke inside him.

He threw his Game Boy across the room.

Not tossed. Not dropped. Threw, with force, with anger, with desperation.

It hit the wall. Hard. Plastic cracked. Definitely cracked.

Bob stared at what he'd done.

Then he heard it: the Tetris theme song. Still playing. The Game Boy, lying on the floor where it had bounced after hitting the wall, was still working.

Bob started laughing. Hysterical, exhausted, delirious laughter.

James found him fifteen minutes later, sitting on the floor, holding the Game Boy, laughing and crying simultaneously.

"You okay?" James asked.

"I threw it as hard as I could," Bob said. "As hard as I could. And it's fine. Screen's cracked but it works. We literally cannot break these things."

"That's... good?"

"We're going to make it," Bob said. "We literally cannot break these things. We're going to make it."

James sat down next to him. "We're going to make it."

They sat there for a while, two exhausted engineers on a basement floor, holding an indestructible toy, betting their company on an absurd idea.

The next morning, Bob hung the cracked Game Boy on the wall next to his desk.

It's still there, in his home office. The dent in the wall behind it is still there too.

Proof that some things can't be broken. Proof that they were going to make it.

## The Bug That Ate September

September 1993. The bug appeared.

Random crashes when plotting the 47th measurement.

Not the 46th. Not the 48th. Exactly the 47th.

Every time. Perfectly reproducible.

Kevin spent three days debugging. No obvious stack overflow. No memory corruption detected. The code looked fine.

But plotting the 47th point crashed the system.

He added debug code. Printed memory values. Traced execution paths.

On day three, at 2 AM, he found it.

The dirty tile queue—the list of tiles that needed updating during VBlank—was sized for 50 tiles. Reasonable, right?

But the check for "is the queue full?" used `cp 50` instead of `cp 49`.

Array indexing is zero-based. The 50th item is at index 49. But the code checked if the index equaled 50.

The 47th measurement was the first one that triggered a complex great circle requiring exactly 50 tile updates.

Overflow. Write into the stack. Next function return jumps to garbage. Crash.

**Fix:** Change `50` to `49`. One byte. One number.

**Time to find:** 3 days.
**Time to fix:** 30 seconds.
**Lines of code involved:** 1.

Kevin submitted the fix. Bob reviewed it.

"This is why we drink," James said.

They drank.

## Lisa's Interface Revolution

Lisa had been watching users test the prototype. Well, "users"—mostly Sarah, sometimes Maggie, occasionally geology grad students they'd recruited.

Everyone struggled with the same thing: data entry.

The current system:
- Press Start to enter data entry mode
- D-pad up/down adjusts trend (0-359)
- D-pad left/right adjusts plunge (0-90)
- A confirms, B cancels

"It's too many steps," Lisa said. "From seeing an outcrop to having the measurement plotted is six button presses minimum."

"How do we reduce it?" Bob asked.

Lisa showed them a mockup. "We change the mental model. The SELECT button toggles tools. There are four tools: Plot, Rotate, Stats, and Data. When Plot is active, the cursor appears on the stereo net. You move it with the D-pad. When it's where you want it, press A. The point plots immediately. You can see the result in real time."

"But how do they enter precise numbers?"

"They don't. Not for the first pass. They move the cursor to approximately where the measurement should be, plot it, then refine. Or they use the data table to enter exact values later."

Sarah tested it. Loved it.

"This is how I think in the field," she said. "I'm not thinking 'trend 045, plunge 32.' I'm thinking 'dipping southeast, moderate angle.' I'd move the cursor southeast, mid-range, plot. Done."

They implemented Lisa's design. It was revolutionary.

Time to plot a point dropped from 6 button presses to 2 (move cursor, press A).

Field usability improved dramatically.

"This," Maggie said, "is why we hired you."

## December: The Deadline Approaches

They weren't going to make December.

The code was stable but incomplete. Core features worked. Edge cases didn't. Error handling was minimal. The UI was functional but rough.

Manufacturing wasn't ready. They hadn't even ordered cartridge components.

"We need to push the launch," Bob said.

"To when?" Maggie asked.

"February. Maybe March. We need two more months minimum."

Maggie looked at the budget. They could afford two more months. Barely.

"February," she decided. "We launch in February. Final deadline. No extensions."

Bob nodded. "February."

They had eight weeks left. Again.

This time, they knew they could do it.

They'd done it before.

## The Manufacturing Nightmare

They found a manufacturer in Shenzhen who could produce 500 cartridges for $12,500. PCBs, components, assembly, the works.

The cartridges arrived in early January 1994.

Bob tested one.

Crash.

Tested another.

Crash.

Tested ten more.

Seven crashed immediately. Three worked.

"No," Bob said. "No, no, no."

He opened a cartridge. The EEPROM—the chip containing the game code—had been programmed with the wrong voltage. 40% of the ROMs were corrupted.

He called the factory. It was 3 AM in China.

Using a mix of broken Mandarin, frantic English, and faxed diagrams, he explained the problem.

"We fix," the manager said. "Send new batch. Two week."

"We don't HAVE two weeks. We launch in three weeks!"

"Two week. Best I can do."

Bob hung up.

The team hand-tested all 500 cartridges. 203 worked perfectly. 297 were garbage.

They needed 330 units for launch (180 pre-orders, 50 demos, 100 retail).

They were 127 short.

"We reprogram them ourselves," James said.

"We have one EEPROM programmer. It takes eight minutes per cartridge."

James did math. "17 hours of continuous programming. We work in shifts."

They worked in shifts.

For three days, someone was always at the programmer. Bob, Kevin, James, Sarah, Lisa, even Maggie learned how to program EPROMs.

They slept under desks. Pizza boxes accumulated. The climbing gym downstairs complained about noise at 2 AM.

By day three, they had 330 working cartridges.

"Never again," James said, delirious with exhaustion. "NEVER. AGAIN."

(Spoiler: This exact thing happened again in 1996 with the DX launch.)

## February 1994: Launch

GeoCalc Pocket launched on February 12, 1994.

Price: $299.95.
Includes: Cartridge, field case, 64-page manual.

First year sales: 380 units.

It wasn't a massive success. But it was success.

They'd done it. Built a professional geological instrument on a Game Boy. Shipped it. People bought it.

The reviews were mixed. Some geologists loved it. Some thought it was a toy. But nobody could deny: it worked.

Bob kept the first production unit. Serial number 001. It sits on his desk to this day, next to the cracked prototype.

Proof that crazy ideas sometimes work.

Proof that constraints drive innovation.

Proof that sometimes, the best tool is the one that doesn't break when you drop it down the stairs.
