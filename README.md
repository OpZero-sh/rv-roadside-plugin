# RV Roadside

A motorhome chassis issue, a refrigerator part, and a trailer tire call for different shops. RV Roadside helps identify the component, find nearby service, and put the right contact details and questions in one place.

[Connect with Claude, ChatGPT, Muse Code, or Grok](PROVIDERS.md)

## Try it

- Find RV tire service near Moab, Utah. Give me the phone numbers and what to confirm for a fifth wheel.
- Where can I find the model plate on my RV water heater, and which kinds of stores near Asheville might carry its parts?
- My motorhome will not start at a campground near Flagstaff. Who can work on it, and what should I tell them?
- Find a campground near Green River, Utah, for a 32 foot fifth wheel.

## Install in Claude

In Claude Code, run the following commands.

```text
/plugin marketplace add OpZero-sh/plugins
/plugin install rv-roadside@opzero
```

In Claude chat or Cowork, open the installed plugin’s **Connectors** tab and connect the listed service. Complete the OpZero sign in prompt. Then ask a question or name one of the skills below.

## What is included

### Skills

| Skill | What it does |
| --- | --- |
| `find-rv-help` | Works out whether a problem belongs to the chassis, the house or an appliance, finds repair and service shops near you, and gives the question to ask each one. |
| `rv-breakdown` | For a rig that is disabled or unsafe right now. Puts safety and emergency services first, then finds towing, tire or heavy truck service and writes a script for the call. |
| `rv-parts-and-manuals` | Helps you read a component's data plate, points to the maker's manual, and finds stores that may carry the part. |
| `rv-campgrounds` | Finds campgrounds near a place and lists what to confirm before you drive there. |

Each skill carries reference files that Claude reads when needed: which kind of shop handles which problem, what to have ready before a call, safety actions for fire, propane, carbon monoxide and roadside stops, what to copy from a data plate, and what to confirm with a campground.

### Agent

| Agent | When Claude delegates to it |
| --- | --- |
| `service-scout` | Comparing several service, towing, tire or parts options across categories or towns, and returning a shortlist with a call brief. |

The agent is limited to the connector's five lookup tools. It does not contact or book anyone.

### Hooks

None. The plugin installs no hooks and runs no scripts on your machine during use.

## What the plugin connects to

- **One hosted MCP server**, `roadside`, at `https://rv-roadside.mcp.opzero.sh/mcp`. It requires sign in to OpZero through the connector’s OAuth flow. Each tool call sends what the lookup needs to that server: an approximate location such as a town and state or a ZIP, the kind of service or store, a place ID, or a guide section. The tools this plugin uses are `find_rv_repair`, `find_rv_parts`, `find_campgrounds`, `place_details` and `rv_guide`.
- **Links and phone numbers in answers.** Answers pass on the phone numbers and websites in place records for you to use. The plugin does not call, message or book anyone.

The skills tell Claude to search with a town or ZIP, not a street address, and to keep names, phone numbers and exact locations out of searches.

## Optional files for other clients

These files are not used by Claude. Nothing in them runs unless you run it yourself.

- `integrations/grok_example.py` is a sample script for the xAI API. When you run it without the dry run flag, it reads `XAI_API_KEY` and `RV_ROADSIDE_ACCESS_TOKEN` from your environment, and `XAI_MODEL` if set. It sends your question, the server address, the tool list and that access token to `https://api.x.ai/v1/responses`, so that xAI can call the RV Roadside server for you. With `--dry-run` it prints the request with a placeholder in place of the token and sends nothing.
- `integrations/grok-servers.json` holds the server address, the tool list and the name of the token variable that script uses.
- `integrations/muse-settings.json` is a settings fragment for connecting the server in Muse Code.
- `integrations/openai/` holds the manifest and connection file for the ChatGPT and Codex package.
- `scripts/build_openai.py` builds that package as a ZIP on your machine from the manifests, the skills, the license and the product icon. It makes no network requests.
- `.muse-plugin/plugin.json` is the manifest for Muse Code.

## Sources and coverage

Coverage is the United States. Place search accepts a town and state, a ZIP, or approximate coordinates. Results show straight line distance, which is shorter than the drive. A listing shows that a place is in the directory. Hours, parts in stock, prices and whether a shop can take a particular rig all need a phone call.

RV Roadside is not an emergency service and does not dispatch help. For fire, a propane smell, a carbon monoxide alarm, an injury, or a rig stopped in traffic, call 911 first. Repair procedures and part fit come from the maker's documentation.

- [rv-roadside](https://rv-roadside.mcp.opzero.sh/) · [Privacy](https://rv-roadside.mcp.opzero.sh/?page=privacy) · [Support](https://rv-roadside.mcp.opzero.sh/?page=support)
