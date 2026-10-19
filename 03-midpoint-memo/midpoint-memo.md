# Legisla — Midpoint Memo: Legislative Monitoring

Oct 18, 2026 · Luciana Elmais + Vinícius Pacífico

Legislative monitoring at Legisla today is a manual chain from client strategy sheets to impact reports for funders: an intern re-types bills, consultants have to remember to flag wins, and the monitoring team checks the websites of Congress and the state legislatures by hand, even after engagements end. The *Monitor* *Legislativo* prototype already automates status collection for the Câmara and the Senado, plus ALESP through an AI route. We propose extending it across the whole chain: automatic intake of the client strategy sheets, milestone alerts confirmed by consultants, early signals from agendas, speeches and news, tracking that continues after engagements close, an impact log for funder reports, and three agents: one that finds similar bills for planning, one that finds successful laws in other democracies, and one that maps narratives across bills for Legisla’s theory of change.

## 1. Process map

Today, legislative monitoring at Legisla is a chain of spreadsheets and people that starts with a client and ends in impact reports for funders. Steps 3 to 8 are manual copying, searching and checking, and that is where the Monitor upgrades the process. Mapping was quick, since Legisla’s team had already built its own map of the processes and a list of where they think agents could help, the [Mapa de Agentificação](https://claude.ai/artifact/PgU1tPsCpn7C283CB17Nqn).

![Today vs. what we are building: 8 steps, frictions F1–F8, human judgment H1–H6](check process map image)

## 2. Categorized frictions

The table lists the 8 frictions that add no value and that the Monitor and its agents should remove. The 6 places where Legisla’s team can still add a lot of value are marked H on the map.

| # | Friction today | Step | Category | Upgrade |
| --- | --- | --- | --- | --- |
| F1 | An intern re-types the bills from every client sheet into one central list; sheets vary in format and often miss bill numbers | 3 | Automate | Automatic intake: the Monitor reads client sheets directly; only unclear rows go to a person |
| F2 | Wins reach the monitoring team only if a consultant remembers to report them | 4 | Automate | Monitor flags milestones (committee votes, approvals, new laws) and asks the consultant to confirm |
| F3 | Consultants search by hand for similar bills, house by house and state by state | 5 | Automate | Similar-bills agent: finds related bills in the same house and in other states |
| F4 | Status is checked by hand on each house’s website, including houses with no API | 6 | Automate | More routes, including a crawler for houses without an API and a deterministic ALESP route; check times set from data |
| F5 | Tracking after the engagement ends depends on someone remembering, and a bill nobody checked looks the same as a stalled one | 6 | Automate | Long-tail tracking with a daily digest to the consultant when a bill moves; route health flags failed reads |
| F6 | Official data shows a move only after it is decided; agendas, urgency requests, speeches and news are not watched, so advisors can miss the moment to act | 6 | Automate | Early-signals layer: agendas, procedural moves, key actors' speeches and news, flagged before the official update |
| F7 | Bills sit in a repository; nobody reads across them for patterns | 7 | Automate | Narrative agent: maps similarities and narratives across bills |
| F8 | Impact evidence is rebuilt from e-mails and memory for every report | 8 | Automate | Impact log: movements and confirmed wins per client and period |

**Reasoning.** We sorted each friction by one question: would a skilled person’s judgment change the outcome of this step? Re-typing bills, checking websites, remembering to look and digging up evidence need no judgment, so time spent there is cost. Choosing a bill, setting strategy, deciding what counts as impact and telling that story to funders do need judgment, so effort there is the work itself. One friction in the prototype is deliberate and stays: AI readings are checked by Apps Script before they enter the base, so an AI error cannot corrupt the record that impact reports rely on.

## 3. Prototype: Monitor Legislativo

Legisla’s team built the prototype, the [Monitor Legislativo](https://script.google.com/macros/s/AKfycbzVVHa0dtcF_XiZmNu8NQ32awKN_aO81PGwUYrLuNEHeRtW72Ky5XkDPYh4z-U72EUW/exec): a Google Apps Script app on top of a Google Sheet. It already:

- registers bills through a form (house, type, number, year, topic, priority, owner and a link to the bill page) or by bulk import of team spreadsheets;
- tracks 3 bills in 3 houses (Câmara, Senado and ALESP) and checks for news every morning, without anyone opening a legislative website;
- reads the Câmara and Senado APIs automatically and uses an AI route for ALESP, which has no API;
- shows news from the last 14 days and days without movement for each bill in a web panel;
- follows the membership of 49 Câmara and Senado committees, with seats by party or bloc, and alerts when members join or leave; this helps the team plan strategies to get bills passed.

We are working with the team to build the final product from this prototype, iterating on it together. Section 4 lists what we will add.

![How the prototype works: two routes, one checked base, two outputs](check Prototype image)

## 4. Proposed scope for the final deliverable

Legisla’s Mapa de Agentificação already lists its processes and the agents it wants to use. We will prioritize some of the agents flagged there, starting with the ones that remove the monitoring frictions in section 2. A natural first pick is the legislative tracking analyst for houses without an API, which the Mapa rates high impact and medium effort; the “Routes for every house” item below builds it.

**Build (tool)**

1. **Automatic intake** — the Monitor reads the client strategy sheets as they are and sends only unclear rows to a review list (F1).
2. **Milestone alerts with confirmation** — the Monitor flags committee votes, approvals and new laws and asks the consultant whether to report it as impact (F2, H3).
3. **Similar-bills agent** — finds bills like each monitored one in the same house (for legislative planning) and in other states (for strategies to copy); consultants choose what to use (F3, H4).
4. **International benchmark agent (new)** — finds comparable laws that passed and showed results in other democracies (EU, US, UK and Latin American peers), with a link to every source; consultants judge what transfers to Brazil (H4).
5. **Routes for every house** — expand the routes the team has been building, including a crawler for houses without an API and a deterministic ALESP route that replaces the AI read for São Paulo. Check times come from data: a look-back at past bill histories shows the days and hours each house records moves, and a few weeks of hourly checks measure how long moves take to appear in each API. When a monitored bill is on that day's agenda, its route checks every 15–30 minutes (F4).
6. **Long-tail tracking with a daily digest** — bills keep being tracked after the engagement closes, and the consultant gets a daily digest whenever a bill moves; every route shows its last good read (F5).
7. **Early signals** — besides official status, the Monitor watches committee and plenary agendas, urgency requests, rapporteur reports, speeches by each bill's key actors (author, rapporteur, committee chair, party leaders) and news; consultants can also log what they hear. Signals stay marked as unconfirmed and out of the impact log; urgent ones (bill on an agenda within 48 hours, an urgency request) alert at once, the rest go in a daily digest (F6).
8. **Narrative agent (new)** — reads across the repository for similarities and narratives that advance Legisla’s theory of change (F7, H5).
9. **Impact log** — movements and confirmed wins per client and period, exportable for funder reports (F8, H6).
10. **Company home** — move the sheet, script and Drive folder from a personal Google account (about 500 bills of capacity) to a Legisla account.

**Routine (people)**

- Consultants still choose bills, write strategy and decide which similar bills and strategies to use (H1, H2, H4); their strategy sheets stay as they are.
- A short weekly review: consultants confirm flagged milestones, the monitoring team clears the review list and approves any new route.
- The team decides which narratives to use for the theory of change (H5), and the monitoring team drafts impact reports from the log (H6).

**Out of scope for now:** the vote radar, topic search and automatic cross-house linking, which are on the prototype’s own roadmap. The similar-bills agent only suggests related bills for consultants to review; it does not link bills across houses automatically.

**Validation plan**

| Test | How | Target (proposed) |
| --- | --- | --- |
| Coverage | Import every bill Legisla supports today | 90% of rows import with no manual fix |
| Accuracy | For 2 weeks, check a sample of bills by hand on each house’s site, ALESP included | No missed movement; every failed read flagged within 1 day |
| Timeliness | Compare alert time to movement date | Same morning for API houses; next business day without an API |
| Early signals | For each urgent signal, record whether the official move followed and how much earlier the alert came | Agenda alerts arrive at least 1 business day before the vote |
| Usefulness | Owners mark each digest useful or not | At least 4 in 5 marked useful |
| Benchmark agent | Consultants rate each suggested foreign law | Every suggestion has a working source; at least 1 in 3 judged relevant |
| Ownership | Count bills with a named owner | 100% |
| Impact report | Rebuild the last impact report using only the impact log | Every reported win traces to a logged movement |

**Biggest challenge so far.** Getting the information on bill movements, reliably and from every house, including the ones without an API. We think that once we can get that data, building the screens and views the team uses to follow its bills will be the easier part.

## Open questions for Legisla

- How long after an engagement ends should a bill keep being tracked?
- What counts as a win worth reporting to funders: committee approval, plenary vote, enactment, regulation?
- How often are impact reports produced, and for which funders?
- Who on the monitoring team would own the weekly review?
