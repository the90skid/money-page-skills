---
name: build-your-money-page
role: conversion-optimization
description: "Builds a money page for the user's Shopify store: researches the product, drafts the copy (optionally modeled on a reference page's structure), and creates it as an unpublished Shopify page via the connector."
toolkit: shopify-mcp-connector
api_version: "2025-01"
graphql_operations:
  - shop:query
  - pageCreate:mutation
status: beta
version: 2.0
compatibility: Claude Cowork / claude.ai / Claude Code
---

## Purpose
Builds ONE money page end to end: research → skeleton → draft copy → create in Shopify (unpublished) → ship checklist. A **money page** is a standalone page between a paid ad and the product page, with no navigation and one job: take a cold visitor from curious to convinced, then send them forward. This is not a homepage and not a product page.

## Prerequisites
- Shopify MCP connector connected (for the build step)
- Required store scopes: `write_content` (pages), `read_products`
- Web browsing/fetch available for research (fallback: user pastes product page text and reviews)
- Optional but recommended: a completed recommendation from `pick-your-money-page` (format + angle + customer)
- Optional: a reference page from the Funnel Vault (link + full-page screenshot) and a brand design system (Claude Design) or `brand.md`

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| product_url | url | yes | The product this page sells |
| ad_concept | text | yes | The ad (copy or idea) that will point at this page |
| reference_page | url or screenshot | no | A money page the user wants to model |
| format | enum | no | article / listicle / quiz / offer |
| angle | enum | no | persona / mechanism / switch / trend / desire / enemy / story / authority / occasion / comparison |
| cta_target | url | no | Product page URL, or a cart link (`https://<store>/cart/<VARIANT_ID>:1`) that drops the product straight into checkout |
| brand_file | file | no | brand.md with colors, fonts, button style, voice |

## Safety

> ⚠️ Mutation skill. It creates ONE new page with `isPublished: false`. It never edits the live theme, the product page, navigation, or any existing page. Show the full draft and get explicit approval before executing any mutation.

## Workflow Steps

> **CRITICAL: Every claim on the page must trace to the research or the user's answers. Never invent reviews, review counts, numbers, studies, experts, or press. If proof for a section is missing, ask for it or cut the section. If a reference page leans on proof the user doesn't have ("backed by 2 clinical studies"), do not imitate the claim — rebuild that section around the proof they do have.**

1. **RESEARCH** — fetch `product_url`, the store homepage, and reviews. Extract: what the product is, price, variants, what makes it different, all visible proof, current offers, and the exact words customers use. Summarize; ask the user to correct.

2. **SKELETON** — two paths:
   - **Reference page provided:** extract its skeleton, never its words. List each section top to bottom: what it is, what job it does (hook / problem / belief change / mechanism / proof / offer / guarantee / CTA), the order the argument runs. That's the outline. Test for the finished page: *the original brand would recognize their structure, but not one sentence of the copy.*
   - **No reference:** use the standard skeleton for `format` (below), shaped by `angle`. If neither is given, pick the best fit from research and say why.
   - Present the skeleton as a **wireframe**: for every section, its purpose, the content it needs, the proof it requires, the recommended visual, and the CTA placement. Do not write copy yet.

3. **BRAND FILE** — if the user has a Claude Design design system, they will design the final page there (see DESIGN NOTE). If `brand_file` exists, follow it. If not, offer to create one: from the store homepage note primary color, accent color, heading and body fonts, button shape, voice (playful, clinical, premium, blunt). Save as `brand.md`; tell the user to reuse it for every page so all pages look like one brand.

4. **DRAFT** — first state the buying argument in 6 lines: the problem this customer already recognizes, why their current solution falls short, why this product fits better, the distinct reasons to believe it (with evidence for each), the objections to answer, the offer it leads to. Then fill the wireframe using the copy rules (below). Show the draft as clean sections with headers. Mark missing evidence as `[MISSING: …]` instead of filling it. Ask for edits. Do not build until approved.
   - **Test variant (offer it):** after approval, offer ONE alternative first screen (headline + hero + first proof) that runs a different angle on the same page. This is the user's first A/B test. Build it only if they say yes.
   - **DESIGN NOTE:** AI writes good copy and mediocre layouts. If the user wants an agency-level design, stop here: give them the approved copy plus a design brief (a grid, a type scale, a spacing scale, consistent components, the full copy, brand colors) to paste into Claude Design with their design system. They come back with the exported design, and you build that instead.

