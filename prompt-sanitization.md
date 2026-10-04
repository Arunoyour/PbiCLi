# Prompt Sanitization

Why "go ahead, implement it" is a dangerous instruction in a PDLC (Product Development Life Cycle) workflow, and how to fix it.

## What It Is

Prompt sanitization is the practice of forcing an AI-assisted implementation step to be **descriptive and explicit** before it happens — what exactly will be built, how, and with what scope — instead of letting a vague approval like "go ahead" or "looks good, implement it" stand in for real understanding.

The problem it solves: an AI commonly presents a recommendation at a **high level** ("I'll add caching here, use approach X for the API, and refactor this module"). The human, trusting the summary, replies "go ahead." But a high-level recommendation and the actual implementation are not the same thing — the AI still has to fill in dozens of smaller decisions on its own, and some of those fill-in decisions are effectively guesses (hallucinated specifics) that were never surfaced, never agreed to, and never understood by the human who approved it.

By the time something breaks or looks wrong, the human is left saying "I didn't ask for that" — not because the AI lied, but because the approval was given to a *summary*, not to the *actual plan*.

## Why This Happens

- **Humans approve the gist, not the detail.** "Go ahead" is quick to say and feels efficient, but it transfers trust to a one-line description of work that may span many files, decisions, and trade-offs.
- **AI recommendations compress by design.** A good summary leaves out mechanics on purpose, to stay readable. That's useful for a status update — but dangerous as the thing being approved.
- **Gaps get filled silently.** Where the AI's plan is underspecified, it has to decide *something* to keep moving. Those filled gaps are exactly where hallucinated assumptions creep in, and they're invisible unless someone asks to see them.
- **Follow-up questions rarely happen by default.** Unless explicitly prompted, the human doesn't ask "what does that actually involve?" — and the AI doesn't volunteer the full detail unless it's asked to.

## What "Sanitized" Looks Like vs. What Doesn't

| Unsanitized (risky) | Sanitized (safe) |
|---|---|
| "Go ahead and implement it." | "Before implementing: list every file you'll change, the exact approach for each, and anything you're assuming that wasn't explicitly stated." |
| "Sounds good, do that." | "Walk me through what 'that' means step by step before you start — what gets added, removed, or changed, and why." |
| "Yes, proceed with the recommended option." | "Confirm back to me in plain terms: what will exist after this that doesn't exist now, and what could go wrong with this approach?" |
| Approving a one-paragraph summary of a multi-step change | Asking for the detailed plan *first*, approving the plan, then letting implementation follow the approved plan |

## A Concrete Example

**The scenario:** An AI assistant is asked to "fix the slow search feature." It replies: *"I'll add caching and optimize the search query — this should significantly improve speed. Want me to go ahead?"* The user says "yes, go ahead."

**What actually happens under the hood:** To "add caching," the AI has to decide: what gets cached, for how long, where (in-memory vs. a cache service), and what happens when the underlying data changes. None of that was in the one-line summary. The AI makes reasonable-sounding choices for all of it — including caching results in a way that doesn't invalidate when a product's price changes.

**The result:** Search is faster, but customers start seeing stale prices for up to an hour after a price change. The user never approved a caching strategy with a one-hour staleness window — they approved the sentence "I'll add caching." The actual implementation detail that caused the bug was never surfaced, never asked about, and never understood before "go ahead" was said.

**What sanitizing the prompt would have caught:** Asking, before implementation, "what exactly will be cached, for how long, and what happens when the data changes?" would have surfaced the staleness trade-off up front — turning a silent assumption into an explicit decision the human could approve, reject, or adjust (e.g., "cache for 2 minutes, not an hour").

## How to Apply Prompt Sanitization

1. **Never approve a summary — approve a plan.** Ask the AI to expand "I'll do X" into the concrete list of changes, decisions, and assumptions before saying "go ahead."
2. **Make the AI state its assumptions out loud.** Anywhere the AI had to fill a gap the human didn't specify, that gap should be named explicitly, not buried in the implementation.
3. **Ask "what would I notice differently?"** A good sanitized plan answers this in terms the human can actually observe — not just the technical mechanism.
4. **Treat "go ahead" as a two-step action, not one.** Step one: the AI describes the detailed plan. Step two: the human reviews *that*, not the one-line pitch, before anything is built.
5. **Apply this earlier in the PDLC, not after.** The cheapest point to catch a wrong assumption is before a single line of implementation exists — not during QA, and not in production.

## Key Takeaway

"Go ahead and implement it" is an approval of a *description*, not of the *work*. Prompt sanitization closes that gap by requiring the AI to make its real plan — including the parts it would otherwise quietly decide on its own — visible and understandable to the human, before implementation starts, not after something unexpected shows up.
