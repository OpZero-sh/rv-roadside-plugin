---
name: find-rv-help
description: "Find RV repair, parts, campgrounds, or manufacturer manuals in the United States and prepare the details a shop will need."
---

# RV Roadside

Start with the rig and the immediate need. For people in immediate danger, stranded in traffic, or encountering smoke, fire, or gas odor, prioritize moving clear and contacting local emergency services from safety. For a parts or service lookup, ask only for missing details that change the answer: coach or trailer, year/model, appliance label, approximate location, and whether the rig can move.

- Use `rv_guide` with section `manuals`, `plates_and_parts`, `breakdown`, or `where_to_buy` as appropriate. Look up the coach, chassis, or component maker named on the relevant data plate. Use the manufacturer's cited source when stating fit or procedures.
- Route service to `find_rv_repair` with `need` such as `tires`, `towing`, `heavy_truck`, or `rv_service`. Route a part to `find_rv_parts` with the matching store type. For a place to stay use `find_campgrounds`. Use a town and state, ZIP, or approximate coordinates in `near`. A first search does not need a street address.
- Open the best matches with `place_details`. Give the phone or site returned, straight-line distance if available, any dated field report with its provenance, and one precise question for the shop. For example, ask whether a service truck can handle the rig's tire size. A directory listing does not verify stock, opening hours or rig capacity.
- A lookup only retrieves places. Keep private contact details and the user's exact location out of shared field reports.
