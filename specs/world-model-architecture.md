# Open Precedent as a World Model

**Status**: Draft for discussion
**Author**: Vish (with Ronak)
**Date**: 2026-04-21
**Inspiration**: [Block — From Hierarchy to Intelligence](https://block.xyz/inside/from-hierarchy-to-intelligence)

---

## Who Open Precedent Is For

Open Precedent is built for people facing a legal situation they cannot afford professional help for, where the outcome materially shapes their life.

The mother trying to keep custody of her kid. The tenant who just got a 30-day notice. The worker who was fired and doesn't know if it was legal. The person who got served and has no idea what to do next. The immigrant trying to understand their status. The debtor being sued by a collection agency.

These users share a profile that shapes everything about how OP has to work:

- **The stakes are enormous.** An outcome on the line is their home, their kid, their job, their freedom, their ability to stay in the country. Not an hour of billable time.
- **They cannot afford the alternatives.** Westlaw, Lexis, Harvey — not in a thousand years. Even a $10/month subscription competes with Netflix. The economic frame is not "is this cheaper than a lawyer" — it's "can I afford anything at all."
- **They have one situation, not a portfolio.** They are not managing a caseload. They have _a_ problem, and it is the thing keeping them awake at night.
- **They are legally unsophisticated by necessity, not deficiency.** They don't know what "jurisdiction" or "procedural posture" means. They know their landlord said they have to leave, or the judge said something they didn't catch, or the form says "respond within 21 days" and they don't know what a response is supposed to look like.
- **They engage in urgent bursts.** Hearing in three days. Notice arrived today. Just got served. Then silence until the next thing hits.
- **They carry significant emotional load.** This is not research. This is fear, anger, shame, grief — often all at once. The interaction has to meet them where they are.
- **Their privacy is not an enterprise concern. It is a safety concern.** What they tell OP is about their abuser, their immigration status, their custody fight, their criminal record. Data leaks here don't embarrass them. They endanger them.

Everything in this architecture is built against this user. When a design decision could go two ways, we pick the one that better serves the person in the worst moment of their life without access to a lawyer.

---

## The What — What We Mean by "World Model"

A world model is a continuously-updated, structured representation of the world the system operates in — rich enough that an intelligence layer can reason over it, compose actions against it, and produce outcomes without a human routing each step.

For Open Precedent, the world has two halves, both serving the user described above.

### The state of the law

Not "documents in a database." The structured understanding of American law as a living system: what's on the books, what's still good law, what's been overruled, what binds where, what just changed. Jurisdictional authority surfaces, doctrinal relationships, treatment graphs, temporal state. This is the substrate our user relies on — and relies on being correct, because a dead statute cited confidently can cost them their case.

### The state of the user's situation

What's happening to them. What's already happened. What's next on their timeline. What they're trying to accomplish. Who the other side is. Where they are. What they've already tried.

This is not a case file. Our user doesn't think in case files. They think in the plain-English reality of what they're going through. The system holds that reality gently, extracts what it needs ambiently (jurisdiction from a zip code, deadlines from a pasted notice, situation type from how they describe it), and carries it across sessions so they never have to start over.

The world model is not the data. The world model is the _structured understanding_ that makes the data useful for reasoning.

---

## The Why — Why This Framing Matters for This User

Three reasons this architecture is the right one for Open Precedent specifically.

### 1. Context-free answers are dangerous for this user

An attorney who gets a generic answer to a legal question can pattern-match it against the specifics of their case and catch the gaps. Our user cannot. They don't know what they don't know. A confident-sounding generic answer, divorced from their jurisdiction, their deadlines, their procedural posture, can send them down a path that costs them the outcome.

Every response OP gives must be grounded in who this user is, where they are, what they're facing, and what's already in motion. That is not a tone preference. It is a safety requirement. A search box cannot do this. A chatbot cannot do this. Only a system with a real understanding of the user's situation can.

### 2. The most consequential moments happen when the user isn't asking

A pro se litigant's most dangerous failure mode is missing a deadline. Response windows. Filing cutoffs. Statutes of limitations. Hearing dates. None of these show up as questions they think to ask. They show up as lost cases — default judgments, dismissed claims, expired rights — because nobody was watching the calendar.

A system that only answers when asked cannot save these users from that failure mode. A system that understands their situation _can_ — it can warn them when a deadline approaches, flag when a law they were relying on changes, surface a case similar to theirs when one is decided. This is not a convenience feature. For this user, it is often the difference between keeping their home and losing it.

### 3. This user base has been failed by the copilot framing

Every well-funded legal AI product is racing to build the same thing: a faster search engine and a slicker synthesis layer for attorneys who already have Westlaw. Pro se users and the underserved are an afterthought — "maybe there's a free tier." The copilot framing serves the users who already have the most; it does not serve the users who have the least.

A world-model architecture, built from day one around a user with no lawyer, no deadline tracking, no research assistant, and no safety net, produces a categorically different product. Not a free tier of an enterprise tool. A thing designed from the ground up for someone who needs a system that understands their situation and stays with them through it.

---

## How It Works — The Four Layers

The architecture has four layers. The user only ever sees the top one. Everything else exists to make the top one meet them where they are.

### Layer 1: Capabilities

Capabilities are the atomic building blocks. Each is a single well-defined primitive with measurable inputs, outputs, latency, and accuracy. None has a user-facing interface. They are composed, not consumed directly.

Capabilities we have today, built in service of the user:

- **Hybrid search over the corpus** — fast, accurate retrieval across 12.9M chunks of law. The user doesn't call this. The intelligence layer does, on their behalf, with their situation as context.
- **Citation graph traversal** — which cases rely on which, which have been overruled, which authority weighs more where. The user doesn't see citations-by-citations. They see "this is still good law" or "don't rely on this — it was overruled last year."
- **Court and jurisdiction metadata** — so when the user mentions a city or pastes a court document, we know which law applies to them.
- **Case metadata enrichment** — so answers can say "the California Supreme Court held in 2023..." instead of returning a URL.
- **Synthesis** — plain-English answers grounded in real law, with the trusted-friend-who-went-to-law-school tone.
- **Parenthetical extraction** — court-written summaries so we show the user what a case actually said, not our paraphrase.
- **Eval harness** — so we can measure whether our answers are right, not just confident.

Capabilities we need to build, each driven by an unmet need of this user:

- **Document uploader / parser** — when the user pastes the eviction notice, the complaint, the letter from the state, the system extracts deadlines, parties, claims, jurisdiction. Turns a scary piece of paper into structured understanding.
- **Deadline extractor and watcher** — given what the user has told us or what we've parsed from their documents, compute what's due when, and watch the clock on their behalf.
- **Situation classifier** — map the way a user describes their problem (in their words) to the legal structure underneath (jurisdiction, area of law, typical procedural posture, common paths forward). Ambient, not interrogative.
- **Authority watcher** — when a case or statute the user is relying on changes status, flag it. Proactive, not reactive.
- **Jurisdiction watcher** — when new law is decided or enacted in the user's jurisdiction touching their situation, surface it.

Capabilities are built in response to unmet intelligence-layer needs. No capability is built speculatively. No capability exists without a consumer above it.

### Layer 2: The World Model

The world model is the structured understanding, layered on top of raw data, that lets the intelligence layer reason.

**On the law side**, the world model is the difference between a pile of documents and a live map of what binds who, where, and when. Doctrine taxonomies. Treatment graphs. Live-vs-dormant classifications. Temporal state so we can answer "what was the law on the date the user signed the lease." We have the substrate. We have maybe 20% of the structured understanding. The rest is a real engineering investment, and it's the investment that keeps our user safe from confidently wrong answers.

**On the user side**, the world model holds the user's situation as the system understands it. It is built ambiently, from conversation and uploaded documents, not by asking the user to fill out a form. It knows:

- What the user is going through, in human terms
- Who the other side is, if there is one
- Where the user is, so the right law applies
- What has already happened
- What is coming up and when
- What the user is trying to accomplish
- What they've already been told, tried, or paid for

The system holds this lightly. It can be wrong. It can be incomplete. It makes its understanding visible to the user so they can correct it. It never leaks it, never sells it, never uses it against them.

This is the layer we do not have today. It is the single largest investment implied by this architecture, and it is the one that actually unlocks the rest.

### Layer 3: The Intelligence Layer

The intelligence layer observes both halves of the world model, reasons over them, and composes solutions from the capability primitives.

It runs in two modes.

**Reactive mode** — the user asks something or says something or uploads something. The system composes a task against their situation: search this corpus slice with these authority weights, traverse these citations, check these deadlines, synthesize in this tone for this user who is in this state. This is what we have today, but rebuilt so that "compose" is an explicit phase grounded in the world model, not an implicit prompt trick.

**Proactive mode** — the system initiates. On a schedule and on world-model events. For each user with an active situation:

- Is a deadline approaching? Warn them, help them prepare.
- Did a law they were relying on change? Tell them what it means for them.
- Was a case similar to theirs just decided? Surface it in plain English.
- Has the user gone silent near a known deadline? Gentle nudge.

Proactive mode for this user is mostly about deadline awareness and reassurance and warning — not research digests. It is the layer that separates "a system that answers questions" from "a system that stays with you through your situation."

### Layer 4: Interfaces

Interfaces are delivery surfaces over the composer and capabilities. None of them are products. They are different shapes of the same underlying intelligence.

Interfaces that serve this user:

- **Chat** — the core surface. But with onboarding that gently discovers the user's situation without feeling like a form, and with explicit "here's what I remember about your situation" visibility so it never feels like surveillance.
- **Timeline view** — what has happened, what's coming up, what's due when. The one interface distinctly for this user, because no one else builds it (their users have lawyers who track this).
- **Plain-English mode** — "explain it like I'm not a lawyer" as a first-class output format, not a stretch goal. Tied to tone: knowledgeable friend, not database, not disclaimer machine.
- **Document uploader** — paste or upload the notice, the complaint, the letter. The system extracts structure, integrates it into the user's situation model, and explains what it means.
- **Self-help document generation** (longer horizon) — response forms, answers to complaints, declarations. Massive legal effect if done right, massive harm if done wrong. Not v1. But it's the obvious interface over our capabilities once trust and accuracy are earned.
- **Mobile-first everything** — this user has a phone. Often not a laptop. Design constraint, not a feature.

Interfaces we are _not_ building (because they serve a different user):

- Word / Google Docs drafting add-ins
- Brief-check for adversarial filings
- Matter management dashboards
- Enterprise integrations

When those come up in future roadmap discussions, the answer is: different audience. Not now, maybe not ever from us.

---

## How We Plan on Using It

This architecture is not just a description. It determines how we build and how the product behaves.

### As a product

OP becomes a system that accompanies the user through their situation. The default state is not "I have a question"; it is "I have a situation, and OP knows about it, and OP is watching the law and my deadlines on my behalf." Queries are one way to interact — and early on, the main way — but over time, the product's value lives in what it does for the user when they aren't actively asking.

The product's success is not measured in queries answered. It is measured in whether a user in trouble is better off after using OP than they would have been without it. That is a different bar than any legal tech product has ever set, and it is the right bar for this user.

### As an engineering effort

Three shifts in how the team works.

**The backlog reframes around what the user needs.** Issues take the form "the intelligence layer needed to do X for a user in situation Y, and capability Z was missing or insufficient." Not "build feature A." The failure to serve a user becomes the signal for what to build next. No capability is built without a user-facing consumer. No capability is kept without evidence it serves users.

**The user-side world model becomes the top engineering priority.** Not a better model. Not more chunks. Not more data sources. Without an understanding of the user's situation, every other investment has a ceiling. With it, every other investment compounds.

**The team shape flattens.** Deep individual contributors on capabilities (search, graph, evals, pipeline, parsing, deadline math). Directly-responsible individuals on cross-cutting outcomes (close the 22% court-metadata gap; ship situation awareness v1; ship deadline watcher; ship the document uploader). No middle layer whose job is routing information — the world model does that.

### As a differentiation strategy

Every other legal AI product competes on prompt cleverness and model access. OP competes on the quality of its understanding — of the law and of the user's situation.

Competitors copying our chat UI copy a surface. Competitors copying our world model copy years of ingestion, schema design, situation extraction, and signal collection. That is the moat the copilot framing does not produce.

And for this user specifically, the moat is even deeper: we are the only legal tech product in a position to hold this user's context at all. Everyone else is serving attorneys. We are serving the person the attorney never meets.

---

## What This Is Not

- **Not an agent framework rebrand.** Agents are a tactic; the world model is the asset.
- **Not a knowledge graph product.** The graph is an implementation detail.
- **Not a reason to delay shipping.** The current system keeps improving while the world model gets built underneath. Evolution, not rewrite.
- **Not a commitment to ambient monitoring from day one.** v1 of user-side understanding is minimal — remember what the user told us, carry it across sessions, warn on obvious deadlines. Ambient inference and proactive monitoring come next.
- **Not a pitch to enterprise legal.** When the architecture makes us look like we could serve attorneys, that is incidental. We built this for someone else.

---

## Honest Caveats

Two advantages Block has in their version of this that we do not:

- **Two-sided visibility.** Block sees both merchant and consumer. We see the user's side of a legal situation, not the other side, not the judge, not the opposing party. Our world model of any given situation is inherently partial.
- **Paying customers on the primitives.** They have revenue already. We don't yet, and the user we're serving is the hardest user to monetize in legal tech by design. Monetization is real and worth a separate conversation, but it does not drive the architecture.

Neither caveat is a reason to reject the framing. They are reasons to be honest about where OP is on the curve.

---

## Success Criteria

We will know this architecture is working when:

- A user in a high-stakes legal situation gets an answer shaped by their jurisdiction, their deadlines, and their posture, without having had to explain any of those things to us.
- A deadline gets caught by OP that would otherwise have been missed, and the user keeps their home, their kid, their job, their status — because the system was watching on their behalf.
- A new capability ships without touching the chat endpoint or the frontend.
- A new interface (document uploader, timeline, eventually self-help generation) ships by composing existing capabilities, without new intelligence layer work.
- A user describes OP in their own words as "it remembered what was going on with me" or "it told me about a deadline I was about to miss," not "it answered my legal question."
- Competitors' chat UIs look thin by comparison, because the same question gets a categorically better answer when grounded in the user's real situation.

---

## Next Step

This document describes the _what_, the _why_, and the _how_. It stops before the _when_ — phased build-out, sequencing, concrete schemas, capability contracts — which come in a follow-up document once this frame is agreed on.

Monetization and go-to-market are also deliberately deferred. They are real questions, but they do not change the architecture: we are building the right system for this user regardless of how we fund it, and the funding question deserves its own document.

The one concrete ask of this document: agree (or disagree) that this is the architecture Open Precedent is building toward, so every subsequent design decision can be measured against it.
