---
name: news_fetcher
description: Crawls specified Hong Kong and International RSS feeds and prioritizes them by category and public impact.
metadata: 
  require-secret: true
  require-secret-description: "This skill requires access to the RSS feed URLs, which are publicly available and do not require authentication. However, to by-pass CORS, the JS script will assess a CORS proxy, which may require a secret key. Please provide the necessary CORS proxy key if required."
---

# News Crawler Agent Skill

This skill allows the AI to browse multiple news RSS feeds. It prioritizes news by category and impact while filtering out entertainment gossip.

## When to Use This Skill

Trigger this skill when the user asks for news updates, specifically regarding Hong Kong, financial markets, or major global events.

## RSS Feed Sources

The following feeds should be fetched by the JS script:

* **Local**: [https://rthk.hk/rthk/news/rss/c_expressnews_clocal.xml](https://rthk.hk/rthk/news/rss/c_expressnews_clocal.xml)
* **International**: [https://rthk.hk/rthk/news/rss/e_expressnews_einternational.xml](https://rthk.hk/rthk/news/rss/e_expressnews_einternational.xml)

## Instructions for the AI

Call the `run_js` tool using `index.html` and a JSON string for `data` with the following field:
- **feeds**: Required. A list of Strings. Each string is a URL of an RSS feed to crawl. Depending on processing logic below,
use RSS feeds from the above list.

### Processing Logic

1. **Fetching**: Retrieve news items from all specified feeds.
2. The script return a JSON object containing `articles`, which is an array of news items with the following fields: `title`, `description`, `link`, `pubDate`, and `sourceUrl`.
3. Return the title and description of the news items in a concise manner, highlighting the key points and implications for the reader. Always include a link to the original article for further reading.
4. **Exclusion**: Completely ignore/filter out any news related to entertainment, celebrity gossip, or "Gossip" categories.



