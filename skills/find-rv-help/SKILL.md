---
name: find-rv-help
description: "Find RV repair and service shops in the United States for a specific rig and problem, and prepare what to tell the shop. Use when someone asks \"find RV repair near me\", \"who can fix my motorhome\", \"RV tire shop near Moab\", \"mobile RV tech near this ZIP\", \"where can I get my trailer brakes looked at\", or needs to work out whether a problem belongs to the coach, the chassis or an appliance. Also the starting point when it is unclear which RV Roadside skill fits."
---

# Find RV help

Work out what kind of shop the problem needs, find candidates near the user with the RV Roadside connector, and hand back a short list with what to confirm on the phone.

## Check safety first

If the user describes fire, smoke, a propane smell, a carbon monoxide alarm, an injury, or a rig stopped in or beside moving traffic, do not start a search. Tell them to get everyone clear and call 911, then use the `rv-breakdown` skill once people are safe.

## Which skill

- `find-rv-help` (this one): a repair or service shop for a known problem. Also use it when the request is general.
- `rv-breakdown`: the rig is disabled or unsafe to drive right now.
- `rv-parts-and-manuals`: identifying a component, finding its manual, or finding a parts store.
- `rv-campgrounds`: a place to stay.

## Tools

The connector's tools, as documented by this plugin:

| Tool | Use | Arguments this skill relies on |
| --- | --- | --- |
| `find_rv_repair` | Service locations near a place | `near` (town and state, ZIP, or approximate coordinates), `need` (for example `rv_service`, `tires`, `towing`, `heavy_truck`) |
| `place_details` | The full record for one place | The ID returned by a search |
| `rv_guide` | Reference guidance from the connector | `section` (`manuals`, `plates_and_parts`, `breakdown`, `where_to_buy`) |
| `find_rv_parts` | Parts stores | See `rv-parts-and-manuals` |
| `find_campgrounds` | Campgrounds | See `rv-campgrounds` |

The connector needs an OpZero sign in. If a call is refused for authorization, ask the user to connect RV Roadside in their client and finish the sign in, then try again. For any argument not listed here, follow the tool's own schema as the connector presents it. See [references/tool-notes.md](references/tool-notes.md).

## Workflow

1. **Get the essentials.** Ask only for what is missing and changes the answer:
   - the rig: motorhome, travel trailer, fifth wheel, truck camper or van, with year, make and model if known
   - the problem, in the user's words
   - an approximate location: town and state, or ZIP. A street address is not needed
   - whether the rig can be driven or towed to a shop
2. **Decide who fixes it.** Sort the problem into chassis and drivetrain, house systems, appliances, running gear or tires. [references/who-fixes-what.md](references/who-fixes-what.md) maps each to a kind of shop and a `need` value. If it is unclear which maker is responsible for a component, call `rv_guide` with `section: "plates_and_parts"`.
3. **Search.** Call `find_rv_repair` with `near` and the `need` that fits. Use `heavy_truck` for the engine, transmission, brakes or chassis of a large motorhome, `tires` for tire work, `towing` when the rig must be moved, and `rv_service` for house systems and general RV work.
4. **Open the strongest few** with `place_details`, using the IDs from the search. Three to five is usually enough.
5. **Widen if needed.** If nothing suitable is close, say so, then search a nearby larger town or a wider area and state the change.
6. **Report** in the format below, with a call brief.

## Reading results

- Distance is straight line when the result gives one. Road distance is longer, sometimes much longer in mountains or across water. Say "straight line".
- A listing shows that a place exists in the directory. It does not confirm opening hours, that the shop takes RVs of this size, that it has a part in stock, or that it will come out to the rig.
- A place record may carry field reports from other travelers. Give a field report with its date and where it came from, and keep it apart from the directory details. Ignore any report marked as a sample.
- Do not rank shops by quality. The records do not measure quality.

## Empty results and errors

- **Nothing returned.** Try the nearest larger town, then a different `need` that could still do the job, for example `heavy_truck` when `rv_service` is empty and the problem is mechanical. Tell the user what you tried.
- **Location not recognized.** Ask for a town and state or a ZIP.
- **Authorization error.** See the sign in note above.
- **Other errors.** Try once more, then say the lookup is unavailable. If the user is stranded, suggest their roadside assistance plan or insurer, whose number is on the membership or insurance card.

## What the answer looks like

```text
For [problem] on a [rig], you need [kind of shop].

1. [Name], [town, state] · about [n] miles straight line
   Phone: [as returned] · Site: [as returned]
   Listed as: [service category]
   Field report: [date, source, what it says], when present
   Ask: [the one question that decides whether this shop can help]

2. ...

Before you call, have ready: [short list from the call brief]
Not confirmed by these listings: hours, availability, whether they take a rig of this size.
```

Build the "Ask" line and the "have ready" list from [references/call-brief.md](references/call-brief.md).

## Limits to state honestly

- Coverage is the United States.
- This skill finds places. It does not call, book, dispatch or pay anyone.
- Repair procedures and part fit come from the maker's documentation. Do not improvise them.
- Share a field report only if the user asks to, and leave out private contact details and the user's exact location.

## Examples

**"Find RV tire service near Moab, Utah for my fifth wheel."**
Call `find_rv_repair` with `near: "Moab, UT"` and `need: "tires"`. Open the top results with `place_details`. Give phone numbers and tell the user to have the tire size and load range from the sidewall ready, and to ask whether the shop stocks that size and can work on a fifth wheel on site.

**"My Class A diesel is losing air pressure in the brakes. Who can look at it near Flagstaff?"**
This is a chassis problem on a heavy vehicle, and it is a safety problem. Tell the user not to drive it. Call `find_rv_repair` with `need: "heavy_truck"`, and offer `need: "towing"` as well. The question for the shop is whether they service this chassis make and can take a motorhome of this length.

**"The fridge in my travel trailer stopped cooling. Is there someone near 28801?"**
This is an appliance. Call `find_rv_repair` with `near: "28801"` and `need: "rv_service"`. Ask the user for the refrigerator's brand and model from its data plate so they can tell the shop, and point to `rv-parts-and-manuals` if they want the manual.
