# Tool notes for the RV Roadside connector

What this plugin knows about the connector's tools, and how sure it is.

## Evidence

The connector requires an OpZero sign in, so its tool schemas could not be listed when these skills were written. The tool names and arguments below come from this plugin's own configuration and earlier skill text. Where the connector's schema differs, the schema is right.

## Tools

| Tool | What it does | Documented arguments |
| --- | --- | --- |
| `find_rv_repair` | Finds repair and service locations near a place | `near`, and `need` with values including `tires`, `towing`, `heavy_truck` and `rv_service` |
| `find_rv_parts` | Finds parts stores near a place | `near`, and a store type. The store type's argument name and values are in the tool schema |
| `find_campgrounds` | Finds campgrounds near a place | `near` |
| `place_details` | Returns the full record for one place | The ID a search returned |
| `rv_guide` | Returns reference guidance | `section`, one of `manuals`, `plates_and_parts`, `breakdown`, `where_to_buy` |

`near` accepts a town and state, a ZIP code, or approximate coordinates. A first search does not need a street address.

As this plugin documents them, all five are lookups. None of them contacts a business, books anything or changes data.

## What results carry

- A name and location for each place, with an ID for `place_details`.
- A straight line distance when the search can compute one.
- A phone number or website when the directory has one.
- Field reports on some places: observations from other travelers, each with a date and a source. Reports marked as samples are placeholders and are not evidence.

## What results do not establish

- opening hours on a given day
- whether a shop accepts a rig of a given size or type
- parts in stock
- prices or wait times
- quality of work

Each of these needs a phone call.

## Sign in and errors

- The connector uses OAuth through OpZero. A user who has not connected it sees an authorization error. The fix is to connect RV Roadside in the client's connector settings and complete the sign in.
- If a search returns nothing, change the location to a larger nearby town or change the category before concluding that nothing is available.

## Privacy

- Use the coarsest location that works: a town or a ZIP.
- Do not put a user's name, phone number, exact coordinates or campsite number into any search.
- If the connector offers a way to share a field report, use it only when the user asks, and keep private contact details and exact location out of it.