5. **BUILD** — quiz pages: recommend a quiz app instead (see Quiz note). All other formats: create the page via the connector using the operation below, `isPublished: false`. Apply the brand file. Mobile first (most visitors arrive from a phone ad; short paragraphs, images legible at 375px). Every CTA points to `cta_target` — never the homepage. Report the preview URL and what was built, section by section.
   - **No menu, no footer.** A standard Shopify page renders inside the theme with the store header and footer, which breaks the one-exit rule. Ask the user whether their theme has a money-page template without header/footer (for example `page.money`). If yes, set `templateSuffix` to it in `pageCreate`. If no, say so plainly, and give them this prompt to run on a **duplicate** theme (never the live one): "On the unpublished duplicate theme [THEME NAME] only, create a page template called `money` that uses a layout with no header, no navigation and no footer, keeping the theme's head, styles, analytics and pixel. Do not change the live theme or any other template." Then set `templateSuffix: "money"` once it exists.

6. **SHIP CHECKLIST** — walk the user through it (below).

## Page Skeletons (formats)

- **Article** — headline that promises information ("Before You Buy ___, Read This") → story that builds a problem the reader didn't know they had → why it happens → product revealed as the answer → proof → offer.
- **Listicle** — one angle first, then 6–12 numbered points in the buyer's worry order (most important problem near the top), each = claim + why + proof → offer. Six strong reasons beat twelve padded ones. No reason included just to raise the count.
- **Quiz** — see Quiz note below.
- **Offer page** — the offer as the headline → what you get, itemized with real value → the honest reason the deal exists (holiday, launch, first order) → proof → claim it. No fake timers.
- **Long version (any format)** — when the page must do the whole job alone: what this is → why it matters → why it works → who it's for (a section per buyer type if needed) → proof → comparison → FAQ → guarantee → buy box. Closes on the page.

## Angles (shape the argument inside the format)

- **Persona** — headline names exactly who it's for → their worries in their order → reviews only from people like them. Nothing speaks to anyone else.
- **Mechanism** — the old way and the specific thing it gets wrong → the mechanism, named in words a customer could repeat at dinner → show it working → proof proportional to the claim → guarantee (new mechanisms need de-risking).
- **Switch** — "people are switching": what they're leaving, why, what they get instead.
- **Trend** — real momentum (sales, reviews, press) up top; the reasons it's taking off.
- **Desire** — no enemy, no problem; stacked reasons life is better with it.
- **Enemy** — what the incumbent gets wrong (only true, provable points) → the alternative.
- **Story** — a real first-person account from someone the reader relates to. Never invented.
- **Authority** — a real, named expert or institution the reader trusts carries the argument.
- **Occasion** — the same page re-dressed for a real date (holiday, season, launch).
- **Comparison** — side by side with the alternative the buyer knows, on facts you can prove.

One page, one angle. Never stack angles.

## Quiz Note

Do not hand-build quiz logic on a static page. Recommend a quiz app (Octane AI, RevenueHunt, or the current top-rated quiz app in the Shopify App Store) and deliver everything ready to paste in: the questions, the answer options, the proof screens between questions (ask → prove → ask → prove), and the results-page copy where the recommendation is explained in terms of the user's answers. Then continue to the ship checklist.

## Images

Fill the page's images from this ladder, in order:

1. **The store first.** The product page you researched already has images hosted on the Shopify CDN: product shots, lifestyle photos, before/afters. Reuse those URLs directly. Most sections fill this way.
2. **The user's folder.** Ask once: "Any images in the folder I should use?" Ad creatives, UGC, press screenshots all beat stock.
3. **Placeholder + prompt.** For any section still missing its image, insert a clearly marked placeholder block containing: (a) what the image must show, (b) a ready-to-paste AI generation prompt for it, and (c) the size/aspect ratio. The page cannot be published while placeholders remain — this is a ship-checklist item.

