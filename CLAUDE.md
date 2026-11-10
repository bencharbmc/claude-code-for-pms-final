# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Answer only the question the student asks, from the sources they
  name. Don't investigate beyond it or offer a theory about what
  Release 4.2 changed unless they ask for one.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for **Rook Dispatch** at Rook Industries. I replaced Priya
Raghunathan, who left on 21 Aug 2026 with no overlap. Sources: rook-wiki
(About Rook, Glossary, Team directory, Releases, Q3 roadmap, Product briefs)
and `00-rook/company/notes/handoff-from-priya.docx`.

### The company
- Rook sells coordination and provisioning software to independent masked
  **responders** and to the **handlers** and **quartermasters** who support
  them. Rook employs no responders. Publicly it's a logistics and
  workforce-coordination vendor for emergency services.
- 241 staff. HQ is Site Aleph, with offices in Berlin, Singapore and Cornwall.
  Most people work remotely. Pricing is a subscription per active responder.
- Monthly release train with 4.x point releases. Support has three tiers.
  Tickets filed during an active callout skip the queue.
- **Hard rule:** Rook never stores or reconstructs a responder's legal identity
  (Security Policy 4.1). We hold only capability tags, availability and callout
  history. Never design or analyze anything that assumes a link to someone's
  real identity.

### The products
- **Rook Dispatch** (mine): decides who gets pinged about a callout and
  whether they take it, based on availability and proximity. Handlers use the
  desktop console. Responders use their phones. Per Priya, the console and
  mobile app are stable (mobile since 4.1), and routing is where both the
  interesting work and the risk are.
- **Rook Supply**: gear provisioning (requisitions, maintenance, field failure
  reports), used by handlers and quartermasters. It reads Dispatch's Responder
  Availability Record to schedule maintenance.

### People (Dispatch team, per the directory, 2 Sep 2026)
| Name | Role (as used in wiki comments) | Notes |
|---|---|---|
| Helen Achebe | Director of Product | My director. Owns the roadmap and commitments. Site Aleph. |
| Marcus Oyelaran | Engineering manager | Runs Dispatch engineering. Priya: "straight with you", start here. Site Aleph. |
| Wen Li | Staff engineer | Built the routing (who gets pinged). The only real source on how ranking works, since it isn't written down. Berlin. Was away 14–24 Aug. |
| Sofia Marino | Product designer | Owns the console and phone app. Ran the September interviews. Site Aleph. |
| Ravi Menon | Data analyst | Owns the numbers and sends the weekly acceptance report. Singapore. |
| Nadia Hoffmann | Support lead | Owns the tickets and hears handler complaints first. Priya suggests a standing 15 minutes with her. Berlin. |
| Priya Raghunathan | Former Dispatch PM | Left 21 Aug. Marcus has her number. |

Note: Priya's handoff says Marcus "can usually pull numbers," but the
directory says Ravi owns the numbers.

Named customers in the wiki: handlers Aunt Dot (for Vesper), Mr. Ambrose
(Captain Vantage), Halloran (Sgt. Bulwark; asks for things on the Supply side)
and Kip (Meteor Mite and The Gale).

### Vocabulary
- **Callout**: a request for a responder to attend an incident. It's the unit
  of work in Dispatch.
- **Ping**: one callout offered to one responder's phone. A ping ends as
  **taken**, **turned down** or **missed** (no answer before the ping wait ran
  out). Turned-down and missed pings both move on to the next responder, but
  they're counted separately.
- **Ping wait**: how long a ping stays on the phone. It's the same for
  everyone and set per release (90s, then 60s from 4.2).
- **Acceptance rate**: the share of pings taken. It's the headline metric.
  **Time-to-accept** is the median number of seconds to a take.
- **Coverage gap**: no available responder had the required tags, so nobody
  *could* go. That's different from low acceptance, where nobody *would* go.
