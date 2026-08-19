# Two Truths
**What the contract says, and what you think it says.**

Omnikon National Hackathon 2026 · Problem Statement `Omni_FinTech_8` — Protecting Small Investors from Mis-Sold Products · Team **Twin Mind**

---

## The problem

Most first-time investors walk away from a sale feeling like they understood it. The pitch is short, the person explaining it is usually someone they trust, and the product sounds simple. The contract they sign is a different document altogether.

> *"I can take my money out whenever I want."*

The policy wording usually tells a more conditional story: withdrawal is allowed only in certain situations, and what you get back depends heavily on the year you exit.

Complicated paperwork isn't the whole problem. The real gap is that **nobody ever checks whether the buyer's understanding matches the contract** — not before the money moves, and often not for years afterwards.

The insurer holds the contract. The distributor holds the application form. The regulator might eventually hold a complaint. Not one of them holds the thing that actually caused the problem: what the buyer believed at the moment they said yes.

## What this is

Not another document explainer. We start with the buyer, not the PDF.

1. **Ask four questions** before showing any product information, so we capture the belief rather than accidentally teaching it.
2. **Read the contract** and locate the clauses that speak to those four answers.
3. **Compare the two** and show the buyer's own sentence next to the clause and its page number.

Where they disagree, we hand over a question to ask the seller — not a verdict.

### The four questions

| Question | What we check |
|---|---|
| What do you get back, and when? | Maturity benefit; guaranteed vs projected components |
| Can you take the money out early? | Lock-in period and surrender value |
| What happens if you stop paying? | Lapse, paid-up and revival rules |
| What is this costing you? | Charges and deductions |

### Three output states

| State | Meaning |
|---|---|
| `MATCH` | The belief agrees with the contract |
| `MISMATCH` | The belief conflicts with the contract |
| `NOT CHECKED` | Not enough evidence to verify — we say so instead of guessing |

## What it deliberately does not do

- Recommend whether to buy a product
- Give an investment risk score
- Say a product is safe or unsafe
- Predict returns
- Tell the user what decision to make

We are not advisors and we don't pretend to be. Every finding points back to a clause and a page, and stops there.

A clean result never means "this product is fine." It means we found no mismatch in what we checked — and we show you what we skipped.

## Two modes

**Buyer Mode** — the investor answers the four questions themselves, before they commit.

**Reviewer Mode** — a son, a colleague, a friend answers on someone else's behalf. In practice the person who spots the problem is rarely the person who signed.

## Architecture

AI understands language. Rules make the final decision.

```
1. Four questions        →  buyer answers in their own words
2. Belief normaliser     →  AI     converts the answer into a structured claim
3. Document pipeline     →         PDF parsed and indexed with page references
4. Clause locator        →  AI     finds the relevant section of the contract
5. Term extractor        →  RULES  lock-in, surrender value, charges, lapse, maturity
6. Comparison engine     →  RULES  MATCH / MISMATCH / NOT CHECKED
7. Output                →         belief + clause + page + question to ask
```

We don't want a model deciding whether a contract says three years or five. That is not a judgement call, so rules make it — deterministic and testable. AI is used only where it earns its place: understanding informal answers and finding clauses in badly formatted documents.

## Stack

| Layer | Technology |
|---|---|
| Frontend | React + Vite + Tailwind CSS |
| Backend | Python + FastAPI |
| PDF processing | pdfplumber + PyMuPDF |
| Search | BM25 + embeddings + FAISS |
| AI | Hosted LLM API |
| Rules | Python + YAML |
| Storage | SQLite |
| Evaluation | pytest |
| Deployment | Single container |

No microservices, no agent framework, no separate vector database. The hard part of this project was never infrastructure — it is pulling the right clause out of a badly formatted policy document. That is where the hours go.

## Scope

**Phase 2 must-have:** four-question belief check · one product family (endowment / ULIP) · five key contract terms · mismatch cards with clause and page citations · three output states · five labelled documents

**Should-have:** Reviewer Mode · deferral message · timestamped session · second product family

**Not in Phase 2:** voice input · multilingual support · product ratings · user accounts · cross-user aggregation

None of this is off the table forever. It's off the table for Phase 2, because every extra feature is one more thing that can break.

## Status

Specified, not yet built. Build begins **21 August 2026**.

There are no accuracy numbers here yet, and that is deliberate. Given that the entire project is about people believing claims nobody verified, publishing an unvalidated figure felt like the wrong way to start. Extraction accuracy, false-alarm rate and user findings will be reported only after evaluation against labelled documents.

## Team

**Twin Mind** — A D Suriya · Vikass Y B

---

*Two truths. One comparison.*
