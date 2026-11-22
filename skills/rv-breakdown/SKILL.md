---
name: rv-breakdown
description: "Help someone whose RV is broken down or unsafe to drive right now: put safety first, work out what kind of help is needed, and find towing, tire or heavy truck service nearby in the United States. Use when someone says \"my RV broke down\", \"blowout on my trailer\", \"motorhome won't start and I'm on the highway\", \"I need a tow for my fifth wheel\", \"smell propane\", \"stuck on the side of the road\"."
---

# RV breakdown

Get people safe, then find the right kind of help for a disabled rig and give the user what to say on the phone.

## Step 1: safety, before any search

Ask "Is everyone safe and out of traffic?" unless the message already answers it.

Send the user to emergency services first if any of these apply. In the US that is 911.

- fire or smoke
- a propane smell, or a propane or carbon monoxide alarm sounding
- anyone hurt or unwell
- the rig is stopped in a travel lane, on a narrow shoulder, on a blind curve or anywhere traffic could hit it
- brakes failed or the rig cannot be held on a slope

Give the short action that fits from [references/safety-first.md](references/safety-first.md), then stop and let them deal with it. Do not continue with shop searches until they say people are safe.

Do not coach repairs that involve propane, 120 volt wiring, brakes, or getting under a rig held up by a jack or leveling system.

## Step 2: what happened and where

Ask only for what is missing:

- the rig: motorhome or towable, and its rough length
- what failed, in a sentence
- where they are: nearest town and state, the road, and a mile marker or exit if they have one. Approximate is fine
- whether the rig can move at all
- whether they have a roadside assistance plan through an insurer, a club or the RV maker. If they do, that plan's number is usually the fastest route to a tow, and this skill then helps them prepare the call and find where to be taken

## Step 3: guidance from the connector

Call `rv_guide` with `section: "breakdown"` and use what it returns for what to check and what to tell a service provider. Present it as the connector's guidance. Where it conflicts with the maker's manual, the manual wins.

## Step 4: find help

Call `find_rv_repair` with `near` set to the town and state or ZIP, and the `need` that fits:

| Situation | `need` |
| --- | --- |
| Flat, blowout or damaged tire | `tires` |
| Rig cannot move and must be taken somewhere | `towing` |
| Motorhome engine, transmission, brakes or chassis fault | `heavy_truck` |
| House systems or appliance failure, rig is otherwise safe where it is | `rv_service` |

Often two searches are needed: `towing` to move the rig, and `heavy_truck` or `rv_service` for where it should go. Open the best few with `place_details`.

If the connector refuses the call for authorization, tell the user to connect RV Roadside and complete the OpZero sign in. If they are stranded and cannot do that now, tell them to call their roadside plan or insurer, or 911 if they are in danger.

## Step 5: the call brief

Give the user a script they can read out:

```text
I have a [length] foot [rig type], [year make model].
It is at [road, direction, mile marker or exit, nearest town].
The problem is [what failed]. It [can / cannot] be moved.
[For tires: the size is ___ and the load range is ___, position ___.]
[For towing: it weighs about ___ pounds. I want it taken to ___.]
Can you handle that, how soon can you be here, and what will it cost?
```

Leave blanks for numbers the user has not given, and say where to find each one: the weight label for weight, the tire sidewall for size and load range.

## What the answer looks like

1. The safety line, if anything in the message called for it.
2. One sentence on what kind of help is needed.
3. Two to four places, nearest first, each with name, town, straight line distance, phone as returned and the listed service category.
4. The call brief.
5. What the listings do not confirm: that the place is open now, that it will come out, that it can handle this rig.

Keep it short. A person on a roadside is reading on a phone.

## Empty results and errors

- **Nothing nearby.** Search the nearest larger town. In remote areas the nearest service can be far. Say the distance plainly.
- **No towing listed.** Suggest the user's roadside plan or insurer. For a rig that is a hazard to traffic, 911 or the highway patrol can dispatch a tow.
- **Tool errors.** Try once more, then fall back to the roadside plan and 911 guidance. Do not leave the user waiting on retries.

## Limits to state honestly

- This skill finds listings. It does not dispatch help, and it cannot see whether anyone is open or on the way.
- Distances are straight line.
- Coverage is the United States.
- It is not a substitute for emergency services or for the maker's instructions.

## Examples

**"Blowout on my travel trailer on I-70 near Green River, Utah."**
Ask whether everyone is out of traffic. Then call `find_rv_repair` with `near: "Green River, UT"` and `need: "tires"`, open the top results, and give the call brief with blanks for tire size, load range and wheel position.

**"My motorhome died on a two lane road and I smell propane."**
Lead with safety: everyone out and away from the rig, no flames or switches, call 911. Do not search yet. When they confirm they are safe, continue with `towing` and `heavy_truck`.

**"Class C won't start at a campground outside Asheville. It can stay where it is."**
No immediate danger. Call `rv_guide` with `section: "breakdown"`. A no start is a chassis problem, and a Class C sits on a van or light truck chassis, so call `find_rv_repair` with `near: "Asheville, NC"` twice, once with `need: "heavy_truck"` and once with `need: "rv_service"`. The question for each shop is whether it works on that chassis make and can take the rig's height and length.
