---
name: reddit-sentiment-analyzer
description: Analyze Reddit sentiment for any product, brand, or topic. Scrapes subreddit posts and comments, classifies sentiment, extracts recurring themes, and generates an actionable report with direct links to source threads.
---

# Reddit Sentiment Analyzer

Turn Reddit's unfiltered user opinions into structured market intelligence. Sai browses target subreddits, reads posts and top comments, classifies sentiment (positive/negative/neutral), identifies recurring themes, and compiles a report with quantified insights and direct source links.

**Sai's unfair advantage:** Most sentiment tools use Reddit's API (which has strict rate limits and costs money since the 2023 API changes). Sai uses a real browser session — it can read any public subreddit, see full comment threads, and follow cross-posts. No API key needed, no per-request costs.

## When to Use
- "What does Reddit think about [product/brand]?"
- "Analyze sentiment in r/[subreddit] about [topic]"
- "Find common complaints about [competitor] on Reddit"
- "What features do Reddit users wish [product category] had?"
- "Monitor Reddit mentions of [brand] from the last 30 days"
- "Compare Reddit sentiment: [product A] vs [product B]"

## Required Context (ask if not provided)
- **Target**: Product name, brand, or topic to analyze
- **Subreddits**: Specific subreddits or let Sai discover relevant ones
- **Time window**: Last 7 days, 30 days, 90 days, or all time
- **Focus**: General sentiment, feature requests, complaints, comparisons, or all?
- **Competitor comparison**: Any competitors to compare against?
- **Output**: In-chat summary, Google Sheet with raw data, Google Doc report, or all?

## Workflow

### Phase 1: Discover Relevant Subreddits

```javascript
// Search Google for relevant subreddits
var discoveryPage = await browser.newtab("https://www.google.com/search?q=" + encodeURIComponent("[product] site:reddit.com"))
await discoveryPage.wait({ waitTime: 2 })
var discSnap = await discoveryPage.snapshot()

// Extract subreddit names from results
var subreddits = [] // e.g., ["r/product", "r/industry", "r/reviews"]

// Also try Reddit's own search
var redditSearch = await browser.newtab("https://www.reddit.com/search/?q=" + encodeURIComponent("[product]") + "&type=link&sort=relevance&t=month")
await redditSearch.wait({ waitTime: 3 })
```

### Phase 2: Scrape Posts & Comments

```javascript
// For each subreddit, search for the topic
var allPosts = []

for (var sub of subreddits) {
  var subPage = await browser.newtab("https://www.reddit.com/" + sub + "/search/?q=" + encodeURIComponent("[topic]") + "&restrict_sr=1&sort=relevance&t=month")
  await subPage.wait({ waitTime: 3 + Math.random() * 3 })
  var subSnap = await subPage.snapshot()
  
  // Extract post titles, scores, comment counts, dates
  // Click into top 5-10 posts to read comments
  
  // For each post, collect:
  var post = {
    title: "",
    subreddit: sub,
    score: 0,
    commentCount: 0,
    date: "",
    url: "",
    topComments: [], // top 5-10 comments with scores
    sentiment: "",   // will be classified
    themes: []       // will be extracted
  }
  
  allPosts.push(post)
  await wait({ waitTime: 2 + Math.random() * 3 }) // rate limiting
}
```

### Phase 3: Sentiment Classification

```javascript
// Classify each post and comment
var sentimentResults = {
  positive: [],
  negative: [],
  neutral: [],
  mixed: []
}

for (var post of allPosts) {
  // Analyze post title + top comments to determine sentiment
  // Look for signal words:
  // Positive: love, amazing, best, recommend, switched to, game changer
  // Negative: hate, terrible, avoid, switched from, waste, broken
  // Neutral: question, how to, comparison, alternative
  
  // Classify and tag
  post.sentiment = "positive" // or negative/neutral/mixed
  post.themes = [] // "pricing", "features", "support", "UX", etc.
  
  sentimentResults[post.sentiment].push(post)
}
```

### Phase 4: Theme Extraction & Quantification

```javascript
// Group by recurring themes
var themes = {}
for (var post of allPosts) {
  for (var theme of post.themes) {
    if (!themes[theme]) themes[theme] = { positive: 0, negative: 0, neutral: 0, posts: [] }
    themes[theme][post.sentiment]++
    themes[theme].posts.push(post.url)
  }
}

// Sort themes by frequency
var sortedThemes = Object.entries(themes)
  .sort((a, b) => (b[1].positive + b[1].negative + b[1].neutral) - (a[1].positive + a[1].negative + a[1].neutral))
```

### Phase 5: Generate Report

```javascript
// Compile findings
var report = {
  summary: {
    totalPosts: allPosts.length,
    sentimentBreakdown: {
      positive: sentimentResults.positive.length,
      negative: sentimentResults.negative.length,
      neutral: sentimentResults.neutral.length,
      mixed: sentimentResults.mixed.length
    },
    overallSentiment: "", // "Mostly Positive" / "Mixed" / etc.
    topPositiveTheme: "",
    topNegativeTheme: ""
  },
  themes: sortedThemes,
  topPositivePosts: sentimentResults.positive.slice(0, 5),
  topNegativePosts: sentimentResults.negative.slice(0, 5),
  actionableInsights: [],
  subredditsAnalyzed: subreddits,
  timeWindow: "Last 30 days",
  sourcesCount: allPosts.length
}
```

### Phase 6: Output to Google Sheet (Optional)

```javascript
var sheet = await google.sheets.createSpreadsheet({
  title: "Reddit Sentiment: [Product] - " + new Date().toLocaleDateString(),
  sheets: ["Summary", "Raw Posts", "Theme Analysis"]
})

// Summary tab
await google.sheets.updateValues({
  spreadsheetId: sheet.id,
  range: "Summary!A1",
  values: [
    ["Reddit Sentiment Analysis Report"],
    ["Target", "[product]"],
    ["Time Window", "Last 30 days"],
    ["Total Posts Analyzed", allPosts.length.toString()],
    [""],
    ["Sentiment", "Count", "Percentage"],
    ["Positive", report.summary.sentimentBreakdown.positive.toString(), ""],
    ["Negative", report.summary.sentimentBreakdown.negative.toString(), ""],
    ["Neutral", report.summary.sentimentBreakdown.neutral.toString(), ""]
  ]
})

// Raw Posts tab - all posts with sentiment tags and URLs
// Theme Analysis tab - theme breakdown with evidence
```

## Safety & Anti-Ban Rules
- **Random delays**: Wait 2-5 seconds between page loads (randomized)
- **Max pages per session**: 40 Reddit pages
- **Never interact**: Read only — no voting, commenting, or posting
- **Respect private subreddits**: Skip if access denied
- **If CAPTCHA appears**: STOP immediately, report to user
- **Old Reddit fallback**: If new Reddit is slow/blocked, try old.reddit.com

## Output Checklist
- [ ] Every sentiment claim backed by specific post/comment URL
- [ ] Theme percentages based on actual counted data
- [ ] No fabricated quotes — only paraphrase or direct attribution
- [ ] Actionable insights tied to evidence
- [ ] Time window and subreddits clearly documented

