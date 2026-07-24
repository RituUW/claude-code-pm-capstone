# QA Checklist — Weekly Summary Feature

Prototype reviewed: `prototype/v3.html` (latest version)
Cross-referenced against: `docs/spec-readiness.md`, `prototype/README.md`

## 1. Edge Case List

### Empty states
- No transactions in the past week — what renders in the "Top Insight" card?
- No savings goal set — what renders in the "Savings Goals" card? (Note: spec-readiness.md commits to single-goal for v1; the card also needs a real zero-goal variant, not just a populated one.)
- No app sessions/opens in the past week at all — is the weekly summary still generated and sent? Does a "Welcome back" greeting make sense for a user's very first-ever open?
- Flat week — no category clears the deviation threshold (spec-readiness.md tier 2 fallback: promote goal progress instead of a weak insight).
- Cold start — not enough transaction history to compute a personal average yet (spec-readiness.md tier 3 fallback).
- User arrives via the in-app nav instead of the email — does "Opened from your weekly email" context strip still make sense, or does it need a conditional variant?

### Edge data conditions
- Single transaction in a category — an "average" or "% deviation" computed from one data point is statistically close to meaningless; what's shown?
- Duplicate/miscategorized transactions (e.g., the same DoorDash order tagged under two categories) inflating a total.
- Zero-dollar transactions (authorization holds, fully refunded charges) — included or excluded from totals?
- Negative amounts (refunds, reversals, cashback) — netted against the category total, or shown as a separate line?
- One outsized transaction skewing an average (e.g., a large payment miscategorized into "food delivery").
- Duplicate transactions from sync retries/webhook replays double-counting spend.
- A category landing exactly at the deviation threshold boundary (e.g., exactly 30-40%, per Raj's placeholder number) — which side does it fall on?
- Rounding edge cases on displayed totals vs. underlying decimal values.

### Multi-account scenarios
- Multiple bank accounts connected — does a category total aggregate across all accounts, or only one?
- A transfer between the user's own two connected accounts double-counted as spending.
- One account newly connected mid-week (partial history) blended into an average with another account's full history.
- Shared/joint account — another person's spending mixed into "your" insight without attribution.
- One connected account's sync fails or is stale for the week — does the summary silently use incomplete data, or flag it?

### Permission states
- Notifications off — does the weekly digest still arrive by email, or does the user get nothing at all? (No push infrastructure exists per `docs/codebase-summary.md` — worth reconfirming email isn't silently gated by a notification-off setting meant for push.)
- Background refresh off (mobile) — does opening the app show stale "this week" data, and is staleness communicated?
- General marketing-email opt-out — does that block the weekly summary too, or is it treated as transactional and exempt?
- Bank read-permission revoked mid-week (e.g., Plaid disconnect) — error state, or silent stale/incomplete data?

## 2. PM QA Pass Against `prototype/v3.html`

| # | Check | Result | Notes |
|---|---|---|---|
| 1 | Email preview content matches in-app content | **Pass** | Both show $164 food delivery, ~3x average, consistently. |
| 2 | Top Insight math is internally consistent | **Pass** | $164 ÷ $54 avg ≈ 3.03x — "3x" label checks out. |
| 3 | Savings Goals card matches agreed v1 scope | **Fail — blocking** | `spec-readiness.md` explicitly commits to **single goal only for v1** ("sticking to your recommendation" — confirmed by PM). `v3.html` still shows **two** goals (Emergency Fund + Credit Card Payoff), in both the email and app screen. This is a real spec/prototype drift, not a nitpick. |
| 4 | Nudge copy matches agreed v1 scope (no mid-week alerting) | **Fail — blocking** | `spec-readiness.md` cuts mid-week alerting to backlog per Raj's own recommendation. `v3.html`'s nudge subnote and post-action projection both still promise it ("we'll flag it then too," "we'll alert you mid-week"). Shipping copy that promises a feature that isn't being built is a real user-trust risk — the exact failure mode `docs/decision-brief.md` flags competitors for (Rocket Money, Cleo). |
| 5 | Primary nudge's progress bar accurately represents overage | **Fail — known issue** | Spend is $164 against a $60 limit (~2.7x over), but the bar visually caps at 100% width, which reads as "right at the limit," not "already blown past it by a lot." Not blocking, but should be tracked — undercuts the "specific, not generic" trust the nudge is designed around. |
| 6 | "Cap it" category actions are independent and non-breaking | **Pass** | Each button's state and `event.stopPropagation()` are scoped correctly; clicking one doesn't affect other rows or the primary nudge. |
| 7 | Transaction drill-down total reconciles with the headline total | **Fail — blocking** | Listed transactions sum to $141.95, with a trailing note "+ 2 more orders, totaling $164.00." As written this is ambiguous — it can be read as "the 2 remaining orders total $164," which would make the true total $305.95, contradicting the $164 headline. This directly undermines the one feature (transaction drill-down) built specifically to earn trust — an ambiguous total is worse than no drill-down at all. |
| 8 | Bottom nav destinations (Home / Goals / More) are functional | **Cannot determine** | No click handlers exist on these three tabs — only "This Week" is wired up. Reasonable for a prototype, but needs an explicit answer before real build: is this out of scope for v1, or an oversight? |
| 9 | "Replay prototype" fully resets state for a repeat test session | **Pass** | Resets nudge, category-cap buttons, and all drill-down toggles; goal bars recompute correctly on next view via the existing `goTo` animation. |
| 10 | Email and app screen are internally consistent with each other | **Pass** | Consistent — but this actually reinforces #3: the two-goal mismatch with spec isn't a one-off typo, it's systemic across both screens. |

**Blocking before launch:** #3 (goal scope contradicts agreed spec), #4 (mid-week alert copy promises a cut feature), #7 (transaction total is ambiguous, undermining the trust feature's own purpose).

**Can ship as a known/tracked issue:** #5 (progress bar visually understates overage magnitude — real, but not a correctness or trust break on the same level).

**Needs a decision, not a fix:** #8 (bottom nav scope — a question for the team, not a bug).

## 3. Draft PR Comment for Raj

> Quick question before I sign off — the nudge card's subnote and the post-action projection text both promise mid-week alerting ("we'll flag it then too," "we'll alert you mid-week"), but I have it noted from our spec review that we cut mid-week alerting to backlog for v1 since it'd need a second scheduled job and we don't have push infra to lean on. Did that change, or is this copy left over from before we made that call? Want to make sure we're not shipping a promise to users that the mid-week check isn't actually going to happen yet — feels like exactly the kind of over-promising we flagged as a risk in the competitive research. Happy to send updated copy if this is just a leftover.
