# The Passion Project

## March 2011: Kevin Can't Let It Go

The Nintendo 3DS launched in March 2011. Nintendo's newest handheld. The feature that caught everyone's attention: stereoscopic 3D display. No glasses needed.

Kevin saw the announcement and immediately thought: *aerial photo interpretation in TRUE 3D*.

For context: Structural geologists traditionally used stereoscopes to view aerial photo pairs in 3D. Two overlapping photos, viewed through lenses, create depth perception. You can see topography, identify faults, trace geological features.

It's a classic technique. Taught in every structural geology course. But it requires:
- Physical photo prints
- A stereoscope (expensive, bulky)
- Good lighting
- Desk space

Kevin's idea: Replace the stereoscope with a 3DS.

Load stereo photo pairs digitally. View with true 3D depth perception (thanks to the 3DS's autostereoscopic screen). Annotate features on the touch screen. Export to GIS software.

All the benefits of stereoscopic viewing, but digital, portable, with modern workflow integration.

It was a solution looking for a problem that didn't really exist anymore (digital photogrammetry had mostly replaced stereoscopes). But Kevin didn't care.

He wanted to build it.

## The Pitch

May 2011. Kevin walked into Maggie's office (now at Trimble HQ).

"I want to build a 3DS app."

Maggie looked up from her email. "The whole industry is on iPad and Android tablets. Who would buy a 3DS geology app?"

"Maybe 200 people. I know it doesn't make business sense. But it would be SO COOL."

"How much time?"

"Twenty percent time. My own project. I'll do it nights and weekends if I have to."

Maggie considered. Kevin had been with the company since 1992. Never asked for anything unreasonable. Had shipped every version of GeoCalc on time.

"What do you need?"

"DevkitPRO license. 3DS hardware. Maybe $15K budget for tools and marketing. Permission to call it a Trimble research project."

"If I approve this, it's a hobby project. No expectations of profit. No promises of support. You build it, we'll see if anyone wants it."

"Deal."

"And Kevin? You're the only person in this company who would think of this."

"I know."

Trimble approved it. Research project. Kevin's 20% time. Demo at AGU 2012. If there was interest, limited release. No expectations.

Kevin started development in June 2011.

## Technical Challenges

The 3DS's headline feature was the stereoscopic 3D display. Autostereoscopic (no glasses required). Adjustable depth via slider.

Perfect for viewing stereo aerial photos.

**But:**

Loading high-resolution aerial photos on a handheld with limited RAM was tricky. Typical stereo pair: 5000×5000 pixels, two images. Far too large for the 3DS's 64 MB of application RAM.

Kevin's solution: Tiled loading. Load only the visible region, stream from SD card. Level-of-detail pyramid. Same techniques used in modern map apps, adapted for 3DS.

**Annotation:**
Bottom screen: Touch interface for tool selection and attributes
Top screen: Stereo image display
Circle Pad: Pan image
L/R buttons: Zoom

Users could trace faults, mark features, add notes. Everything georeferenced.

**Export:**
- Shapefile (for ArcGIS/QGIS)
- KML (for Google Earth)
- CSV (for spreadsheets)
- Direct integration with GeoCalc smartphone app

The hardest part: Stereo alignment. Photo pairs needed precise alignment for comfortable 3D viewing. Kevin built auto-alignment tools, but users could manually adjust if needed.

## Development Montage

Kevin worked on AeroGeo 3D for 18 months.

Evenings. Weekends. Lunch breaks. Any spare time.

His wife Rachel (also a programmer) helped with the UI design. "Make it less ugly," she said, reviewing his early mockups.

He did.

His kids (ages 6 and 4) occasionally tested it. "Daddy, why are you making a game about rocks?"

"It's not a game, it's a geology tool."

"It looks boring."

"It's for grown-ups."

"Grown-ups are weird."

Fair point.

By November 2012, he had a working prototype. It could:
- Load stereo aerial photo pairs
- Display in 3D (adjustable depth)
- Pan and zoom smoothly
- Annotate with polylines, points, polygons
- Export to standard GIS formats
- Save projects to SD card

It was niche. It was weird. It was everything Kevin wanted it to be.

## AGU 2012 Demo

Kevin set up a demo station at the Trimble booth. One 3DS, loaded with example stereo pairs (classic fold structures, fault zones).

Conference attendees would walk by, curious.

"Is that a 3DS?"

"Yes."

"For geology?"

"Aerial photo interpretation. Stereo viewing, like a traditional stereoscope, but digital."

"Can I try it?"

They'd try it. View the 3D aerial photos. Use the Circle Pad to pan around. Adjust the 3D slider for comfortable depth.

Reactions varied:

**Older geologists (60+):** "This is what we used to do with stereo scopes! But digital. That's clever."

**Middle-aged geologists (40-60):** "We don't use stereo photos much anymore. Digital photogrammetry is easier. But this is a nice nostalgia piece."

**Younger geologists (<40):** "What's a stereoscope?"

Kevin demonstrated the workflow. Load photos, view in 3D, trace a fault, export to GIS.

"How much?"

"$129. Includes software, example datasets, manual."

"Who would buy this?"

"Professors who teach photo geology. Retired geologists who grew up with stereoscopes. People like me who think it's cool."

About 30 people left business cards. "Contact me when it ships."

Not a huge market. But a market.

## The Limited Run

December 2012. **AeroGeo 3D** released.

Manufacturing: 100 units. Kevin's conservative estimate.

Price: $129 (software + SD card with examples).

Target market:
- University professors teaching photo geology (15 potential buyers)
- Retired geologists with nostalgia for stereoscopes (20 potential buyers)
- Nintendo + geology enthusiasts (50 potential buyers)
- Curious professionals (15 potential buyers)

Marketing: Blog post, email to GeoCalc user list, booth at GSA 2013.

**Result:**

Sold out in 3 months.

Not a runaway success. Just 100 people who thought: "That's weird enough to be worth $129."

**Reviews:**

Geologists Weekly (yes, it exists): "Like a View-Master for geology nerds. Surprisingly functional. Mostly a curiosity."

GeoStructure blog comments: "This is the most 'Kevin' thing I've ever seen. I love it."

Email from Dr. Susan Martinez (Oregon State): "I use this in my photo geology class. Students think it's weird. Then they use it and go 'ohhh, I get it now.' Effective teaching tool. Thanks for building it."

**Revenue:** $12,900
**Cost:** $15,000 (development, manufacturing, marketing)
**Profit:** -$2,100

Financial failure.

Kevin didn't care.

## Never Updated, Never Restocked

Kevin never released version 2.0.

He could have. Users requested features:
- Automated feature detection
- DEM generation from parallax
- Wi-Fi sync

He didn't implement them.

Why?

"I built what I wanted to build. It works. It does the thing I imagined. I'm satisfied."

Some projects are complete when they ship, not when they're perfect.

AeroGeo 3D was complete.

## The Epilogue to the Epilogue

The stereoscopic visualization concepts Kevin developed for AeroGeo 3D got incorporated into Trimble's VR geology applications in 2016.

Virtual reality headsets (Oculus Rift, HTC Vive) for geological visualization. View 3D models of terrain, geological structures, seismic data.

Kevin consulted on the project. Shared his code. Helped design the interface.

The VR apps were commercially successful. Used by universities and petroleum companies.

None of it would have existed without Kevin's "useless" passion project on Nintendo 3DS.

Research has a way of paying off in unexpected ways.

## Kevin's Reflection (Blog Post, 2013)

> I knew AeroGeo 3D wouldn't be a commercial hit. I knew the market had moved on. I knew it was nostalgia and indulgence.
>
> But I'd wanted to make a 3DS geology app from the moment the console was announced. Twenty years of my career started with a Game Boy. I had to see it through one last time.
>
> We sold 100 units. Made $12,900 in revenue. Spent $15,000 making it. Lost $2,100.
>
> Worth every penny. Zero regrets.
>
> Sometimes you build things because they matter to you, not because they make business sense. GeoCalc Pocket started that way too—a desperate pivot born from Bob seeing his nephew drop a Game Boy.
>
> If we'd only built things that made business sense, we'd never have put geology software on a Game Boy in the first place.
>
> So yes, I built a niche geology app for a handheld game console that most people have never heard of. I sold 100 copies. It lost money.
>
> And I'm glad I did it.
>
> Because twenty years ago, Bob bet the company on a crazy idea. That idea changed how structural geology is taught. Helped thousands of geologists. Saved at least one PhD dissertation.
>
> Maybe somewhere, one of those 100 AeroGeo 3D users will have a moment where it matters. Where that weird 3DS app helps them see something they wouldn't have seen otherwise.
>
> Probably not. But maybe.
>
> And that possibility is worth $2,100.

## The Last Nintendo Project

AeroGeo 3D was the last GeoStructure/Trimble project on Nintendo hardware.

Timeline:
- 1994: GeoCalc Pocket (Game Boy)
- 1996: GeoCalc DX (Game Boy)
- 1998: GeoCalc Color (Game Boy Color)
- 2001: GeoCalc Advance (Game Boy Advance)
- 2003: GeoCalc Advance SP (Game Boy Advance SP)
- 2012: AeroGeo 3D (Nintendo 3DS)

Eighteen years. Five platforms. 74,680 units (including AeroGeo).

From bankruptcy-avoiding pivot to passion project.

From desperate innovation to nostalgic indulgence.

From "we have to do this to survive" to "I want to do this because it's cool."

The arc of a company's relationship with a platform.

It started with necessity.

It ended with love.

## What It Meant

Maggie's comment, seeing Kevin demo AeroGeo 3D at AGU:

"This is why we hired creative people and let them chase ideas. Twenty years ago, Bob chased a crazy idea about Game Boys. It saved the company. Ten years ago, Sarah insisted on field testing everything. It made our products reliable. Now Kevin chases a crazy idea about 3DS. It won't save the company—we're fine—but it keeps us weird. Weird matters."

James agreed: "We're the company that put geology on Game Boys. We could have become boring after the acquisition. Just another Trimble division. But we kept being weird. That's our identity."

Bob's take: "Every company needs some skunkworks projects. Things built for joy, not profit. They keep engineers happy. They keep the culture alive. And occasionally, they lead to unexpected breakthroughs. Kevin's 3D work fed into VR geology, which is now a real product line. You never know."

Sarah's perspective: "I tested AeroGeo 3D in the field. It works. It's niche, but it works. That's our legacy—building things that work, even if only 100 people care. Those 100 people matter."

Lisa: "Kevin's blog post about AeroGeo got 10,000 reads. More people read about it than bought it. The story matters as much as the product. We've always been a story-driven company. This is a good story."

## The Collector's Item

Original AeroGeo 3D units are now collectibles.

eBay listings (2020): $300-500 for a sealed unit.

Why? Rarity (only 100 made), uniqueness (geological software for 3DS), and the GeoCalc connection.

Kevin finds this hilarious.

"I lost money making them. Now people pay triple for sealed units they'll never open."

He kept one for himself. Uses it occasionally. Loads aerial photos from field areas he's visited. Views them in 3D. Remembers why he built it.

Because it was cool.

Because he could.

Because after twenty years in the industry, he'd earned the right to build something impractical and beautiful.

That's enough.

## Legacy of the Passion Project

AeroGeo 3D taught Trimble something important:

Let engineers build weird things. Passion projects. 20% time. Skunkworks.

Not everything needs an ROI. Not everything needs market validation.

Sometimes the value is:
- Learning new skills
- Keeping people engaged
- Maintaining creative culture
- Occasionally stumbling into breakthroughs

Kevin's 3DS project led to VR geology, which became profitable. But even if it hadn't, it was worth it.

Because companies that only build profitable things become boring. And boring companies lose their best people.

GeoStructure never became boring. Even after acquisition, even after smartphones, even after the original founders semi-retired.

They stayed weird.

And weird matters.

## The Full Circle

Twenty years earlier: Bob's nephew drops a Game Boy. Bob has a crazy idea. Bob pitches the idea on a napkin. Maggie says yes. The company bets everything on it. It works.

Twenty years later: Kevin has a crazy idea. Kevin builds it on 20% time. It loses money. Nobody cares. Except Kevin. And the 100 people who bought it. And the VR team who learned from it.

The cycle of innovation:
- Crazy idea
- Risky bet
- Build it anyway
- Learn something
- Iterate

The first time (GeoCalc Pocket), it saved the company.

The last time (AeroGeo 3D), it saved nothing and everything.

Saved Kevin's passion. Saved the culture. Saved the story.

Not every innovation needs to be revolutionary. Some just need to be true.

AeroGeo 3D was true.

And that's enough.
