# The Garage Years

## March 1987, Golden, Colorado

Maggie Chen's garage smelled like solder flux and disappointment.

The disappointment was new. The solder flux had been there since January, when she and her two partners had converted her two-car garage into what they generously called "headquarters." The Toyota 4Runner now lived in the driveway, accumulating snow and skeptical looks from neighbors.

Inside, three desks salvaged from a university surplus sale formed a U-shape around the garage's center. Bob Kuwahara hunched over the left desk, surrounded by oscilloscopes and breadboards. James Okoye occupied the right, a half-assembled prototype scattered across his workspace. Maggie stood in the middle, holding a clipboard and trying to figure out how to tell her partners they were running out of money.

Again.

"How many pre-orders?" Bob asked, not looking up from his soldering iron.

"Seventeen."

"FieldLogger II?"

"FieldLogger II."

Bob set down the iron. "We need at least forty to break even on the production run."

"I'm aware." Maggie glanced at her clipboard, though she had the numbers memorized. Forty units at $899 each. Manufacturing cost: $280 per unit. That left $619 per unit for overhead, salaries (minimal), loan payments (maximal), and the perpetual hope that something would eventually work.

"How many returns from the first batch?" James asked.

"Six."

"Out of?"

"Forty-seven."

James whistled. "Fifteen percent. That's—"

"Catastrophic," Maggie finished. "I know."

The returns told the story. FieldLogger II featured a digital compass module—expensive, finicky, and apparently allergic to the temperature swings of actual field work. After three to five days of use, the sensor would drift. Not much. Just enough to make measurements unreliable. Just enough to destroy trust.

When field geologists can't trust their tools, they don't buy them.

"Recalibration firmware?" Bob suggested.

"Already tried. Remember v1.3?"

Bob winced. Version 1.3 had required users to perform a seven-step calibration procedure involving rotating the device in three axes while avoiding magnetic interference. The manual was eleven pages. Nobody used it.

"We could offer free replacements," James said.

"With what money?" Maggie set down her clipboard. "We have four months of runway left. Maybe five if I stop paying myself."

The garage fell silent except for the hum of the space heater in the corner. Outside, March snow was falling on Golden, Colorado. Inside, three engineers contemplated the collapse of their dream.

\* \* \*

## Eighteen Months Earlier: The Beginning

They'd met at the Colorado School of Mines.

Maggie had been finishing her PhD in structural geology, spending summers mapping fault systems in the Rockies and winters writing code to analyze her field data. Bob was doing a postdoc in geophysics, building custom instrumentation for seismic surveys. James was visiting from MIT—a hardware engineer consulting on a sensor project.

The three of them ended up at the same conference poster session in September 1985, all presenting variations on the same problem: commercial field instruments were either too expensive, too fragile, or too limited for real scientific work.

"Why isn't anyone building better tools?" Maggie had asked, frustrated after watching a $2,000 data logger fail in her field area for the third time that season.

"Because it's a tiny market," Bob said. "Universities, geological surveys, consulting firms. Maybe 5,000 potential customers worldwide?"

"But they'd pay," James countered. "Good tools are worth money."

"So why don't we build them?" Maggie asked.

The question hung in the air for three seconds. Then all three of them started laughing at the absurdity of it.

But the idea wouldn't go away.

Over beers at a Golden brewpub that night, they sketched out a business on bar napkins. Small, focused. Custom electronics for field geologists. High quality, reasonable prices, designed by people who actually did fieldwork.

They would call it GeoStat Instruments.

By December, Maggie had submitted her dissertation and cleared out the garage. Bob had quit his postdoc. James had turned down an offer from Hewlett-Packard. They'd pooled their savings ($43,000), taken out a small business loan ($25,000), and ordered their first batch of components.

In January 1987, they shipped their first product.

## FieldLogger I: The First Mistake

FieldLogger I was supposed to be simple: a digital replacement for the field notebooks that geologists had been using for a century. Text entry, coordinate logging, timestamp functionality. Store a few hundred observations, download to PC later.

They built it around a Zilog Z80 processor, 32KB of RAM, and a 4-line LCD display. It had a membrane keyboard, sealed weatherproof case, and ran on AA batteries. The whole package fit in one hand.

Maggie designed the user interface. Bob wrote the firmware. James engineered the hardware. Three months from first sketch to shipping product.

It worked.

It actually worked.

They sold seventeen units in the first month, mostly to Maggie's former colleagues in the geology department. The feedback was universally positive. "Finally, something that makes sense." "Easier than paper notebooks." "The battery life is incredible."

Then someone asked: "Can it measure strike and dip?"

