---
name: pick-your-money-page
role: conversion-optimization
description: "Researches a founder's store and recommends which money page to build first (one of 4 formats plus one angle), with reasons and real example pages from the Funnel Vault to study."
toolkit: web-browsing
api_version: n/a
status: beta
version: 2.0
compatibility: Claude Cowork / claude.ai / Claude Code
---

## Purpose
Recommends ONE money page (a format plus an angle, and a runner-up) for the user's product, based on live research of their store and a short interview. A **money page** is a standalone page between a paid ad and the product page: cold visitors from ads aren't ready to buy, and this page does the convincing (explains the problem, changes a belief, builds trust, or creates urgency) before handing them forward. Advisory skill — reads the web, changes nothing.

Every page is two choices:
- **Format** — how the page is built: Article, Listicle, Quiz, or Offer page.
- **Angle** — the argument it makes: Persona, Mechanism, Switch, Trend, Desire, Enemy, Story, Authority, Occasion, or Comparison.

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
   1. Is traffic for this page cold (new audiences) or warm (retargeting, followers, past visitors, email)?
   2. What can you afford for a first-time buyer offer? Is there margin or repeat purchase (subscription, reorders) behind it?
   3. When new customers don't buy, what reason do you hear most? (Too expensive / didn't get it / didn't believe it / chose a competitor / never knew they needed it)
   4. What do you have that competitors can't claim? (Ingredient, process, story, expert, study — or honestly, nothing yet)

3. **THE THREE QUESTIONS** — answer each from the research and interview, and show your reasoning in one line each:
   1. **How aware is the buyer this ad will reach?** → picks the format (table below).
   2. **How many times has this buyer heard the product's main claim?** (market sophistication) → picks the angle (table below).
   3. **Is the traffic cold or warm?** → reality check on question 1. If they disagree, trust the traffic.

4. **DECIDE** — one format + one angle, a runner-up, then apply the feasibility filter.

5. **DELIVER** — the output format at the end.

## The 4 Formats

1. **Article** (the Article That Sells) — reads like a blog post or editorial. Introduces a problem the reader didn't know they had, builds curiosity, challenges a belief, educates, then reveals the product. For unaware and problem-aware buyers, or products that need education.
2. **Listicle** — the pitch as a numbered list ("6 Reasons…", "7 Benefits…", "4 Myths…"), one argument per point, built around one angle. For buyers who understand the category and need reasons to pick you or switch. 6–12 reasons; six strong beat twelve padded.
3. **Quiz** — asks questions, then recommends a product from the answers, so the recommendation feels like a diagnosis. For catalogs where "which one?" or "will it work for me?" honestly depends. Built with a quiz app.
4. **Offer page** — the offer is the headline ("Free first bag"). One deal, one page built around claiming it. For the most-aware buyer who knows the product and needs a reason to buy now. Requires real offer economics. Gamified versions ("pick a card to reveal your gift") are still offer pages.

Length is a separate decision: any article or listicle can run long (story → problem → mechanism → proof → comparison → FAQ → guarantee → buy box) when the page must do the whole selling job alone for a stranger. Earned by how much convincing is needed, not by price.

## Awareness → Format

| Awareness stage | What's in their head | Format | Angles that fit |
|---|---|---|---|
| Unaware | "I don't have a problem." | Article | Story, Enemy |
| Problem aware | "I have this problem, I don't know what fixes it." | Article or Listicle | Enemy, Mechanism, Persona, Desire |
| Solution aware | "I know the kind of thing that fixes it. Which one?" | Listicle or Quiz | Switch, Comparison, Mechanism |
| Product aware | "I know your product. Not sure it's for me, or better." | Listicle or Quiz | Persona, Authority, Trend |
| Most aware | "I want it. Give me a reason to buy now." | Offer page | Occasion, or the offer itself |

Cold prospecting traffic is mostly unaware or problem aware. Retargeting, past visitors and email are product aware or most aware. The ad sets the stage: the page must start where the ad did.

## Sophistication → Angle

- **The claim is new in this market** (few competitors make it) → say it plainly: Desire, Trend, or lead with a strong offer.
- **Everyone makes the same claim** → give a new reason to believe it: **Mechanism** (a nameable difference), or **Comparison** (win side by side on provable facts).
- **The buyer is tired of claims altogether** → talk about the person, not the product: **Persona**, **Story**, **Authority**, or **Enemy** (dismantle the incumbent).

## The 10 Angles

| Angle | The argument | Requires |
|---|---|---|
| Persona | "Made for people like you." Headline names the reader; benefits in their worry order; reviews only from people like them. | Real segment-specific proof |
| Mechanism | "Here's why this works when the others didn't." Old way vs new way, one nameable difference. | A real, nameable mechanism — never invented |
| Switch | "People are switching." | Buyer already uses something in the category |
| Trend | "This is taking off." | Real momentum (sales, reviews, press) |
| Desire | "Your life is better with this." | A pleasure purchase |
| Enemy | "What they're not telling you." Dismantle the incumbent. | True, provable criticisms |
| Story | "I didn't believe it either." A first-person account. | A real person and a real story |
| Authority | "Vets love it." Name the expert the reader trusts. | A real, named expert or institution |
| Occasion | "For this moment." Same page re-dressed for a season or holiday. | A real date — the calendar supplies urgency |
| Comparison | "Us vs them." Side by side with the known alternative. | Facts you can prove |

