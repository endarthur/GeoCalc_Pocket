# Eight Weeks

## Week One: The Tools

Bob bought two Game Boys on January 3rd, 1992. One from a pawn shop on Colfax ($35), one from a garage sale ad in the paper ($25). Both worked perfectly. Both came with *Tetris*. The pawn shop one also included *Super Mario Land*, which Bob kept for "research purposes."

Actually, it was research. He needed to understand how these things worked.

He set up one of the Game Boys on his desk next to his 486 PC, a breadboard, an oscilloscope, and a soldering iron. The other one went in his backpack as a "backup." He'd already learned the first lesson of Game Boy development: always have a backup.

James and Mark (Bob's brother-in-law, the programmer) joined him in what they were now calling "The Lab"—actually just Bob's basement, but morale mattered.

"Show us what you've got," James said.

Bob had spent the previous week researching. The Game Boy homebrew scene was small but real. There were people out there writing their own games, reverse-engineering the hardware, sharing tools and knowledge. Most of it was scattered across BBSes and early internet forums, but Bob had found enough to start.

He showed them a printable file he'd compiled: "Pan Docs - The Game Boy Technical Reference." Someone with the handle "Pan" had documented the entire Game Boy hardware architecture. Every register, every memory address, every quirk of the video system.

"This is our bible," Bob said.

He'd also found assemblers. The most promising was ASxxxx, a cross-assembler that could target the Z80. With some modifications, it worked for the Game Boy's CPU, which was Z80-like but not quite Z80.

"Can we afford development kits?" Mark asked.

"No," Bob said. "Official Nintendo SDK costs thousands and requires licensing. We're doing this the hard way—public tools, homebrew methods, test on real hardware."

"Isn't that illegal?"

"Writing our own software? No. Nintendo can't stop us from programming their hardware. They can refuse to manufacture it officially, but we'll handle manufacturing ourselves. As long as we don't use Nintendo's trademarks or copyrighted code, we're fine."

"Are you sure?"

"No. But we can't afford lawyers anyway, so let's build it first and worry about legal later."

James picked up one of the Game Boys, turned it over. "How do we get our code onto this?"

Bob showed them the cartridge. "We need to build a programmer. Write our code to an EPROM chip, wire it into a cartridge. There are schematics online."

"You want us to build a cartridge from scratch?"

"For prototyping, yes. If this works, we'll find a manufacturer. But right now, we need to prove we can even write code that runs."

Mark cracked his knuckles. "Okay. What's the first goal?"

"Get 'Hello World' on the screen."

It sounded simple. It wasn't.

## Week Two: Hello Nothing

The Game Boy's video system was nothing like a PC.

On a PC, you had a framebuffer—write pixels to memory, they appeared on screen. Simple.

The Game Boy had tiles.

Everything on the screen was made of 8×8 pixel tiles. You defined the tiles (what they looked like), then you built a tilemap (which tiles went where). The video hardware composited the tilemap onto the screen sixty times per second.

This was genius for games—very memory-efficient, fast to update. For drawing arbitrary graphics, it was a nightmare.

"We need 256 unique tiles just for the stereonet," Bob said, staring at his calculations. "But the Game Boy only supports 256 tiles *total*. That leaves zero for text, UI, anything else."

"Can we generate tiles on the fly?" Mark asked.

"Maybe, but where do we store them? We have eight kilobytes of video RAM. The stereonet alone needs..." Bob did math "...3,136 bytes. That's almost half our VRAM budget. We still need space for the tilemap, text, UI elements."

James pointed to Bob's sketch. "What if we don't use 256 tiles? What if we reuse tiles?"

"The stereonet is a circle with a grid. Every position is unique."

"But is it? Look—these four tiles are just rotations of each other. These are mirrors. We could reduce duplicates."

Bob studied the sketch. James was right. With clever tile design, they could maybe get it down to 196 tiles. That left 60 for everything else.

"That's tight," Mark said.

"Everything about this is tight," Bob replied.

They spent three days just designing the tile set. Bob sketched by hand, Mark converted to pixel data, James reviewed for optimization. Every tile mattered.

On day four, they tried to compile their first code.

The assembler threw 47 errors.

## Week Three: The First Pixel

The errors were mostly Bob's fault. He'd written Z80 assembly before, but the Game Boy's CPU had subtle differences. Different opcodes, different timing, different everything.

Mark caught most of the mistakes. He'd been teaching himself from the Pan Docs, cross-referencing every instruction.

"This opcode doesn't exist on the Game Boy," he'd say.

Bob would check the docs. Mark was always right.

By day five of week two, they had code that compiled.

By day six, they had code that ran.

By day seven, they had code that crashed immediately.

"It's hitting an infinite loop somewhere," James said, watching the screen stay blank.

They didn't have a debugger. They didn't have an emulator. They had a Game Boy, an oscilloscope, and hope.

Bob's solution: write values to specific memory addresses and check them manually. Basically, debug by halting the code and examining memory with a cartridge reader.

It was slow. It was tedious. It worked.

The problem was in the video initialization. Bob had forgotten to wait for VBlank before writing to VRAM. The Game Boy's video hardware didn't like that. It would corrupt the writes or, in this case, lock up.

Mark fixed it: "We wait for VBlank, then write. Every time."

They recompiled. Reflashed the EPROM. Inserted the cartridge.

The Game Boy booted.

The Nintendo logo appeared (that was built into the hardware, not their code).

Then...

A single pixel appeared in the top-left corner of the screen.

Bob, James, and Mark stared at it.

One white pixel on a gray background.

"We did it," Mark whispered.

"We drew a pixel," James said.

"We drew a PIXEL!" Bob shouted.

They'd spent two weeks to draw one pixel. They had six weeks left to build a stereonet plotter.

But they'd drawn a pixel. That meant the toolchain worked. The hardware worked. They could write code that ran.

That pixel was progress.

## Week Four: Text and Despair

Getting text on screen should have been straightforward. It wasn't.

They needed a font. The Game Boy didn't come with one. They had to design their own.

More tile budget problems.

A full alphabet (A-Z), numbers (0-9), and basic punctuation took 40 tiles. They had 60 tiles available (after the stereonet used 196). That left 20 for UI elements, symbols, indicators.

"We can't do lowercase," Mark said. "Not enough tiles."

"Fine. UPPERCASE ONLY," Bob typed into his design doc.

They cut every non-essential character. No lowercase. No fancy punctuation. Just the bare minimum to display coordinates, measurements, and basic labels.

Lisa—the technical writer they'd hire later—would make this font actually readable. Right now, it was blocky and ugly, but functional.

With the font working, they could finally display debug information on screen. This made development much faster. No more manually checking memory addresses.

Bob wrote "GEOCALC V0.1" in the top-left corner.

It appeared.

James took a photo. "For the archives."

They had four weeks left.

## Week Five: The Math Crisis

Bob had calculated that they could do stereonet projection with fixed-point math and lookup tables.

Bob had been optimistic.

The problem was precision. Fixed-point math in 8.8 format (8 bits integer, 8 bits fraction) gave them 1/256 precision. For most things, that was fine. For stereonet projection at the edge of the circle, that wasn't nearly enough.

Errors accumulated. A point that should be at the stereonet's edge would be off by 3-4 pixels. That was 5-6 degrees of error. Unacceptable.

"We need more precision," Mark said.

"We don't have more precision," Bob said. "The CPU is 8-bit. We can do 16-bit math but it's slow."

"How slow?"

Bob did calculations. "Multiply is... maybe 20-30 cycles per operation in 16-bit. We need to do four multiplies and two adds per point. At four megahertz, that's... still fine, actually. Under a millisecond per point."

"Then what's the problem?"

"Memory. Lookup tables. If we use 16-bit fixed-point, our sine/cosine table is 720 bytes instead of 360. Our radius table doubles. We're tight on RAM as it is."

They spent two days trying to optimize. Could they compress the tables? Could they calculate on-the-fly? Could they cheat somehow?

Mark had the breakthrough.

"What if," he said slowly, "we use different precision for different parts? Low precision for fast calculations, high precision for final projection?"

It was hybrid fixed-point arithmetic. Crude but effective. They used 8.8 for intermediate calculations, 12.4 for final screen coordinates. The tables stayed small, but accuracy improved.

Bob tested it. Plotted test points around the stereonet.

Maximum error: 1 pixel. About 1.6 degrees.

"Close enough for geology," James declared.

Bob agreed. They had three weeks left.

## Week Six: The Breakthrough

At 2 AM on a Tuesday morning in week six, James got the first point to plot correctly on the stereonet.

He'd been working on the projection algorithm, translating Mark's mathematical tables into actual screen coordinates. It was tedious work—convert polar coordinates to Cartesian, apply fixed-point math, account for screen offset, clip to boundaries.

The test case was simple: plot a pole with trend 045°, plunge 32°.

The math said it should appear at screen position (x=76, y=51) relative to the stereonet center.

James ran the code.

A single pixel appeared on the stereonet.

He measured it manually, counting pixels from the center.

Position: (76, 51).

Exactly right.

He tried another point: trend 132°, plunge 67°.

The pixel appeared at (x=-31, y=-28) from center.

Correct.

Another: trend 278°, plunge 15°.

Correct.

He tested ten points. All correct.

"Holy shit," he said to the empty office. "It works. It actually works."

He called Bob. It was 2 AM. Bob answered on the first ring.

"Does it work?" Bob asked.

"It works."

"You're sure?"

"I've plotted ten points. All correct. The projection math works. We can do this."

Bob was quiet for a moment. Then: "We can do this."

"We can do this."

"I'll call Maggie in the morning. We're going to make our deadline."

James stared at the Game Boy screen. A stereonet with ten lonely pixels scattered across it.

It was the most beautiful thing he'd ever seen.

## Week Seven: Everything at Once

With two weeks until deadline, they had:
- Working projection math ✓
- Text rendering ✓
- Stereonet display ✓
- Single point plotting ✓

They did not have:
- Data entry UI
- Multiple point storage
- Great circle plotting
- Any way to save data
- Any way to delete data
- Any error handling
- Any robustness

"We're going to ship a demo," Bob decided. "Not a product. A demo that proves the concept."

They split the work:

**Bob:** Data storage and SRAM save/load
**Mark:** Data entry UI
**James:** Multiple point plotting and great circles

They worked 16-hour days. The office (Bob's basement) smelled like coffee, solder flux, and stress.

Bob got SRAM working after three days of fighting with the memory bank controller. The Game Boy's MBC1 chip was supposed to make bank switching easy. It didn't. Bob eventually figured out the magic sequence of register writes that made it behave.

He could save up to 50 measurements in SRAM. They'd stay saved even when power was off. Just like a field notebook, but digital.

Mark built a data entry screen. D-pad to adjust trend and plunge, A to confirm, B to cancel. It was clunky but functional. Testing it with gloves would come later.

James worked on plotting multiple points simultaneously. This was harder than it sounded. The naive approach—replot everything every frame—was too slow. 50 points × 4 milliseconds per point = 200 milliseconds. At 60 frames per second, that left only 16ms per frame. They'd drop frames.

His solution: only redraw when data changed. Track "dirty" state. Plot to a shadow buffer in RAM, then copy tiles to VRAM during VBlank.

It was the technique that would make everything else possible.

## Week Eight: The Demo

On the last day of week eight, they had a working prototype.

It could:
- Display a stereonet
- Enter orientation measurements (trend/plunge)
- Plot up to 50 points
- Save/load data from cartridge
- Show a data table
- Navigate with D-pad and buttons

It could not:
- Rotate data
- Calculate statistics
- Print anything
- Export to PC
- Recover from most errors
- Handle edge cases
- Look particularly good

It was buggy. It was incomplete. It was slow.

But it was provably real.

Bob called Maggie. "We're ready."

## The Demo

Maggie arrived at Bob's basement office at 2 PM. She brought her field notebook and Brunton compass—the tools she'd used for ten years.

"Show me," she said.

Bob powered on the Game Boy. The GEOCALC logo appeared (just text, nothing fancy).

"This is a stereonet," he said, pointing to the circular grid on the screen. "Equal-area projection, just like you plot by hand."

He pressed Start. The data entry screen appeared.

"You enter a measurement. D-pad adjusts the trend." He pressed up, and the number incremented. "A button confirms."

He entered a test measurement: 045°, 32°, pole.

A red pixel (well, a darker gray pixel—no color on the original Game Boy) appeared on the stereonet.

Maggie leaned closer. "Where is that?"

"Exactly where a 045-32 pole should be. Mark verified it against hand calculations."

Bob entered more measurements. Each appeared on the stereonet in real time.

Ten points.

Twenty.

Thirty.

A pattern emerged—a cluster of poles representing bedding measurements from an imaginary fold. Bob had prepared test data from a real field area.

"The mean vector is..." Maggie did quick mental math, looking at the pattern "...about 127 degrees, 45 plunge?"

"We don't calculate that yet," Bob admitted. "But yes, that's roughly right."

He pressed Select. The screen switched to a data table. All 30 measurements listed with their trend, plunge, and type.

"You can scroll through them," James said. "Edit them. Delete them."

Bob pressed Start again. A menu appeared: SAVE, LOAD, NEW.

He selected SAVE.

"Now the data is in the cartridge's battery-backed RAM. I can turn off the Game Boy, come back tomorrow, and it's still there. Like a field notebook."

He powered off the Game Boy.

Powered it back on.

LOAD.

All 30 measurements reappeared. The stereonet redrew itself.

Maggie was quiet.

Bob, James, and Mark waited.

Finally, Maggie picked up the Game Boy. Turned it over in her hands. Pressed buttons, navigated the menus. Added a few more measurements. Watched them plot in real time.

She set it down.

"This is real," she said.

"This is real," Bob confirmed.

"You actually did it."

"We actually did it."

"In eight weeks."

"In eight weeks."

Maggie looked at the three of them—exhausted, unshowered, running on coffee and adrenaline.

"I need to test it in the field," she said. "Real outcrop. Real measurements. Real conditions."

"When?"

"This weekend. There's an outcrop near Morrison I know well. I've plotted it by hand a dozen times. If your device matches my field data, we're in business."

Bob nodded. "We'll fix the obvious bugs. Make it more stable. But the core works."

"The core works," Maggie agreed.

She pulled out her field notebook, flipped to a page of calculations. The company budget. Their runway.

"We've got three months of funding left. Maybe four if we're lean. How fast can you build a shippable version?"

Bob looked at James and Mark. James shrugged. Mark did quick mental math.

"Nine months," Bob said. "December. We can have a real product by then."

"Not a full year?"

"We'll cut features. Focus on core functionality. Ship something that works, update it later."

Maggie made a note. Then she looked up. "Okay. Here's the deal. You've got until December. You build this. I'll handle fundraising, marketing, and keeping us alive long enough to ship."

"Deal."

"And Bob?" She smiled—the first real smile Bob had seen from her in months. "I'm sorry I doubted you."

"You should have doubted me. This was insane."

"It was insane," she agreed. "But it worked."

They shook hands.

Eight weeks ago, Bob had pitched a crazy idea about building geology tools on video game hardware.

Now they had a prototype.

In nine more months, they'd have a product.

In three years, they'd have changed structural geology.

But first, they had to survive the next nine months.

That was going to be the hard part.