Strike and dip—the fundamental measurement in structural geology. The orientation of a rock layer in 3D space, described by two angles. Every field geologist measures hundreds of these per project.

FieldLogger I could record those numbers, but it couldn't measure them. You still needed a Brunton compass for that, then manually typed the values into the logger.

"Can you add a digital compass?" the customer asked.

"Of course," Maggie said.

That was the first mistake.

## StrikeCalc: The Sensor Disaster

Adding a digital compass turned out to be harder than they thought.

A Brunton compass is an elegant piece of 19th-century technology: a magnetized needle, a bubble level, a clinometer, all encased in bronze and glass. Geologists trust them because they're simple, reliable, and maintainable. They don't drift. They don't need batteries. They work at -20°F and 110°F.

Digital compasses in 1988 were none of these things.

The magnetoresistive sensors they sourced from Honeywell were sensitive, expensive, and temperamental. They needed careful calibration. They were affected by local magnetic fields (like metal tools in a geology field bag). They drew significant power.

But they could work, in theory.

Bob spent four months integrating the compass module into what they now called "StrikeCalc." James designed a three-axis gimbal mount to keep the sensor level. Maggie wrote the calibration software and tested it in the field.

They shipped twelve units in October 1988.

Eleven came back.

The sensors would drift after a few days of field use. Temperature cycling between day and night caused slight changes in the magnetic calibration. Without perfect leveling, the accuracy degraded. The battery life was seven hours instead of the advertised twenty.

"This is unusable," one customer wrote. "I'll stick with my Brunton, thanks."

They refunded everyone.

The company bank account now held $8,200.

## The Denny's Meeting

November 1988. A Denny's restaurant off I-70, halfway between Golden and Denver.

"We can't keep calling ourselves GeoStat," Maggie said, picking at a Grand Slam breakfast she wasn't really eating. "People think we do statistics. Or that we're a GPS company."

"GeoStructure?" Bob suggested.

"Better. GeoStructure... what?"

"Systems," James said. "GeoStructure Systems. Sounds more professional than 'Instruments.'"

Maggie wrote it on a napkin: **GeoStructure Systems, Inc.**

"Assuming we're still in business next year," she added quietly.

The waitress refilled their coffee. Outside, traffic hummed past on the highway. Inside, three engineers tried to figure out how to save their company.

"What if we fix StrikeCalc?" James asked.

"With what money?" Bob said. "We'd need to redesign the sensor mount, rewrite the calibration code, maybe switch to a different compass module. That's six months and $15,000."

"We don't have six months," Maggie said.

"Then what?" James asked. "We can't keep building products that don't work."

Maggie stared at her coffee. "I don't know."

That was the lowest point. November 1988. Bank account: $8,200. Product returns: 92%. Market reputation: damaged. Path forward: unclear.

They filed the name change paperwork that week anyway. GeoStructure Systems, Inc. Optimism disguised as bureaucracy.

## FieldLogger II: The Last Chance

By spring 1989, they'd pulled together enough consulting contracts to stabilize the finances. Bob did some seismic analysis for an oil exploration company. Maggie consulted on a mining survey. James took on hardware debugging work for a local tech startup.

It wasn't glamorous, but it paid the bills.

And it gave them time to try one more time.

FieldLogger II would combine everything they'd learned. The data logging interface from FieldLogger I (which actually worked). A better compass module from a different vendor. Improved weatherproofing. Longer battery life.

They would do it right this time.

Bob sourced a new digital compass from KVH Industries—more expensive, but with better temperature stability. James designed a custom shock-mounted sensor housing. Maggie created a simplified calibration procedure: just three steps, one page of instructions.

They built fifty units. Pre-orders were encouraging. The user manual was professional. The packaging looked legitimate.

Version 1.0 shipped in November 1990.

The first returns came in January 1991.

Same problem: sensor drift. Different vendor, same fundamental issue. Digital compass modules in 1991 just weren't reliable enough for field geology.

By March 1991, they'd sold 47 units and processed 6 returns. 12.8% return rate. Not as catastrophic as StrikeCalc, but unacceptable for a company that couldn't afford another product failure.

Pre-orders for the second batch: 17 units. Not enough to break even.

## Four Months

March 1991. The garage.

Maggie did the math on her clipboard, though she already knew the answer.

Bank balance: $14,300
Monthly burn rate: $3,200 (minimal salaries, parts, loan payment)
Runway: 4.4 months

They could stretch it to five if they all stopped paying themselves. Six if they cancelled the office phone line and started using the residential number. But that was just delaying the inevitable.

