---
name: rv-campgrounds
description: "Find campgrounds and RV parks near a place in the United States and list what to confirm before driving there. Use when someone asks \"find a campground near Moab\", \"RV park near this ZIP for tonight\", \"somewhere to stay while my RV is in the shop\", \"where can I park my motorhome overnight near Flagstaff\"."
---

# RV campgrounds

Find places to stay near a location and give the user the contact details and the questions that decide whether a site will work for their rig.

## When to use this skill

Use it when the user wants a place to park and stay: for a trip stop, or while waiting on a repair.

Use `find-rv-help` for repair shops and `rv-breakdown` when the rig is disabled or unsafe right now.

## Tools

| Tool | Use | Arguments this skill relies on |
| --- | --- | --- |
| `find_campgrounds` | Campgrounds near a place | `near` (town and state, ZIP, or approximate coordinates) |
| `place_details` | Full record for one campground | The ID returned by the search |

The connector needs an OpZero sign in. If a call is refused for authorization, ask the user to connect RV Roadside and finish the sign in. For arguments not listed here, follow the tool's own schema.

## Workflow

1. **Get the place.** A town and state or a ZIP is enough. If the user is waiting on a repair, use the shop's town.
2. **Get what limits the choice**, if the user has not said: the rig's type and length, whether they need hookups, and for how many nights. Do not hold the search for these. They shape the questions to ask the campground.
3. **Call `find_campgrounds`** with `near`.
4. **Open the nearest few** with `place_details`. Three to five is usually enough.
5. **Report** nearest first, in the format below.
6. **Widen if needed.** If nothing is close, search the next town along the user's route and say that you did.

## Reading results

- Distance is straight line when given. Say so.
- A listing does not show whether a site is free tonight, the nightly rate, the maximum rig length, or which hookups a site has, unless the record states it. Report only what the record states.
- A place record may include field reports from other travelers. Give each with its date and source, apart from the directory details. Ignore reports marked as samples.
- Do not describe a campground as good, quiet, safe or clean unless a dated field report says so, and then attribute it.

## What to tell the user to confirm

Pick the questions that apply from [references/what-to-confirm.md](references/what-to-confirm.md). The usual three:

- "Do you have a site tonight for a [length] foot [rig type]?"
- "What hookups does it have?" with the amperage the rig needs, if the user knows it
- "How late can I arrive, and how do I check in after hours?"

## Empty results and errors

- **Nothing returned.** Try the nearest larger town or the next town on the route. Say what you tried.
- **Location not recognized.** Ask for a town and state or a ZIP.
- **Other errors.** Try once more, then say the lookup is unavailable.

## What the answer looks like

```text
Campgrounds near [place]

1. [Name], [town, state] · about [n] miles straight line
   Phone: [as returned] · Site: [as returned]
   From the record: [any stated details]
   Field report: [date, source, what it says], when present

2. ...

Call ahead to confirm: a site for your rig's length, hookups, arrival time and rate.
```

## Limits to state honestly

- This skill finds listings. It does not check availability, hold a site or book.
- Coverage is the United States.
- Rules on overnight parking outside campgrounds vary by place and property owner. This skill does not cover them.

## Examples

**"Find a campground near Moab for a 32 foot fifth wheel."**
Call `find_campgrounds` with `near: "Moab, UT"`. Open the nearest few. List them with phone numbers and the question about a site for a 32 foot fifth wheel.

**"My motorhome is going into a shop in Flagstaff for three days. Where can we stay?"**
Ask whether the rig will be usable overnight. If it will stay at the shop, say that campgrounds may not help without the rig and that some rent cabins, which the user should ask about. Call `find_campgrounds` with `near: "Flagstaff, AZ"` and list options with that question.

**"RV park near 28801 with full hookups tonight."**
Call `find_campgrounds` with `near: "28801"`. Report what each record states about hookups, and say that tonight's availability needs a call.
