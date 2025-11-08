# The Revelation

## December 26, 1991

Bob Kuwahara's nephew Danny was nine years old and terrible at *Tetris*.

Bob watched from the kitchen doorway as Danny mashed buttons on his Game Boy, trying to rotate falling blocks while his stack crept toward the top of the screen. The kid's tongue stuck out in concentration—a universal sign of maximum cognitive load.

"Uncle Bob, I'm gonna beat your high score!" Danny announced, just before his stack topped out. Game over. The blocky Game Boy speaker played its sad little tune.

"Sure you are," Bob said, smiling despite himself. Despite everything.

It was the day after Christmas, 1991. Bob's sister's family had driven up from Denver to Golden for the holiday. Julie, Bob's wife, was in the living room with his sister, showing off photos from their summer camping trip. Bob had escaped to make coffee, grateful for a moment of quiet.

He'd been thinking about work. He'd been thinking about work constantly for three months.

Four months of runway left. Maybe less. The FieldLogger II returns were accelerating. Every day brought another customer complaint: compass drift, battery drain, random lockups. Every return cost them $280 in lost manufacturing plus the labor to diagnose and maybe fix it. They couldn't fix most of them. The sensor was just unreliable.

Maggie wanted to pivot again. James thought they could fix it with better calibration software. Bob thought they were finished.

"I'm gonna try again!" Danny announced, hitting the start button.

Bob poured his coffee, watching the kid play. The Game Boy was a hand-me-down from Danny's older cousin—scratched, scuffed, the screen slightly yellowed. But it worked. Danny had been playing on it for two days straight. The original four AA batteries were still going.

"How long have you had that?" Bob asked.

"This Game Boy? Forever. Like, since I was seven. Jake gave it to me when he got his Game Gear."

Two years. A nine-year-old had been using this thing for two years. How many drops? How many thrown into backpacks, stuffed under couch cushions, forgotten in the back seat of cars?

"Does it ever break?"

Danny shrugged without looking up from the screen. "Nah. It's a Game Boy. They don't break."

Bob sipped his coffee. Watched the kid play.

The FieldLogger II broke if you looked at it wrong. They'd had one fail in shipping. Another one stopped working after a week in moderate humidity. The compass sensor—a tiny, delicate thing—would drift if the temperature changed too much, if there was magnetic interference, if Mercury was in retrograde for all Bob knew.

Meanwhile, this $89 toy was indestructible.

"Hey Danny," Bob said. "Can I see that for a second?"

"After this level."

Bob waited. Danny lost. He always lost. He handed over the Game Boy without complaint.

Bob turned it over in his hands. Chunky gray plastic, the Nintendo logo worn almost smooth. The rubber buttons still had good travel. The screen was scratched but perfectly readable. The whole thing felt solid—well-designed, well-built, engineered for the worst-case scenario.

Which, for a nine-year-old, was pretty bad.

"How often do you drop it?"

"I dunno. A lot?"

As if on cue, the Game Boy slipped from Bob's hands.

It happened in slow motion, the way these things do. Bob's coffee-slicked fingers lost their grip. The Game Boy tumbled end-over-end. Bob lunged for it, missed. It hit the tile floor of the kitchen with a sharp plastic crack.

*Oh no.*

Danny looked up. "Uncle Bob!"

Bob picked it up, heart racing. He'd just broken his nephew's Game Boy. Right after Christmas. This was the worst uncle failure possible.

He turned it over, inspecting for damage.

Nothing. Not a scratch. Not a crack. Not even a scuff on the already-scuffed corner that had hit the floor.

He pressed the power button. The Nintendo logo appeared. The *Tetris* theme song played. The game loaded exactly where Danny had left it.

"See?" Danny said. "They don't break."

Bob stared at the device in his hands.

His brain, the part that had been stuck in a loop of worry and stress and impending failure for three months, suddenly engaged in a different direction.

*This thing survives a drop onto tile.*

*Our FieldLogger II's compass dies if you breathe on it too hard.*

*This costs $89 retail. We charge $899 for something that works worse.*

*This runs for 30 hours on four AA batteries. Our device gets eight hours on the same batteries, with lower brightness.*

*This has been working for two years in the hands of a nine-year-old. Our devices fail after months in the careful hands of professional geologists.*

*What are we doing?*

"Uncle Bob?" Danny was looking at him funny. "Can I have it back?"

Bob handed it over, his mind somewhere else entirely.