"We need a different product," Maggie said.

"We've tried three different products," James countered. "The pattern is consistent: build something, ship it, it doesn't quite work, customers return it."

"So we build something that does work."

"Like what?"

Nobody had an answer.

Bob went back to soldering. James returned to his prototype assembly. Maggie stared at her clipboard, looking for a solution in the numbers that wasn't there.

The space heater hummed. Outside, March snow fell on Golden.

Four months until failure.

\* \* \*

## The Nature of the Problem

The fundamental issue wasn't incompetence. All three of them were competent engineers. Maggie had published six papers on structural analysis. Bob's seismic instrumentation had been used in research worldwide. James had designed embedded systems for NASA contractors.

The problem was that building custom field electronics in 1991 was **hard**.

Every product required custom PCB design, firmware development, case manufacturing, testing, documentation, and support. Every sensor had its own quirks. Every subsystem could fail in novel ways. And the target market—field geologists—needed tools that were more reliable than commercial-grade hardware, at one-third the price.

It was a terrible business case.

But they'd already invested three years and $68,000. Maggie had finished her PhD but abandoned the academic career it promised. Bob had left a research position doing work he loved. James had turned down good job offers to stay with the company.

They couldn't just quit.

"Maybe we go back to pure data logging," Bob suggested in late March. "Forget the sensors. Just build the best field data entry device we can, let users bring their own instruments."

"So FieldLogger I again?" Maggie asked.

"FieldLogger I worked. Nobody returned those."

"Nobody bought them either. We sold seventeen units."

"Better than losing money on returns."

They debated it for three days. The garage whiteboard filled with product specs, market estimates, and financial projections. Every scenario ended the same way: not enough revenue to sustain the company.

By the end of March 1991, they were out of ideas.

"We have four months left," Maggie announced. "If we don't have a winning product by July, we shut down. Agreed?"

Bob and James nodded.

Four months to save the company.

Or four months to figure out what to do next.

\* \* \*

## Survival Mode: April - November 1991

They didn't find a winning product.

But they didn't shut down either.

Instead, they did what desperate engineers do: consulting work. Lots of it.

Bob took a contract with an oil exploration company doing seismic analysis—boring work, but it paid $4,500 a month. Maggie consulted on a mining survey project that stretched from May through September. James debugged embedded systems for a defense contractor in Boulder, three days a week at $800 per day.

It wasn't glamorous. It wasn't their company. But it kept the lights on.

They stopped paying themselves salaries in April. They cancelled the office phone line and used Maggie's residential number. They stopped taking pre-orders for FieldLogger II and quietly processed the remaining returns.

GeoStructure Systems wasn't dead. It was in suspended animation.

The consulting work bought them time, but it didn't solve the fundamental problem: they didn't have a product that worked. Every month that passed was another month of existence without progress, another month where the dream stayed frozen.

By November, the bank account had stabilized at $18,000—enough for four more months if they stayed lean, if the consulting work continued, if nothing went catastrophically wrong.

But they were tired. Three years of failed products, returns, pivots, and near-death experiences had worn them down.

"Maybe we should just accept this," Maggie said one evening in November, after a long day of writing mine survey reports. "We're consultants now. It's stable. It pays the bills."

Bob and James didn't argue. What was there to say?

The garage still smelled like solder flux. But the disappointment had faded into something worse: resignation.

\* \* \*

## Easter Weekend, 1991

Bob spent Easter weekend at his brother-in-law's house in Denver. Family gathering. Too much food. Kids running around. A welcome distraction from the slow-motion failure of GeoStructure Systems.

His nephew Danny had just turned nine. The kid was obsessed with a Game Boy he'd gotten for Christmas the previous year—played it constantly, barely looked up during dinner.

Bob watched him play *Tetris* at the kitchen table, tongue stuck out in concentration, mashing buttons with the fierce determination only nine-year-olds possess.

The Game Boy was beat to hell. Scratched case, yellowed screen, probably dropped a hundred times. But it kept working.

Bob thought about the digital compass modules in FieldLogger II. Expensive. Delicate. Failed after three days of field use.

This $89 toy from Nintendo had survived months of abuse from a nine-year-old.

The idea began to form.

But it would take one more thing—one specific moment—to crystallize it into something real.

That moment would come eight months later, the day after Christmas 1991, when Danny dropped his Game Boy in his uncle's kitchen and changed everything.

But in March 1991, none of that had happened yet.

GeoStructure Systems had four months left.

The garage still smelled like solder flux and disappointment.

And Bob Kuwahara hadn't quite figured out what he was seeing.

Not yet.
