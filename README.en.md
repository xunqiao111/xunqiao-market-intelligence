<div align="center">

<img src="assets/cover.svg" alt="Xun Qiao — Connect the news. Understand the market." width="100%" />

# Xun Qiao · 讯桥

**News, events, charts and AI research for crypto and U.S. stocks.**

**[Open the website ↗](https://xunqiao.qq10.fun/) · [API documentation](https://xunqiao.qq10.fun/api-docs) · [Feedback](https://github.com/xunqiao111/xunqiao-market-intelligence/issues)**

[简体中文](README.md) · English

**1,000+ source coverage · Free core features · Chinese / English · Dark / light themes**

</div>

---

Xun Qiao connects the questions behind market research: what happened, which assets may be affected, how prices moved, and what evidence to watch next. Use it directly in your browser.

The homepage is available without signing in. A free account unlocks the research workbench, news, markets and published AI reports. Optional premium membership adds AI conversations and developer API access.

![The live Xun Qiao homepage](assets/home.jpg)

## One research workflow

**Discover news → Explore asset relationships → Check prices and sources → Read research → Record conditions → Review changes**

### News and event collections

- Browse **All news, Official updates, AI Picks and AI Reports**.
- Search by message, symbol or source; choose sources, apply filters and save articles.
- Follow an event through grouped coverage and later developments, with summaries and article counts.
- Read AI Picks by importance over the past 24 hours or by time, and explore ongoing macro themes.
- Inspect asset and sector tags, relationship strength and bilingual explanations.
- Incoming items enter the feed automatically while preserving the reading position when browsing older content.

![AI Picks and grouped event coverage](assets/news.jpg)

### An asset-focused workbench

Select one or several assets, then focus on the one you are researching. BTC and NVDA provide the starting workspace.

- Price charts with 5-minute, hourly and daily intervals, plus optional event markers.
- Asset-related news, price ranges, volume, market capitalization and event counts.
- Available derivatives, funding, open-interest, on-chain, supply and protocol observations.
- Upcoming macro, supply, activity and company-calendar context.
- Bookmarks, research notes, observation plans and links to published reports.

Coverage varies by asset, venue and source. Missing fields are not zero.

![The research workbench](assets/workbench.jpg)

### Markets with context

Move from a market list to charts and events, then continue into data and research. Crypto and equities share the research workflow while retaining their own market sessions and units. Check the displayed time, currency, unit and market status when comparing data.

![The markets interface](assets/markets.jpg)

### Structured AI research

Leading assets by related-news count are scheduled for research every 30 minutes, staggered by three minutes across the top ten. Reports combine recent news, prices and available research observations around a forward **five-hour** window.

Explore supporting evidence, directional views, observation conditions, invalidation criteria and risk considerations. Where inputs are insufficient, uncertainty should remain explicit.

- **Research plans:** record conditions to observe; they do not place orders.
- **Report comparisons:** inspect what changed as new evidence arrived.
- **Five-hour reviews:** examine subsequent price paths when the window and data are complete.

The report's Möbius-ring rating is distinct from an event's importance rating. AI probabilities are research estimates, not guaranteed or calibrated trading win rates.

![Published AI reports](assets/reports.jpg)

### Personalization and notifications

Chinese / English, dark / light themes, separate theme background preferences and dynamic / static options. Desktop uses a left navigation rail; mobile uses five bottom tabs. Site announcements are available after login, while market and research alerts are opt-in.

### Premium AI and developer API

Ask follow-up research questions with selected assets and site intelligence as context, without configuring a model provider key.

The read-only developer API covers news, AI Picks, event collections, reports, asset overviews, related events, quote snapshots and research data. **Real-time candlestick data is excluded.** The site provides cURL, JavaScript and Python examples. Keep your key private.

```bash
curl -H "Authorization: Bearer $XQ_API_KEY" \
  "https://xunqiao.qq10.fun/api/v1/news?limit=30&view=compact"
```

## Free and optional premium access

| Capability | Guest / Bronze | Free member / Silver | Premium / Gold |
| --- | :---: | :---: | :---: |
| Complete homepage, market focus, headlines and calendar | ✓ | ✓ | ✓ |
| News, Official updates and AI Picks | Sign in | ✓ | ✓ |
| Workbench, markets and published reports | Sign in | ✓ | ✓ |
| Personal research tools | Sign in | ✓ | ✓ |
| View the API page | Sign in | ✓ | ✓ |
| AI conversation replies | — | Upgrade | ✓ |
| Developer API key creation and calls | — | Upgrade | ✓ |

Registration creates a free ordinary membership. Premium activation, duration and current entitlements are described on the website.

## Reading the visual language

Spheres and rings represent bullish and bearish interpretations for an associated asset. Gold and red each have one to three stars for event importance; **importance is separate from price direction**. Orange marks macro observations. Green and purple events remain part of the relationship system. Tag borders distinguish direct, indirect and watch relationships.

Relationships are research leads, not proof of causation. High importance is not an instruction to trade.

## Getting started

1. [Open Xun Qiao](https://xunqiao.qq10.fun/) and explore the homepage.
2. Create a free account and choose assets in the workbench.
3. Read related events alongside charts, original sources and published reports.
4. Record your own conditions and enable alerts only if you want them.

The first visit downloads interface resources and may take around a minute on a slow connection. Local caching and prepared terminal snapshots help subsequent visits; availability still depends on the network and data inputs.

## About this repository

This is the public **product showcase, documentation, screenshot gallery and feedback repository**. It does not contain the complete website source or a self-hosting package. Publication does not grant redistribution rights to third-party data or components.

The interface uses components and assets including [React Bits](https://github.com/DavidHDev/react-bits). Rights remain with their respective owners; Pro component source is not redistributed here.

## Feedback

Star the repository to bookmark the project, or [open an issue](https://github.com/xunqiao111/xunqiao-market-intelligence/issues) with your device, browser, page, steps and expected result. Remove personal information from screenshots. Never share passwords, verification codes, API keys, membership numbers or private conversations.

Screenshots were captured from the live site on 2026-11-15. Their prices, headlines and analysis are historical interface examples, not endorsements of their factual accuracy or investment advice. Market data and AI output can be incomplete or wrong. Verify sources independently. Xun Qiao is a research tool and does not execute trades.

**[Visit Xun Qiao ↗](https://xunqiao.qq10.fun/)**