## That Night

Bob couldn't sleep.

He kept thinking about the Game Boy. Not the game. The hardware.

At 2 AM, he gave up on sleep and went downstairs to his home office. Julie mumbled something about coffee being already made. She'd learned to sleep through his insomnia years ago.

Bob's "office" was a converted basement corner with a desk, a 486 computer, and shelves full of technical manuals. He pulled out his engineering notebook—a spiral-bound grid paper notebook he'd been using since his Schlumberger days.

He opened to a new page and wrote at the top: "The Game Boy Question."

Then he started listing facts. Not speculation. Facts.

**Game Boy (DMG-01):**
- $89 retail (probably $30-40 manufacturing cost)
- Survives 6+ foot drops onto concrete
- 30 hour battery life (4× AA)
- Screen quality: Good (high contrast, visible in sunlight)
- Availability: Everywhere (millions sold)
- Development cost: $0 (we just write software)
- Distribution: Cartridge (plug and play, no PC required)
- Reliability: Proven in field (meaning: in hands of children)

**FieldLogger II:**
- $899 retail ($280 manufacturing cost)
- Survives... 6 inches? Maybe?
- 8 hour battery life (4× AA)
- Screen quality: Mediocre (LCD with poor contrast)
- Availability: Custom order (we build them)
- Development cost: $75K (and ongoing failures)
- Distribution: Cable to PC (software installation required)
- Reliability: Terrible (15% return rate)

Bob stared at the comparison.

This was insane. This was absolutely insane.

But it was also obvious.

He flipped to a new page and wrote: "What if we build geological instruments on Game Boy hardware?"

Then he started sketching.

The core problem in structural geology was plotting orientation data on stereonets. Geologists measured the orientation of rock layers, faults, fold axes—anything that had a direction in 3D space. They recorded these measurements in field notebooks, then plotted them by hand on stereonet paper when they got back to the office.

It was tedious. It was error-prone. It was slow.

Some companies made software for stereonet plotting. You'd type in your measurements on a computer, and it would draw the stereonet. Better than hand-plotting, but still required getting back to the office.

What if you could plot in real time? In the field? As you collected measurements?

What if you had a device that:
- Fit in your pocket
- Ran for days on standard batteries
- Survived being dropped, rained on, stuffed in a backpack
- Let you plot stereonets at the outcrop
- Didn't require a PC

What if that device was a Game Boy?

Bob started doing math.

The Game Boy screen was 160×144 pixels. Could you fit a usable stereonet in that? He sketched a circle, 112 pixels in diameter. That would leave room for a UI around the edges. 112 pixels for a stereonet meant about 1.6 degrees per pixel at the edge. That was... actually good enough. Geologists worked in 5-degree increments typically. This would be better than hand-plotting.

Could you do the math? Stereonet projection was complex—equal-area projection, great circle calculations, rotation matrices. That required floating-point math. The Game Boy's CPU had no floating-point hardware.

But... fixed-point math. He'd done fixed-point on embedded systems before. Pre-computed lookup tables for sine and cosine. It would be tight, but doable.

Could you store data? The Game Boy had cartridge SRAM. Battery-backed. You could store hundreds of measurements, save them between power cycles. Like a field notebook, but digital.

Bob kept sketching, kept calculating. The ideas came faster than he could write them down.

By 3 AM, he had filled ten pages.

By 4 AM, he had a rough system architecture.

By 5 AM, he had convinced himself this could work.

By 6 AM, Julie found him asleep at his desk, his head on the notebook, pencil still in hand.

## The Napkin Pitch

Bob waited until January 2nd to pitch the idea.

New year, new company, new ideas. Or something like that. Really, he'd spent a week building a case because he knew Maggie would think he'd lost his mind.

They met at the climbing gym cafe—their unofficial office when the garage was too cold. It was 10 AM on a Thursday. James was late, as always. Maggie had coffee and a skeptical expression.

"You sounded weird on the phone," she said.

"I have an idea."

"We don't have money for new ideas."

"This one doesn't cost money. That's the whole point."

James arrived, ordered coffee, sat down. "What'd I miss?"

"Bob has an idea," Maggie said in a tone that suggested this was not necessarily good news.

Bob pulled out a napkin. He'd considered bringing his notebook with all the detailed architecture diagrams, but he'd learned over three years with Maggie that she responded better to simple pitches. Details came later.

He drew a table:

```
                    GAME BOY    OURS (FieldLogger II)
Cost:               $89         $899
Drop test:          6ft+        6 inches
Battery life:       30 hrs      8 hrs
Screen quality:     Good        Mediocre
Availability:       Everywhere  Custom order
Dev cost:           $0          $75K
Software dist:      Cartridge   Cable/PC
Reliability:        Proven      15% return rate
```

He slid the napkin across the table.

Maggie read it. James read over her shoulder.

"What's your point?" Maggie asked.

"My point is: why are we building custom hardware when better hardware already exists?"

"You want to buy Game Boys?"

"I want to write Game Boy software. Stereonet plotting. Field data collection. Everything we've been trying to build, but on hardware that actually works."

Maggie put down her coffee. "Bob. Game Boys are toys."

"Game Boys are field-proven portable computers with better specs than anything we can build."

"They're for children."

"So was the Brunton compass when David Brunton put a compass in a carpenter's level in 1894. Everyone laughed at him. Now every geology student has one. It became the standard because it *worked*, not because it looked fancy."

That got her attention. Maggie loved her Brunton compass. She had a collection of vintage ones.

James was leaning forward. "Walk me through the technical side. Can you actually do stereonet math on a Game Boy?"

Bob flipped the napkin over and started sketching. "160 by 144 screen. We can fit a 112-pixel stereonet with room for UI. Equal-area projection needs floating-point math, but we fake it with fixed-point and lookup tables. I've done the calculations—it's tight but doable. The Z80-like CPU is slow, but fast enough. We plot points, great circles, do rotation. Store measurements in battery-backed SRAM."

"What about data export?"

"Phase one: manual transcription. Phase two: we build a link cable adapter to PC. There's a serial port on the Game Boy. We'd need a level shifter, but that's just a MAX232 chip and some passive components. Twenty bucks in parts."

James was nodding. "This is... actually not crazy."

"It's completely crazy," Maggie said. But she was reading the napkin again.

Bob pressed his advantage. "Look, we have four months of runway. Maybe less. Traditional product development would take eighteen months minimum. A Game Boy cartridge? We could have a working prototype in six weeks. A shippable product in six months. We'd need brutal focus, but it's possible."

"We don't know Game Boy programming."

"We don't know anything until we learn it. James and I can figure it out. The tools are available—assemblers, some documentation. There's a homebrew community. It's doable."

Maggie was quiet for a long moment. James watched her, waiting.

Finally, she looked at Bob. "You really think this can work?"

"I think our current plan definitely doesn't work. FieldLogger II is dead. We don't have resources for FieldLogger III. We're four months from bankruptcy. This is the only idea I've got."

"It's using a video game console."

"It's using the most reliable portable computer we can't afford to build ourselves."

"Geologists will laugh at us."

"Geologists laugh at us now. At least this way we'll have something that works."

Maggie looked at James. "What do you think?"

James tapped the napkin. "I think Bob's right about the timeline. We don't have eighteen months. We maybe have six. And I think he's right about the hardware—the Game Boy is better than anything we've built. But—" he looked at Bob "—can we actually make it work? Can you and I and Mark learn Z80 assembly and build a stereonet plotter in six weeks?"

"I don't know," Bob said honestly. "But I know we can't build FieldLogger III. So we either try this or we shut down."

Silence.

Maggie picked up the napkin, studied it one more time.

"If we do this," she said slowly, "we do it all-in. No half measures. No hedging. We bet the company on this."

"We're betting the company anyway," Bob said. "We're just picking what to bet on."

Maggie looked at James. James shrugged. "I'm in if you are."

She looked at Bob for a long moment.

Then she pulled out a pen and wrote on the napkin: "GEOCALC - Game Boy Stereonet Plotter."

"Prove it can work," she said. "You've got eight weeks. If you can show me a working prototype that actually plots stereonets, I'll bet the company on it."

Bob took the napkin. His hand was shaking slightly.

"Eight weeks," he said.

"Eight weeks. After that, we either have a product or we have severance packages."

James raised his coffee cup. "To terrible ideas that might work."

Bob and Maggie clinked their cups against his.

"To Game Boys," Bob said.

They drank.

In eight weeks, they would either have the future of their company, or they would have nothing.

Bob folded the napkin carefully and put it in his pocket.

He still has it, twenty-three years later. Framed in his home office. The original pitch. The moment everything changed.

All because a nine-year-old dropped a Game Boy down the stairs, and it kept working.
