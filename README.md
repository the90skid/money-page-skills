# The Money Page — Skills

Two Claude skills that come with **[The Money Page](https://www.dtcdailynews.com)**, the DTC Daily playbook on the sales pages winning ecommerce brands run between their ads and their product pages.

## What's inside

| Skill | What it does |
|-------|--------------|
| `pick-your-money-page` | Researches your store and product, asks a few questions, and recommends which money page to build first — one of 4 formats (article, listicle, quiz, offer page) plus one angle — with example pages from the Funnel Vault to study. |
| `build-your-money-page` | Interviews you for inputs, optionally models the structure of a reference page you share, drafts the copy, and creates the page in your Shopify store as an **unpublished** draft via the official Shopify connector. |

## Install (Claude Cowork)

1. Open Claude Cowork → **Customize** → **Add Personal Plugin** → **Browse Plugins** → **Personal** → **Add Marketplace**
2. Paste this link: `https://github.com/the90skid/money-page-skills`
3. Hit the **+** to install.

Full setup instructions, including connecting the official Shopify connector, are in Part 5 of The Money Page (2nd edition).

## Using ChatGPT instead

Skills are a Claude feature, but both workflows ship as ChatGPT paste-prompts in the [`chatgpt/`](chatgpt/) folder:

- [`pick-your-money-page-prompt.md`](chatgpt/pick-your-money-page-prompt.md) — paste into ChatGPT (browsing on), or save as a Project's instructions
- [`build-your-money-page-prompt.md`](chatgpt/build-your-money-page-prompt.md) — same flow; ChatGPT hands you the final page as HTML and you paste it into Shopify (Online Store → Pages → Add page → `</>` view)

## Requirements

- Claude paid plan (claude.ai, Cowork, or Claude Code)
- Official Shopify connector connected (for the build step)

## Safety

- `pick-your-money-page` is read-only.
- `build-your-money-page` creates one new unpublished page. It never edits your live theme, product pages, or navigation. Theme changes (like a menu-free page template) are only ever made on a duplicate theme you name.

## Updates

Skills get improved over time. Reinstall from this repo to get the latest version — updates are free for Money Page customers, forever.

---

© 2026 DTC Daily. See [LICENSE](LICENSE).
