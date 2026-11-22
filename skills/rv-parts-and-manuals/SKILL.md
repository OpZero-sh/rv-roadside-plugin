---
name: rv-parts-and-manuals
description: "Identify an RV component from its data plate, find the right manufacturer manual, and locate stores in the United States that may carry the part. Use when someone asks \"where is the model number on my RV water heater\", \"find the manual for my RV fridge\", \"what part do I need\", \"RV parts store near me\", \"where can I buy an anode rod near Asheville\", \"who makes my RV's furnace\"."
---

# RV parts and manuals

Help the user identify exactly what they have, point them to the maker's manual, and find nearby places that sell parts for it.

## When to use this skill

Use it when the question is about a component, a part or a manual.

Use `find-rv-help` when the user wants someone to do the repair, and `rv-breakdown` when the rig is disabled or unsafe right now.

## Tools

| Tool | Use | Arguments this skill relies on |
| --- | --- | --- |
| `rv_guide` | The connector's reference guidance | `section`: `plates_and_parts` for where data plates are and what they show, `manuals` for finding a maker's manual, `where_to_buy` for the kinds of stores that carry RV parts |
| `find_rv_parts` | Parts stores near a place | `near` (town and state, ZIP, or approximate coordinates), and a store type chosen from the tool's schema |
| `place_details` | Full record for one store | The ID returned by the search |

The connector needs an OpZero sign in. If a call is refused for authorization, ask the user to connect RV Roadside and finish the sign in.

## Workflow

1. **Name the component.** Which system is it: refrigerator, water heater, furnace, air conditioner, generator, water pump, awning, slide, leveling, brakes, something else? Is the rig a motorhome or a towable?
2. **Find the data plate.** Call `rv_guide` with `section: "plates_and_parts"` and tell the user where to look for this component's plate and what to copy from it. The maker, model number and serial number matter most. See [references/identifying-a-component.md](references/identifying-a-component.md) for what to record.
3. **Point to the manual.** Call `rv_guide` with `section: "manuals"`. Manuals come from the maker named on the plate: the component maker for an appliance, the chassis maker for drivetrain parts, the RV maker for the house. Give the route the guide describes. Do not paste or paraphrase a procedure from memory as if it were the manual.
4. **Decide where the part is sold.** Call `rv_guide` with `section: "where_to_buy"` when the kind of store is unclear. Some parts are common hardware or auto parts. Others are sold only by RV dealers or by the maker.
5. **Find stores.** Call `find_rv_parts` with `near` and the store type that fits. Open the best few with `place_details`.
6. **Report** in the format below.

## Decision points

- **The user does not have the model number yet.** Give the plate location and what to copy, and offer to continue when they have it. A store search can still run, but say that the store will ask for the model number.
- **The user asks whether a part fits.** Fit is decided by the maker's parts list for that model and serial range. Say so, and tell the user to confirm with the maker or the store using the numbers from the plate. Do not confirm fit yourself.
- **The user asks how to install or repair.** Point to the manual. For propane, 120 volt electrical and brake work, say that the maker's instructions and a qualified technician are the right source, and offer `find-rv-help`.
- **The part may be a safety recall item.** Vehicle and equipment safety recalls are handled by the maker and by NHTSA. Suggest the user check their VIN with the maker or at nhtsa.gov/recalls. This connector does not look up recalls.

## Empty results and errors

- **No stores returned.** Try the nearest larger town, then another store type that could carry the part. Say what you tried. Mention that the maker or an RV dealer can usually order a part.
- **The guide does not cover the component.** Say so. Fall back to the general rule: the maker named on the data plate is the source for manuals and parts.
- **Other errors.** Try once more, then say the lookup is unavailable.

## What the answer looks like

```text
What you have: [component], made by [maker if known]. Model and serial: [as given, or where to find the plate].

Manual: [how to get it, per the guide]

Where to buy
1. [Store], [town, state] · about [n] miles straight line
   Phone: [as returned] · Site: [as returned]
   Listed as: [store type]
   Ask: "Do you stock or can you order [part] for a [maker] [model]?"

Not confirmed by these listings: stock, price, hours.
```

## Limits to state honestly

- A listing shows a store exists. It does not show what is on the shelf.
- Distances are straight line.
- Coverage is the United States.
- The connector's guide is general. The maker's documentation decides fit and procedure.

## Examples

**"Where is the model plate on my RV water heater, and who near Asheville might carry parts?"**
Call `rv_guide` with `section: "plates_and_parts"` and relay where the water heater's plate is and what to copy. Call `rv_guide` with `section: "where_to_buy"`, then `find_rv_parts` with `near: "Asheville, NC"` and the fitting store type. List stores with the question to ask.

**"I need the manual for the fridge in my 2019 fifth wheel."**
Ask for the refrigerator's brand and model from its data plate, using `plates_and_parts` to say where the plate is. Then call `rv_guide` with `section: "manuals"` and give the route to the maker's manual for that model.

**"Will this water pump fit my trailer?"**
Do not answer yes or no. Ask for the current pump's maker and model from its label, explain that the maker's specifications decide fit, and offer to find a parts store that can match it.