**Tie-breakers**
- Two buyer groups in the reviews → Persona on whichever format awareness picked; build for the biggest segment first, the other is the next test.
- Nothing fits strongly → Listicle with the strongest provable angle. Most forgiving first page.
- One page, one angle. Never stack angles.

**Feasibility filter (apply last)**
- Little real proof → avoid proof-heavy pages (long pages, Trend, Authority); prefer Article or Mechanism built on honest specifics.
- No offer economics → not the Offer page.
- Never suggest fabricating anything: experts, review counts, timers, press logos, stories.

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

1. **Build this: [format] + [angle] angle, for [specific customer].** 2–3 sentences tied to findings: the awareness stage, how crowded the claim is, the traffic.
2. **Runner-up: [format] + [angle].** When they'd switch to it — and that it's the natural first A/B test against the winner.
3. **Study these.** Two pages for the recommended format/angle, one line each on what to notice. Give the Funnel Vault link (the playbook includes Vault access; it keeps saved versions if the live page changes) and the live URL:

   Formats
   - Article: https://funnels.dtcdailynews.com/pages/atlas-coffee-club/big-coffee-scary-facts-2024/ (live: try.atlascoffeeclub.com/big-coffee-scary-facts-2024) · https://funnels.dtcdailynews.com/pages/atlas-coffee-club/espresso-drinkers-are-loving-this-coffee-concierge-service-heres-why/
   - Listicle: https://funnels.dtcdailynews.com/pages/miracle-made/s6-reasons-sweeps/ · https://funnels.dtcdailynews.com/pages/gruns/glp-gwp-listicle/
   - Quiz: https://funnels.dtcdailynews.com/pages/prose/first-question/ · https://funnels.dtcdailynews.com/pages/petlab-co/twostep_step1_gen/
   - Offer page: https://funnels.dtcdailynews.com/pages/trade-coffee/discovery-trial/ · https://funnels.dtcdailynews.com/pages/native-pet/build-your-own-bogo/
   - Long page: https://funnels.dtcdailynews.com/pages/uproot-clean/wmt008-l4v2-mustyclothes/ · https://funnels.dtcdailynews.com/pages/fatty15/drdrew15/

   Angles
   - Persona: https://funnels.dtcdailynews.com/pages/gruns/glp-gwp-listicle/ · https://funnels.dtcdailynews.com/pages/comrad-socks/10-reasons-why-200k-nurses-wear-comrad-socks-for-12-hour-shifts/
   - Mechanism: https://funnels.dtcdailynews.com/pages/fatty15/meet-fatty15/ · https://funnels.dtcdailynews.com/pages/fatty15/liver-health-deep-science/
   - Switch: https://funnels.dtcdailynews.com/pages/miracle-made/s6-reasons-sweeps/ · https://funnels.dtcdailynews.com/pages/branch-basics/5-reasons-to-switch-to-human-safe-cleaning/
   - Trend: https://funnels.dtcdailynews.com/pages/javvy-coffee/ps/ · https://funnels.dtcdailynews.com/pages/blissy-2/gift/
   - Desire: https://funnels.dtcdailynews.com/pages/cometeer/10-reasons-why-world-mug/
   - Enemy: https://funnels.dtcdailynews.com/pages/atlas-coffee-club/4-myths-big-coffee-wants-you-to-believe/ · https://funnels.dtcdailynews.com/pages/atlas-coffee-club/big-coffee-scary-facts-2024/
   - Story: https://funnels.dtcdailynews.com/pages/blurr/how-i-help-my-la-clients-look-10-years-younger-instantly/ · https://funnels.dtcdailynews.com/pages/perelel/why-i-stopped-taking-a-gummy-prenatal-and-what-i-chose-instead/
   - Authority: https://funnels.dtcdailynews.com/pages/petlab-co-2/stkcta/ · https://funnels.dtcdailynews.com/pages/miracle-made/dermatologist-approved-sheets/
   - Occasion: https://funnels.dtcdailynews.com/pages/cometeer/10-reasons-back-to-school-flash/ · https://funnels.dtcdailynews.com/pages/blissy-2/fallsale/
   - Comparison: https://funnels.dtcdailynews.com/pages/pawsafe/ss-compare-eng-us/ · https://funnels.dtcdailynews.com/pages/cometeer/nespresso/

   Tell the user to also filter the Vault by their own category and format, sorted by most live ads, and pick the closest strategic match (category, customer, angle, offer type) as their reference page.
4. **Gather three things for the build:** product facts; real proof (5–10 favorite reviews pasted); the first-buyer offer. Plus the reference page link and a full-page screenshot.
5. **Gaps.** If research exposed one (no proof, no offer, no differentiator), name it kindly and say what to fix before spending on ads.

## Error Handling

| Error | Cause | Recovery |
|-------|-------|----------|
| Page won't load | Bot protection / JS-only site | Ask user to paste product page text + 10 reviews |
| Vault link needs a login | The user's Vault access isn't active | Give the live URL instead; say the live page may have changed |
| Live example changed or 404s | Brands rotate landers constantly | Point to the Vault link, which keeps saved versions |
| User can't answer interview | Early-stage brand | Recommend Listicle or Article; flag the unknowns as homework |

## Best Practices
- Plain language throughout. The user is a founder, not a media buyer. No jargon without a one-line explanation.
- One recommendation, firmly. A menu of options is the failure mode this skill exists to prevent.
- Quote the user's own customers back to them: review language is where the page's headline usually hides.
