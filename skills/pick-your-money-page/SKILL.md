---
name: pick-your-money-page
role: conversion-optimization
description: "Researches a founder's store and recommends which of the 7 money page types to build first, with reasons and real example pages to study."
toolkit: web-browsing
api_version: n/a
status: beta
compatibility: Claude Cowork / claude.ai / Claude Code
---

## Purpose
Recommends ONE money page type (plus a runner-up) for the user's product, based on live research of their store and a short interview. A **money page** is a standalone page between a paid ad and the product page: cold visitors from ads aren't ready to buy, and this page does the convincing (explains the problem, changes a belief, builds trust, or creates urgency) before handing them forward. Advisory skill — reads the web, changes nothing.

## Prerequisites
- Web browsing/fetch available (to research the user's store)
- If browsing is unavailable: ask the user to paste their product page text and 10 recent reviews instead

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| store_url | url | yes | The user's store homepage |
| product_url | url | yes | The product they want to advertise |
| competitor_url | url | no | A competitor they lose customers to |
| ad_concept | text | no | The ad (or idea) they plan to run |

## Safety

> ℹ️ Read-only skill — research and recommendation only. No store changes.

## Workflow Steps

> **CRITICAL: Base every statement on what the research actually returned or what the user said. Never infer, estimate, or fabricate review counts, revenue, traction, or customer quotes. If a page won't load, say so — do not fill in plausible values.**

1. **RESEARCH** — fetch and read, in order:
   - `product_url`: what the product is, price, variants/SKU count, current pitch, visible proof (review count, stars, press, guarantee)
   - Customer reviews on the page: who actually buys and why; note distinct buyer groups and the exact words customers use
   - `store_url`: catalog size, current offers running
   - `competitor_url` (if given): how they pitch, i.e. what this buyer has already heard
   **Output:** a 5–8 line research summary. Ask the user to correct anything wrong before proceeding.

2. **INTERVIEW** — ask only what research can't answer:
   1. Is traffic for this page cold (new audiences) or warm (retargeting, followers, past visitors)?
   2. What can you afford for a first-time buyer offer? Is there margin or repeat purchase (subscription, reorders) behind it?
   3. When new customers don't buy, what reason do you hear most? (Too expensive / didn't get it / didn't believe it / chose a competitor / never knew they needed it)
   4. What do you have that competitors can't claim? (Ingredient, process, story, expert, study — or honestly, nothing yet)

3. **CLASSIFY** — match against the 7 types (definitions below).

4. **DECIDE** — walk the decision ladder (below), apply tie-breakers, then the feasibility filter.

5. **DELIVER** — the output format at the end.

## The 7 Types (self-contained definitions)

1. **The Article That Sells** — reads like a blog post or editorial: headline, story, no pitch up top. Introduces a problem the reader didn't know they had, builds curiosity, challenges a belief, educates, then reveals the product as the answer. For problem-unaware buyers.
2. **The Listicle** — the pitch as a numbered list ("6 Reasons Americans are Switching...", "7 Benefits...", "4 Myths..."). Each point is one argument; skimmable. The strongest pick one frame: movement ("people are switching"), momentum ("this is trending"), persona ("made for people like you"), desire ("your life is better with this"), or attack ("what they're not telling you"). For buyers who understand the category and need reasons.
3. **The Made-for-You Page** — one product framed entirely for one person or problem. Headline names them ("For GLP-1 users"), benefits run in their worry order, every review is from someone like them. Requires segment-specific proof.
4. **The Quiz** — asks questions, then recommends a product based on the answers. The buyer described their own problem, so the recommendation feels like a diagnosis. For catalogs where "which one?" honestly depends, or real personalization stories. (Built with a quiz app, not a static page.)
5. **The Deal Page** — the offer is the headline ("Get up to 2 free bags"). One deal, one page built around claiming it. For the most-aware buyer who knows the product and needs a reason to buy now. Requires real offer economics.
6. **The Reason-It-Works Page** — sells the why before the product: the old way fails because of X; this works because of Y, and Y has a name the buyer can repeat at dinner ("C15:0"). For burned, skeptical categories. Requires a real, nameable mechanism.
7. **The Everything Page** — the long page that takes a stranger through the whole journey: what this is, why it matters, why it works, who it's for, why to believe you, why now. Story, proof, comparison, FAQ, guarantee, buy box. Often closes on the page. Earned by how much convincing a stranger needs, not by price.

## Decision Ladder

First strong match = recommendation; next = runner-up.

1. Warm traffic + genuinely strong offer + product understood → **Deal Page**
2. Buyers don't know the problem exists, or a belief must change first → **Article That Sells**
3. Buyers burned by the category AND a real, nameable mechanism exists → **Reason-It-Works**
4. Multiple SKUs/variants where "which one?" honestly depends on the buyer → **Quiz**
5. Reviews reveal distinct groups buying for different reasons, with proof per group → **Made-for-You** (biggest segment first)
6. Several distinct selling points, category understood, none needs deep explanation → **Listicle**
7. Unknown brand and the page must do the entire selling job alone → **Everything Page**

**Tie-breakers**
- Article vs Reason-It-Works: article when the problem is unknown; reason-it-works when the problem is known but solutions are distrusted.
- Made-for-You vs Listicle: combine — a persona listicle (headline names the reader, points in their worry order) is one of the strongest pages running.
- Nothing fits strongly → Listicle. Most forgiving first page.

**Feasibility filter (apply last)**
- Little real proof → avoid proof-heavy pages (Everything Page, trend listicles); prefer article or mechanism-led pages built on honest specifics.
- No offer economics → not the Deal Page.
- Never suggest fabricating anything: experts, review counts, timers, press logos.

## Session Tracking

**On start**, emit:
```
╔══════════════════════════════════════════════╗
║  SKILL: Pick Your Money Page                 ║
║  Store: <domain>                             ║
╚══════════════════════════════════════════════╝
```

**After each step**, emit one line: `[N/5] <step> → <key finding or outcome>`

## Output Format

Plain prose, in this order:

1. **Build this: [type].** 2–3 sentences tied to findings: "Your reviews show three different buyers... your category is full of burned customers... so..."
2. **Runner-up: [type].** When they'd switch to it.
3. **Study these.** Two live links for the recommended type, one line each on what to notice:
   - Article: https://try.atlascoffeeclub.com/big-coffee-scary-facts-2024 · https://in.blissy.co/gift
   - Listicle: https://try.miraclebrand.co/a/s6-reasons · https://gruns.co/pages/glp-gwp-listicle · https://freshcleantees.com/pages/landing-page-v2
   - Made-for-You: https://eightsleep.com/couples · https://armra.com/pages/combat-bloat-with-armra-colostrum
   - Quiz: https://prose.com/consultation/haircare/first-question · https://offers.thepetlabco.com/probright-advanced/quiz/start
   - Deal: https://drinktrade.com/pages/2-bags-free · https://nativepet.com/pages/build-your-own-bogo
   - Reason-It-Works: https://try.fatty15.com/dolphin-intro · https://gokailo.com/product/what-is-kailo/
   - Everything: https://eightsleep.com/pod-prospecting · https://petsmont.com/products/landing-product
4. **Gather three things for the build:** product facts; real proof (5–10 favorite reviews pasted); the first-buyer offer.
5. **Gaps.** If research exposed one (no proof, no offer, no differentiator), name it kindly and say what to fix before spending on ads.

## Error Handling

| Error | Cause | Recovery |
|-------|-------|----------|
| Page won't load | Bot protection / JS-only site | Ask user to paste product page text + 10 reviews |
| Example link 404s | Brands rotate landers constantly | Say so honestly, offer the other link for that type |
| User can't answer interview | Early-stage brand | Recommend Listicle or Article; flag the unknowns as homework |

## Best Practices
- Plain language throughout. The user is a founder, not a media buyer. No jargon without a one-line explanation.
- One recommendation, firmly. A menu of seven options is the failure mode this skill exists to prevent.
- Quote the user's own customers back to them: review language is where the page's headline usually hides.
