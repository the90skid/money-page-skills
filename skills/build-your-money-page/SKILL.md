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
compatibility: Claude Cowork / claude.ai / Claude Code
---

## Purpose
Builds ONE money page end to end: research → skeleton → draft copy → create in Shopify (unpublished) → ship checklist. A **money page** is a standalone page between a paid ad and the product page, with no navigation and one job: take a cold visitor from curious to convinced, then send them forward. This is not a homepage and not a product page.

## Prerequisites
- Shopify MCP connector connected (for the build step)
- Required store scopes: `write_content` (pages), `read_products`
- Web browsing/fetch available for research (fallback: user pastes product page text and reviews)
- Optional but recommended: a completed recommendation from `pick-your-money-page`

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| product_url | url | yes | The product this page sells |
| ad_concept | text | yes | The ad (copy or idea) that will point at this page |
| reference_page | url or screenshot | no | A money page the user wants to model |
| page_type | enum | no | article / listicle / made-for-you / quiz / deal / reason-it-works / everything |
| brand_file | file | no | brand.md with colors, fonts, button style, voice |

## Safety

> ⚠️ Mutation skill. It creates ONE new page with `isPublished: false`. It never edits the live theme, the product page, navigation, or any existing page. Show the full draft and get explicit approval before executing any mutation.

## Workflow Steps

> **CRITICAL: Every claim on the page must trace to the research or the user's answers. Never invent reviews, review counts, numbers, studies, experts, or press. If proof for a section is missing, ask for it or cut the section. If a reference page leans on proof the user doesn't have ("backed by 2 clinical studies"), do not imitate the claim — rebuild that section around the proof they do have.**

1. **RESEARCH** — fetch `product_url`, the store homepage, and reviews. Extract: what the product is, price, variants, what makes it different, all visible proof, current offers, and the exact words customers use. Summarize; ask the user to correct.

2. **SKELETON** — two paths:
   - **Reference page provided:** extract its skeleton, never its words. List each section top to bottom: what it is, what job it does (hook / problem / belief change / mechanism / proof / offer / guarantee / CTA), the order the argument runs. That's the outline. Test for the finished page: *the original brand would recognize their structure, but not one sentence of the copy.*
   - **No reference:** use the standard skeleton for `page_type` (below). If no type given, pick the best fit from research and say why.

3. **BRAND FILE** — if `brand_file` exists, follow it. If not, offer to create one: from the store homepage note primary color, accent color, heading and body fonts, button shape, voice (playful, clinical, premium, blunt). Save as `brand.md`; tell the user to reuse it for every page so all pages look like one brand.

4. **DRAFT** — fill the skeleton using the copy rules (below). Show the draft as clean sections with headers. Ask for edits. Do not build until approved.

5. **BUILD** — quiz pages: recommend a quiz app instead (see Quiz note). All other types: create the page via the connector using the operation below, `isPublished: false`. Apply the brand file. Mobile first (most visitors arrive from a phone ad; short paragraphs, images legible at 375px). Report the preview URL and what was built, section by section.

6. **SHIP CHECKLIST** — walk the user through it (below).

## Page Skeletons

- **Article That Sells** — headline that promises information ("Before You Buy ___, Read This") → story that builds a problem the reader didn't know they had → why it happens → product revealed as the answer → proof → offer.
- **Listicle** — pick one frame first: movement ("people are switching") / momentum ("this is trending") / persona ("made for people like you") / desire ("life is better with this") / attack ("what they're not telling you"). Then numbered points in the buyer's worry order, each = claim + why + proof → offer.
- **Made-for-You** — headline names exactly who it's for → their worries answered in their order → reviews only from people like them → offer. Nothing speaks to anyone else.
- **Quiz** — see Quiz note below.
- **Deal Page** — the offer as the headline → what you get, itemized with real value → the honest reason the deal exists (holiday, launch, first-order) → proof → claim it.
- **Reason-It-Works** — the old way and the specific thing it gets wrong → the mechanism, named in words a customer could repeat at dinner → show it working → proof proportional to the claim → offer with a guarantee (new mechanisms need de-risking).
- **Everything Page** — the full journey on one page: what this is → why it matters → why it works → who it's for (a section per buyer type if needed) → proof → comparison → FAQ → guarantee → buy box. Closes on the page.

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
# variables: { "page": { "title": "<page title>", "body": "<full HTML>", "isPublished": false } }
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
4. One clear CTA, repeated. No menu, no dead links.
5. The click after the CTA lands somewhere that keeps the same promise — product page or checkout, never the homepage.
6. Pixel/analytics installed on the page.
7. Click through and pretend to buy: the offer works end to end.

Close with the honest line: this page is a draft until traffic votes. Run it, read the numbers in a week, iterate or kill without sentiment.

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
