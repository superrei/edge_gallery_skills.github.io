---
name: news_fetcher
description: Crawls specified Hong Kong and International RSS feeds and prioritizes them by category and public impact.
metadata: 
  require-secret: true
  require-secret-description: "This skill requires access to the RSS feed URLs, which are publicly available and do not require authentication. However, to by-pass CORS, the JS script will assess a CORS proxy, which may require a secret key. Please provide the necessary CORS proxy key if required."
---

# News Crawler Agent Skill

This skill allows the AI to browse multiple news RSS feeds. It prioritizes news by category and impact while filtering out entertainment gossip.

## RSS Feed Sources

The following feeds should be fetched by the JS script:

* **Local**: [https://news.mingpao.com/rss/ins/s00001.xml](https://news.mingpao.com/rss/ins/s00001.xml)
* **International**: [https://news.mingpao.com/rss/ins/s00005.xml](https://news.mingpao.com/rss/ins/s00005.xml)

## When to Use This Skill

Trigger this skill when the user asks for news updates, specifically regarding Hong Kong, financial markets, or major global events.

## Instructions for the AI

Call the `run_js` tool using `index.html` and a JSON string for `data` with the following field:
- **feeds**: Required. A list of Strings. Each string is a URL of an RSS feed to crawl. Depending on processing logic below,
use RSS feeds from the above list or any other relevant feeds.

### Processing Logic

From the enquiry of the prompt, identify if is a general enquiry, or a specific enquiry (e.g. "Give me the latest finance news"). Based on the enquiry, apply the following rules:

#### General Enquiry (e.g. "Give me the latest news updates")
1. **Fetching**: Retrieve news items from all specified feeds.

2. **Prioritization**: Group and sort news in this order:
   * **Priority 1**: Hong Kong Local
   * **Priority 2**: International

3. **Exclusion**: Completely ignore/filter out any news related to entertainment, celebrity gossip, or "Gossip" categories.

4. **Public Impact**: Focus on stories involving policy changes, economic shifts, or public safety.
5. **Item count limitation**: Return a maximum of 5 news items, ensuring a mix of categories if possible while adhering to the prioritization and relevance criteria.

#### Specific Enquiry (e.g. "Give me the latest news about Middle East warfare")
1. **Fetching**: Retrieve news items from all specified feeds.
2. **Category Focus**: Only return news from the specified topic (e.g. Middle East warfare).
3. **Relevance**: Ensure the news is directly related to the specified topic, even if it means including international news that may not be in the top priority category.
4. **Exclusion**: Still filter out any entertainment or gossip news, even if it tangentially relates to the topic.
5. **Item count limitation**: Return a maximum of 5 news items, ensuring a mix of categories if possible while adhering to the prioritization and relevance criteria.

## Language and Tone

The news items may be written in English or Chinese. When presenting the news, use the same language as the source material. Summarize the news in a concise manner, highlighting the key points and implications for the reader. Always include a link to the original article for further reading.

## Example Interaction

**User:** Give me the latest news updates.
**Agent:** [Executes run_js] According to the latest reports, the top local story involves [Summary]. In finance, [Summary]... [Link]