- **Routing priority**: the score that ranks responders. Its inputs are
  proximity (a travel-time estimate), availability, capability match and
  recent acceptance history. Turning down or missing a ping lowers a
  responder's future rank.
- **Capability tags**: flight, structural-entry, hazmat-tolerant,
  cold-weather, aquatic, crowd-management, de-escalation.
- **Cover identity**: a responder's public persona. Rook has no mapping from it
  to a legal identity.
- **Mutual aid**: responders covering for each other across areas. Not
  supported yet; it's on the Q4 exploration list.
- **Supply terms**: requisition, field failure report, service interval.

### Releases
- **4.0** (7 Apr 2026): new console navigation, responder profile redesign,
  routing override audit log.
- **4.1** (16 Jun): proximity now uses a travel-time estimate, bulk callout,
  more reliable phone push delivery.
- **4.2** (12 Aug): change to who gets pinged (proximity weighted up relative
  to acceptance history), ping wait cut from 90s to 60s, console filters
  persist, three defect fixes.

### Where things stand: 4.2 (what the sources say, no conclusions)
- Nadia (18 Aug): callout tickets are running about 3x normal. They fall into
  two themes: "my phone never goes off" and "it's gone before I can answer."
  She tied the second theme to the 60s ping wait and couldn't explain the
  first. On 26 Aug the volume was flat but still high, split about ⅔ / ⅓. A
  handler emailed her directly.
- Marcus (14 Aug) asked whether the routing change was *meant* to apply to
  responders who have been turning jobs down. He said the config doesn't
  distinguish them, and asked whether that was a decision. Wen said she'd look
  after the 24th. No answer is recorded.
- Priya's view (19 Aug comment and handoff): it's mostly seasonal, since August
  is always soft and should recover in September. The routing change was asked
  for, and she says reverting it "trades one angry group for another." This is
  her read, not an analysis.
- Nobody has posted real acceptance numbers yet. Marcus offered rough ones,
  and Ravi's weekly report is the real source. On 28 Aug Marcus said the team
  would regroup after giving the new PM a week to look at it fresh.

### Q3 roadmap (last reviewed 30 Jun; every item is still owned by Priya)
| Item | Target | Status |
|---|---|---|
| Change to who gets pinged | 4.2 | Committed (shipped) |
| Ping timeout tuning | 4.2 | Committed (the ping wait cut shipped in 4.2) |
| Availability Confidence | 4.2 | Committed, but **not in the 4.2 release notes** |
| Requisition approval chains (Supply) | 4.3 | Committed |
| Handler phone app | Q4 | Exploring |
| Shared cover between responders | Q4 | Exploring |

Committed items are locked, and changes go through Product (Helen). Priya says
some items were squeezed out of 4.2 and that I need a conversation with Helen
about which of them are still Q3 commitments. That conversation hasn't
happened.

### Product briefs
- **Bulk callout** (Jan): send the same callout to several responders at once.
  It shipped in 4.1. The brief noted there's no baseline for
  time-to-full-coverage.
- **Routing override audit log** (Mar): log manual overrides of the routing
  pick (who, when, the alternative chosen, and a reason). It shipped in 4.0.
- **Handler phone app** (8 Sep): notifications, a status view and one-tap
  contact for handlers. Sofia is exploring it for Q4; it isn't staffed. The
  brief cites Aunt Dot's interview and has no baseline.
- **Requisition approval chains** (Jul, Supply): a second sign-off for
  requisitions over $2,000. The brief piles on extra scope (escalation after
  48h, a dashboard, handler visibility, replacing Halloran's spreadsheet).
  Phasing is TBD.

### Open asks Priya left me
1. Write down how Dispatch decides who gets pinged. No document exists.
2. Settle with Helen which squeezed-out 4.2 items are still Q3 commitments.
3. Don't let tickets about the filter persistence change eat my first month
   (Priya calls them cosmetic noise).
4. Use my outsider view in the first month, while it lasts.