Two hard rules:

- **Never use the reference brand's images.** Skeletons are stealable; their photos are copyrighted. Not one.
- **AI images for scenery, never for evidence.** Generated backgrounds and textures are fine. Never generate a fake product photo, a fake "customer," a fake before/after, or anything a buyer would read as proof.

## Copy Rules

- Match the ad: the page's first words keep the ad's exact promise.
- Plain language, short sentences. Specific beats impressive: "4.8 stars from 3,214 reviews," not "loved by thousands."
- Use customers' own review words in headlines and benefits; they sell better than marketing copy.
- Announcement bar at the top with the offer, if one exists.
- Repeat the CTA: after the problem, after the proof, at the end. De-risk at the buy moment (guarantee, cancel anytime).
- No fake urgency: a countdown only if the deadline is real.
- No navigation, no menu, no footer links, no "you might also like."

## GraphQL Operations

```graphql
# shop:query — verify connection before building
query ShopInfo {
  shop {
    name
    myshopifyDomain
  }
}
```

```graphql
# pageCreate:mutation — creates the money page UNPUBLISHED
mutation CreateMoneyPage($page: PageCreateInput!) {
  pageCreate(page: $page) {
    page {
      id
      title
      handle
    }
    userErrors {
      field
      message
    }
  }
}
# variables: { "page": { "title": "<page title>", "body": "<full HTML>", "isPublished": false, "templateSuffix": "money" } }
# omit templateSuffix if the store has no header/footer-free page template yet (see BUILD step)
```

If the connector is not connected: deliver the page as clean HTML plus exact paste instructions (Shopify admin → Online Store → Pages → Add page → `</>` HTML view → paste → Save, keep hidden), and what to check after saving.

## Session Tracking

**On start**, emit:
```
╔══════════════════════════════════════════════╗
║  SKILL: Build Your Money Page                ║
║  Product: <name> · Type: <page type>         ║
╚══════════════════════════════════════════════╝
```

**After each step**, emit one line: `[N/6] <step> → <outcome>`

**On completion**, emit:
```
══════════════════════════════════════════════
MONEY PAGE BUILT (UNPUBLISHED)
  Type:      <page type>
  Page:      <title> (<handle>)
  Preview:   <url>
  Sections:  <n> — <list>
  Next:      run the ship checklist, then publish
══════════════════════════════════════════════
```

## Ship Checklist

1. Open the page on your phone. Does the first screen keep the ad's exact promise?
2. Every claim is true and you can back it up.
3. No image placeholders remain, and no image belongs to another brand.
4. One clear CTA, repeated. No menu, no footer links, no dead links.
5. The click after the CTA lands somewhere that keeps the same promise — product page or checkout, never the homepage.
6. Pixel/analytics installed on the page.
7. Click through and pretend to buy: the offer works end to end.

Close with the honest line and the first test: this page is a draft until traffic votes. Duplicate your best ad and change only the destination: one copy to this page, one to your current product page (or use Meta's A/B test to split evenly). Run at least 7 days, judge on cost per purchase, don't kill either side before it has spent 2–3× your target cost per purchase, and only call a winner on a clear gap (roughly 20% or more) that still holds a few days later. Compare purchase counts, not just percentages. Log the result, win or lose.

## Error Handling

| Error | Cause | Recovery |
|-------|-------|----------|
| `userErrors` on pageCreate | Missing scope / invalid input | Report the exact error; fall back to HTML + paste instructions |
| Connector not available | Not connected in settings | HTML + paste instructions path |
| Research pages won't load | Bot protection / JS-only | Ask user to paste product page text + 10 reviews |
| Reference page won't load | Dead link | Ask for a full-page screenshot instead |

## Best Practices
- Draft in the user's brand voice, not generic marketing voice. The brand file is the source of truth.
- When in doubt between two sections, cut one. Winning money pages are complete arguments, not long ones.
- Never publish. The user publishes after the checklist — that moment stays theirs.
