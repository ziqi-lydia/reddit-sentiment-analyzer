<div align="center">
  <img src="assets/cover.png" alt="Reddit Sentiment Analyzer -- Automated Reddit Opinion Mining powered by Sai" width="800" />

  <h1>Reddit Sentiment Analyzer</h1>

  <p><strong>Turn Reddit's unfiltered user opinions into structured market intelligence -- automatically.</strong></p>

  <p>
    <a href="https://www.sai.work/">Powered by Sai</a> --
    <a href="https://www.simular.ai/">Built by Simular</a>
  </p>
</div>

---

## What It Does

This is a [Sai skill](https://www.sai.work/) that automates Reddit sentiment analysis from end to end. Give it a product, brand, or topic, and it will:

1. **Discover relevant subreddits** via Google search -- no need to know which communities to monitor
2. **Scrape posts and comments** using real browser sessions (no Reddit API key required)
3. **Classify sentiment** for each post: positive, negative, or neutral
4. **Extract recurring themes** -- common complaints, feature requests, praise patterns
5. **Generate a structured report** with quantified insights and direct links to every source thread
6. **Export to Google Sheets** (optional) -- with Summary, Raw Posts, and Theme Analysis tabs

## Why Not Just Use the Reddit API?

Since Reddit's [2023 API pricing changes](https://www.redditinc.com/blog/apifacts), third-party access has become expensive and rate-limited. Most sentiment tools either:
- Charge per-request fees on top of their own subscription
- Hit rate limits that cap analysis at a few hundred posts
- Require you to manage OAuth tokens and API keys

**Sai uses a real browser session.** It reads any public subreddit the same way you would -- scrolling through posts, opening comment threads, following cross-posts. No API key needed, no per-request costs, no rate limits beyond natural browsing speed.

## Use Cases

| Prompt | What Sai Does |
|--------|--------------|
| "What does Reddit think about Notion?" | Discovers r/Notion, r/productivity, r/selfhosted; scrapes top posts; classifies sentiment; identifies top praise and complaint themes |
| "Find common complaints about Slack on Reddit" | Focuses on negative sentiment; extracts recurring pain points with source links |
| "Compare Reddit sentiment: Figma vs Sketch" | Runs parallel analysis on both tools; generates side-by-side comparison with theme overlap |
| "Monitor Reddit mentions of our brand from the last 30 days" | Time-scoped search; tracks sentiment trajectory over the window |
| "What features do Reddit users wish project management tools had?" | Extracts feature request patterns across r/projectmanagement, r/agile, r/jira, etc. |

## How It Works

```
User prompt
    |
    v
Phase 1: Discover subreddits (Google search + Reddit browsing)
    |
    v
Phase 2: Scrape posts & comments (real browser, randomized delays)
    |
    v
Phase 3: Classify sentiment (positive / negative / neutral per post)
    |
    v
Phase 4: Extract themes (recurring topics, complaint patterns, feature requests)
    |
    v
Phase 5: Generate report (structured summary with source URLs)
    |
    v
Phase 6: Export to Google Sheets (optional -- Summary, Raw Posts, Theme Analysis tabs)
```

### Safety & Anti-Ban Measures

- **Randomized delays** (2-5 seconds) between page loads to mimic natural browsing
- **Max 40 Reddit pages per session** to avoid triggering rate limits
- **Read-only** -- never votes, comments, or posts on your behalf
- **Respects private subreddits** -- skips if access is denied
- **CAPTCHA detection** -- stops immediately and reports to user if encountered
- **Old Reddit fallback** -- switches to old.reddit.com if the new UI is slow or blocked

## Output Format

### In-Chat Summary

Every analysis includes a structured summary:

- **Sentiment breakdown**: count and percentage of positive, negative, and neutral posts
- **Top themes**: recurring topics ranked by frequency, with representative quotes
- **Actionable insights**: specific recommendations based on sentiment patterns
- **Source links**: every claim backed by a direct URL to the Reddit post or comment

### Google Sheets Export (Optional)

| Tab | Contents |
|-----|----------|
| **Summary** | Target, time window, total posts, sentiment breakdown chart data |
| **Raw Posts** | Every scraped post with: title, subreddit, score, sentiment tag, URL |
| **Theme Analysis** | Theme name, frequency count, sentiment skew, representative quotes |

## Getting Started

### Prerequisites

- [Sai](https://www.sai.work/) installed on your computer (macOS or Windows)
- A Google account connected to Sai (only needed for Google Sheets export)
- No Reddit account or API key required

### Quick Start

1. Open Sai
2. Type a prompt like: *"Analyze Reddit sentiment for Notion over the last 30 days"*
3. Sai discovers relevant subreddits, scrapes posts, classifies sentiment, and delivers a structured report
4. Optionally ask Sai to export the results to a Google Sheet

### Configuration Options

When starting an analysis, you can specify:

| Parameter | Description | Default |
|-----------|-------------|---------|
| **Target** | Product name, brand, or topic | Required |
| **Subreddits** | Specific subreddits to focus on | Auto-discovered |
| **Time window** | Last 7 / 30 / 90 days, or all time | Last 30 days |
| **Focus** | General, complaints, feature requests, or comparisons | General |
| **Competitor** | Compare against a competitor | None |
| **Output** | Chat summary, Google Sheet, or both | Chat summary |

## Data Quality Guarantees

- Every sentiment claim is backed by a specific post or comment URL
- Theme percentages are based on actual counted data, not estimates
- No fabricated quotes -- only paraphrased or directly attributed text
- Time window and subreddits analyzed are clearly documented in every report

## File Structure

```
reddit-sentiment-analyzer/
  SKILL.md        # Sai skill definition (full workflow instructions)
  README.md       # This file
  assets/
    cover.png     # Repository cover image
```

## Related Skills

Other Sai skills that pair well with Reddit Sentiment Analyzer:

- [**cross-platform-trend-scout**](https://github.com/ziqi-lydia/cross-platform-trend-scout) -- Research trends across X, LinkedIn, TikTok, YouTube, and Reddit simultaneously
- [**ai-market-research**](https://github.com/ziqi-lydia/ai-market-research) -- Comprehensive market research with competitor analysis and SWOT reports
- [**lead-enrichment-engine**](https://github.com/ziqi-lydia/lead-enrichment-engine) -- Research companies and find decision-makers from multiple data sources

## License

MIT

---

<div align="center">
  <p>Built with <a href="https://www.sai.work/">Sai</a> by <a href="https://www.simular.ai/">Simular</a></p>
</div>
