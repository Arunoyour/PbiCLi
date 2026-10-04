# Identity Features Gap — Garbage In, Garbage Out

How weak identity data quietly breaks PDLC (Product Development Life Cycle) solutions, and how to catch it before it does.

## What It Is

The "identity features gap" is the difference between the identity data a solution *assumes* it has (clean, unique, well-formed customer/user/entity identifiers) and the identity data it *actually* receives from upstream systems (duplicated, inconsistent, incomplete, or mismatched).

Most PDLC solutions — matching engines, personalization systems, fraud/risk scoring, customer 360 views, recommendation pipelines — are built on an implicit promise: "give me clean identity signals (name, email, phone, device ID, account ID, etc.) and I'll produce accurate output." When that promise is broken upstream, the solution doesn't fail loudly. It fails quietly, by producing confident-looking but wrong output.

This is the classic **Garbage In, Garbage Out (GIGO)** principle applied specifically to identity data: no amount of downstream intelligence (ML models, business rules, fuzzy matching) can fully compensate for identity inputs that were wrong, missing, or inconsistent at the source.

## Why Identity Data Is Especially Prone to This

Identity data is harder to get right than most other data types because:

- **It comes from many sources** — web forms, mobile apps, call centers, third-party data, legacy systems — each with its own validation rules (or none).
- **It changes over time** — people change emails, phone numbers, addresses, and names (marriage, legal name change), but systems often keep stale records.
- **It's entered by humans** — typos, nicknames, abbreviations, and inconsistent formatting ("Bob" vs "Robert", "St." vs "Street") are the norm, not the exception.
- **There's no single global key** — unlike a product SKU, there's rarely one universal identifier that reliably links the same person across every system.
- **Small errors compound** — a single mismatched identity record can cascade through every downstream feature built on top of it.

## Common Causes of the Gap

| Cause | Example |
|---|---|
| Duplicate records | "John Smith" and "J. Smith" treated as two different customers |
| Missing fields | Email captured at signup, but phone number never collected |
| Inconsistent formatting | Phone numbers stored as `(555) 123-4567` in one system and `5551234567` in another |
| Stale data | Customer's old email still marked as "primary" after they switched providers |
| Silent merges/splits | Two accounts quietly merged by a batch job, breaking historical links |
| No validation at entry | A form accepts `abc@abc` as a valid email |
| Cross-system key mismatch | CRM uses `customer_id`, billing system uses `account_no`, and no reliable mapping exists between them |

## How to Identify the Gap (Before Building)

Before a PDLC solution is designed or extended, run a short identity audit:

- [ ] What identity fields does the solution assume exist, and are they *actually* populated, consistently, across all source systems?
- [ ] What percentage of records have duplicate or near-duplicate identities?
- [ ] Is there a single, reliable key to join identity data across systems — or does it depend on fuzzy matching (name + DOB, name + address)?
- [ ] How stale can identity data be before it's wrong (e.g., an email bounced 6 months ago but is still "active")?
- [ ] What happens today when two records *should* be the same person but aren't matched — is that even being measured?
- [ ] Who owns identity data quality — is there a single accountable team, or does every system assume someone else is validating it?

If most of these can't be answered confidently, the gap exists and will surface downstream — usually as a production bug, not a design discussion.

## How to Address It Before Proceeding

1. **Profile the data first.** Run data-quality profiling on real identity data (completeness, duplication rate, format consistency) before committing to a solution design.
2. **Define a canonical identity model.** Decide what "one identity" means for this system (one person, one household, one device) and document the matching rules explicitly.
3. **Validate at the point of entry**, not just downstream — it's far cheaper to reject or correct a bad email at signup than to detect it three systems later.
4. **Build identity resolution as its own layer**, not a side effect of whichever system touches the data first. Treat it as infrastructure, reused by every downstream solution.
5. **Monitor identity quality continuously.** Match rates, duplicate rates, and field completeness should be tracked like any other production metric, not checked once at launch.
6. **Make the gap visible to stakeholders.** If leadership assumes "the data is fine," show them the actual duplicate/incomplete rates before they commit budget to a solution built on top of it.

## A Concrete Example

**The scenario:** A retailer builds a personalization engine to recommend products based on purchase history, keyed on customer email.

**The hidden gap:** The same person shops as a guest (no account, order tied to a one-time email), later creates an account with a different email, and also has a loyalty card tied to their phone number. The system has no way to know these three records are one person.

**The result:** The personalization engine sees three "different" low-activity customers instead of one high-value repeat customer. Recommendations are generic and irrelevant. The marketing team, looking only at the model's output, concludes "personalization doesn't work for us" — when the real problem was never the model. It was that the identity data feeding it was fragmented before it ever reached the algorithm.

**What fixing it looks like:** Standing up an identity resolution step — matching guest orders, account records, and loyalty data into one unified customer profile using a combination of email, phone, and purchase-pattern matching — *before* the personalization engine runs. Once the input is one clean identity instead of three fragments, the same model produces materially better recommendations with no change to its logic.

## Key Takeaway

A PDLC solution is only as good as the identity data it's built on. Sophisticated downstream logic cannot fix a broken identity layer — it can only make wrong conclusions with more confidence. Identify and close the identity features gap *before* investing in the solution itself, not after the first batch of bad output shows up in production.
