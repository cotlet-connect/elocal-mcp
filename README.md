# eLocal API integration and MCP server: connect eLocal to ChatGPT, Claude and any AI agent

**eLocal API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your eLocal Advertiser Portal.** eLocal has an advertiser API for developers, but no official MCP server for your account, no ChatGPT or Claude app and no Zapier, Make or n8n app; its MCP server only searches the developer docs. Cotlet.dev connects your own signed-in eLocal account to any AI agent, so you ask in plain words and the AI tracks calls, missed leads and spend for you.

## What you can ask

> List yesterday's eLocal calls and web leads I did not answer.

> Find this week's eLocal calls under 30 seconds or outside my area for a dispute.

> What did each eLocal campaign cost this month, and what did one booked job cost?

> How much of this week's eLocal budget is left, and which campaign runs out first?

> Pause my eLocal campaigns until Monday after my OK.

## What contractors ask their AI to do in eLocal

From contractors' posts on Reddit, Facebook groups, review sites and the eLocal help center (checked 25 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Call back every lead | "List yesterday's eLocal calls and web leads I did not answer and draft a callback note for each." | Every morning |
| Calls to dispute | "Find calls under 30 seconds, wrong service or outside my area this week, and list them for a dispute." | Weekly |
| Spend by campaign | "What did each eLocal campaign cost this month, and what did one booked job cost?" | Monthly |
| Watch the budget | "How much of this week's budget is left, and which campaign will run out first?" | Daily |
| Pause on a full week | "Pause my eLocal campaigns until Monday after my OK; my crews are booked." | When the week is full |
| Leads into my CRM | "Add this week's eLocal leads with name, phone and service to my CRM." | Your own endpoint or the eLocal API |

Your AI asks you before it sends, buys, replies or changes anything. Payments stay in your hands.

## eLocal API

