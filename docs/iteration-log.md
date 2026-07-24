# Prototype Iteration Log — Round 1 Usability Sessions

Prototype tested: `prototype/index.html` (v1)
Participants: Sonia, Ayo

## Raw Observations

**What do you think this does?**
- Sonia: "It is telling me the most spend category and how I could save the most."
- Ayo: "It is telling where I am spending the most and how it exceeded than my average."

**How would you arrive at this experience?**
- Unclear to both users.

**Would you change your spending based on this?**
- Ayo: "Yes, I would take action as it clearly shows my food spending is 3 times above average."
- Sonia: "Yes, I would set that weekly limit."

**Magic wand — what would you change?**
- Ayo: "I would add what other categories I can save in. Currently it just shows one category."
- Sonia: "With this I would have to wait for a week [to know] how much I saved — I could know beforehand."

## 1. What's Working

- **The top insight is self-explanatory.** Both users independently and correctly described exactly what the screen shows — highest spending category and how far it exceeded their average — without any prompting or instruction. The novel-insight-over-confirmation design bet from the research holds up in a real session.
- **The nudge drives real intent to act, not just acknowledgment.** Both users said they'd act on it — Ayo cited the specific 3x-average comparison as the reason, Sonia said she'd set the weekly limit. This validates the core hypothesis: a specific, novel insight paired with an explicit-action nudge produces willingness to act, which is the whole point of the feature.

## 2. Top 2 Friction Points

1. **Entry-point / wayfinding confusion — both users, unprompted.** When asked how they'd arrive at this experience, neither user could say. They understood *what* the screen was showing once they saw it, but not *how* it would naturally appear as part of their normal app usage — the connection between the weekly email and the in-app screen isn't reinforced once you're inside the app.
2. **The nudge under-communicates scope and payoff at decision time.** Two related but distinct asks: Ayo wants visibility into savings opportunities across more than one category (the nudge currently surfaces only Food Delivery); Sonia wants to know the projected savings impact *before* committing to the action, not have to wait a week to find out. Both are asking for more information at the moment of decision, not after.

## 3. Highest-Priority Change for Next Round

**Add wayfinding context to the app screen so users understand how and why they arrived there.**

This was the only friction point raised independently by 100% of participants (2/2), in direct response to a discoverability question — a stronger, more unanimous signal than the two separate "magic wand" asks, which point in different directions (breadth vs. timing) and would require larger structural changes (multi-category ranking, pre-computed projections) to test properly. Adding a simple context strip that ties the screen back to the weekly email and reinforces its recurring cadence is a low-cost, high-signal fix to test next: does it resolve the "how did I get here" confusion, and does reinforcing the weekly cadence itself support the retention goal?

The category-breadth and upfront-projection requests are logged for a future round — they're valid but represent a larger scope change than a single-round fix.

## 4. Change Made

`prototype/v2.html` adds a context strip directly below the top bar on the app screen: *"📩 Opened from your weekly email · This check-in refreshes every Monday morning."* This explicitly answers "how did I get here" and reinforces that the experience is a recurring, expected touchpoint rather than a one-off screen — directly targeting the top friction point from this round. All v1 content and interactions (insight, nudge action, goal progress) are unchanged so the next round can isolate the effect of this one change.

## Priority Update — Category Breakdown Moved Up

After reviewing round-1 results, the PM decided to prioritize Ayo's "more than one category" request ahead of Sonia's upfront-projection request. Built directly into `prototype/v2.html` (rather than a separate v3 file): a new "More Places To Save" card between the primary nudge and the savings goal card, showing 2 additional categories (Rideshare, Subscriptions) with their spend vs. average and a lightweight "Cap it" action per row, plus a third category (Coffee Shops) shown as on-track for contrast. The single-category primary nudge (Food Delivery) and its full flow are unchanged, so the next round can test whether broader category visibility increases perceived value without diluting the primary nudge's clarity. Sonia's upfront-projection request remains logged for a future round.

## Round 2 — Agentic Persona Interview on v2

Prototype tested: `prototype/v2.html` (synthetic session — Claude in-character as Priya, Tom, and Amara, not live participants)

**What's working (carried over from Round 1):** Comprehension and action-intent remained strong across all three personas — nobody struggled to understand what the screen was showing.

**New friction surfaced:**
- **Priya:** The hardcoded "Emergency Fund" goal doesn't match her actual goal (Europe trip). Wants the warning mid-week, before overspending happens, not only in a retrospective weekly summary.
- **Tom:** His real friction is upstream of the screen — whether he'd open the triggering email at all. Of what's on-screen, the nudge's phrasing ("so it doesn't creep back up") read as mildly lecturing, which risks his explicit "don't lecture me" boundary. Notably, the explicit-action button design still partially won him over despite his skepticism — validates that decision from the original PM brief.
- **Amara (highest risk):** Two trust-critical gaps — (1) she checks accounts regularly and questioned why this is only reachable via a weekly email rather than on-demand, undermining her goal of replacing a manual spreadsheet; (2) she immediately caught that the shown goal isn't a goal she actually has, and per her profile, a wrong-looking number is a product-ending trust break, not a minor issue.

## Change Made — v3

Rather than isolating one variable, `prototype/v3.html` addresses all three findings at once, since this was a synthetic single-session review rather than a live multi-participant round where isolating variables matters more:

1. **Transaction drill-down (Amara — trust):** The top insight amount and each category row in "More Places To Save" are now clickable and expand to show underlying transactions.
2. **Persistent access (Amara — on-demand):** Added a bottom nav bar (Home / This Week / Goals / More) and updated the context strip to state the screen is available anytime, not just from the email.
3. **Multi-goal support (Amara — mismatch):** Savings Goals card now shows two goals (Emergency Fund + Credit Card Payoff) instead of one.
4. **Mid-week proactive framing (Priya — timing):** Added a subnote on the nudge about mid-week flagging, and reworded the post-action projection accordingly.
5. **Tone softened (Tom):** Nudge question reworded from "...so it doesn't creep back up" to the neutral "Set a $60/week limit for food delivery?"

**Caveat:** These changes address a synthetic interview, not live participants. Recommend a live Round 2 usability session against v3 before treating any of these as validated — particularly the multi-goal and drill-down additions, which add real complexity to the screen and should be checked against real users' tolerance for density, not just against the specific complaints that motivated them.