eLocal has its own Advertiser API for campaign reports and listing settings, documented in the [eLocal Advertiser API docs](https://developers.elocal.com/advertiser-apis/readme).

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its eLocal tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"elocal":{"command":"uvx","args":["elocal-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t elocal-mcp .
docker run -i --rm -e COTLET_API_KEY elocal-mcp
```

Tools: `elocal_guide`, `elocal_status`, `elocal_open`, `elocal_read`, `elocal_click`, `elocal_type`, `elocal_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install elocal-mcp        # or run it once: uvx elocal-mcp connect
elocal-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 elocal_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 elocal_agent.py status    # is eLocal ready in your Cotlet.dev account
python3 elocal_agent.py guide     # eLocal screens and sign-in link
python3 elocal_agent.py read      # open eLocal, read the page, close the browser
python3 elocal_agent.py tools     # list Cotlet.dev tools
python3 elocal_agent.py serve     # start the stdio MCP server (same as no argument)
```

## What you can ask your AI to do in eLocal

These are real jobs eLocal users asked about. Cotlet lets ChatGPT, Claude or any MCP client do them in your own signed-in eLocal account.

### Calls and leads

#### Can AI make sure I answer every eLocal lead and call back the ones I missed?

Partly. Cotlet does not place calls itself. With Cotlet, ChatGPT or Claude reads yesterday's calls and web leads in your signed-in eLocal Advertiser Portal, lists the ones nobody answered and drafts a callback note for each. Nothing is sent until you say yes.

Ask your agent: "List yesterday's eLocal calls and web leads that nobody answered and draft a callback note for each. Send nothing yet."

As an API endpoint: Yes. Save the missed lead list as an endpoint in Cotlet and call it every morning from n8n, Make or Zapier to send it to whoever calls back.

#### Can AI find eLocal calls to dispute?

Partly. eLocal's help centre lets you dispute calls that do not qualify. With Cotlet, your agent reads this week's calls in your signed-in Advertiser Portal and lists the ones under 30 seconds, for the wrong service or outside your area. The dispute itself stays with you.

Ask your agent: "Find calls under 30 seconds, wrong service or outside my area this week in eLocal and list them for a dispute with the reason for each."

As an API endpoint: Yes. Save the call check as an endpoint in Cotlet and call it every Monday from n8n, Make or Zapier to get the list by email.

#### How do I get my eLocal leads into my CRM automatically?

Yes. eLocal has no Zapier, Make or n8n app. With Cotlet, your agent reads this week's calls and web leads in your signed-in Advertiser Portal and adds each one to your CRM with name, phone and service, both signed in in your Cloud Browser. Nothing is added until you say yes.

Ask your agent: "Add this week's eLocal leads with name, phone and service to my CRM. Show me the list before you add anyone."

As an API endpoint: Yes. Save the lead read as an endpoint in Cotlet that returns name, phone and service, and call it every hour from n8n, Make or Zapier to push leads into your CRM.

### Spend and budget

#### What did each of my eLocal campaigns cost this month, and what did one booked job cost?

Yes. With Cotlet, ChatGPT or Claude reads the Results screen in your signed-in eLocal Advertiser Portal and gives you spend and calls for each campaign. Add your booked jobs from a sheet and it works out the cost of one job. Nothing changes in eLocal.

Ask your agent: "What did each eLocal campaign cost this month, and what did one booked job cost? Use my booked jobs from this sheet."

As an API endpoint: Yes. Save the results read as an endpoint in Cotlet and call it on the first of each month from n8n, Make or Zapier to get the cost table.

#### How much of this week's eLocal budget is left, and which campaign will run out first?

Yes. With Cotlet, your agent reads the Budget screen and this week's spend by campaign in your signed-in Advertiser Portal, then tells you what is left and which campaign runs out first at the current pace. Nothing changes in eLocal.

Ask your agent: "How much of this week's eLocal budget is left, and which campaign will run out first at the current pace?"

As an API endpoint: Yes. Save the budget read as an endpoint in Cotlet and call it every morning from n8n, Make or Zapier to post the number to Slack or email.

#### Can ChatGPT or Claude pause my eLocal campaigns when my week is full?

Yes. With Cotlet, your agent opens Campaigns in your signed-in Advertiser Portal, shows which campaigns it would pause and until when, then pauses them. You turn them back on in the portal, and calls in between are not sent. Nothing changes until you say yes.

Ask your agent: "Pause my eLocal campaigns until Monday because my crews are booked. Show me which campaigns and wait for my OK."

## Questions from eLocal users

**Does eLocal have an API?**

Yes, for advertisers. In the eLocal Advertiser Portal you generate a key under Settings > API Keys, for campaign results, campaigns and budgets (checked 25 Nov 2026). Building on it means writing code. Cotlet.dev works in your own Advertiser Portal, with no key or code.

**Is there an eLocal MCP server?**

Only for the docs. eLocal's MCP server searches its developer documentation and does not open your account (checked 25 Nov 2026). Cotlet.dev is a hosted MCP server that works in your signed-in eLocal account with Claude, ChatGPT, Cursor, Codex and n8n.

**How do I connect eLocal to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to the eLocal Advertiser Portal once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words.

**Does eLocal work with Zapier?**

No eLocal app was found on Zapier, Make or n8n (checked 25 Nov 2026). With Cotlet.dev, n8n and your AI agent read your calls and campaigns through one MCP server or API key.

**Can AI make sure I answer every eLocal lead?**

Yes. Your AI lists calls and web leads with no answer from you, drafts a callback note for each and keeps the list each morning. A missed lead is still a paid lead, so speed pays.

**Can AI find eLocal calls to dispute?**

It reads call details in your Advertiser Portal, flags short, wrong service or out of area calls and lists them for you to dispute.

## Find it on

- [eLocal on Cotlet.dev](https://cotlet.dev/integrations/mcp/elocal?utm_source=github&utm_medium=readme&utm_campaign=elocal&utm_content=find)
- [PyPI package elocal-mcp](https://pypi.org/project/elocal-mcp/)
- [Odel](https://odel.app/modules/cotlet/elocal)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/elocal/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-elocal-to-ai-agents-guide)

## Also works with

- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [angi MCP](https://github.com/cotlet-connect/angi-mcp)
- [bark MCP](https://github.com/cotlet-connect/bark-mcp)
- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/elocal?utm_source=github&utm_medium=readme&utm_campaign=elocal

Start free: https://cotlet.dev/signup?app=elocal&utm_source=github&utm_medium=readme&utm_campaign=elocal
