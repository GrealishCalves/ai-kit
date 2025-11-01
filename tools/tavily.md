## Tavily - Real-Time Web Search & Content Extraction

### **What It Does**

- Real-time web search for current information and best practices
- Extract and process content from specific web pages
- Crawl multiple pages across websites for comprehensive research
- Map website structure to discover available content
- Search with advanced filters (date ranges, domains, topics)

### **Why Use It**

- **Up-to-Date Information**: Gets current data from the web, not outdated training data
- **Best Practices**: Find industry standards, recommended patterns, and implementation guides
- **Troubleshooting**: Search for error messages, debugging guides, and solutions from community
- **News & Updates**: Get latest announcements, breaking changes, and version releases
- **Community Knowledge**: Access blog posts, discussions, Stack Overflow answers, and real-world examples
- **Multiple Sources**: Cross-reference information from various authoritative sources
- **Content Extraction**: Pull full article content from specific URLs for detailed analysis

### **When to Use Tavily**

**Primary Use Cases:**

1. **Current Events**: Latest news, updates, announcements
2. **Best Practices**: Industry standards, recommended patterns
3. **Troubleshooting**: Error messages, debugging guides, solutions
4. **Tutorials**: Step-by-step guides, how-to articles
5. **Community Insights**: Blog posts, discussions, real-world examples
6. **Verification**: Cross-check information from multiple sources

**Examples:**

- "Latest React 19 breaking changes announcement"
- "Best practices for Next.js App Router 2024"
- "How to fix TypeScript type inference error"
- "Prisma migration best practices tutorial"

### **Available Tools**

#### 1. **tavily_search** - Web Search

Search the web for information with advanced filtering.

```typescript
tavily_search({
  query: "Next.js App Router best practices 2024",
  max_results: 5,
  search_depth: "basic", // or "advanced"
  topic: "general", // or "news", "finance"
  time_range: "month", // "day", "week", "month", "year"
  include_domains: ["nextjs.org", "vercel.com"],
  exclude_domains: ["spam-site.com"],
});
```

#### 2. **tavily_extract** - Extract Page Content

Extract clean content from specific URLs.

```typescript
tavily_extract({
  urls: ["https://react.dev/reference/react/useState", "https://nextjs.org/docs/app/building-your-application/routing"],
  format: "markdown", // or "text"
  extract_depth: "basic", // or "advanced"
});
```

#### 3. **tavily_crawl** - Crawl Website

Crawl multiple pages from a website for comprehensive research.

```typescript
tavily_crawl({
  url: "https://react.dev/reference/",
  max_depth: 2,
  limit: 50,
  instructions: "Find all pages about React hooks",
  select_paths: ["/reference/react/.*"],
  format: "markdown",
});
```

#### 4. **tavily_map** - Map Website Structure

Discover all URLs and pages on a website.

```typescript
tavily_map({
  url: "https://nextjs.org/docs/",
  max_depth: 2,
  limit: 100,
  instructions: "Find all App Router documentation pages",
});
```

### **Search Strategies**

#### Strategy 1: Quick Answer Lookup

```typescript
// Find quick answer to specific question
tavily_search({
  query: "how to use React Server Components with Next.js",
  max_results: 3,
  search_depth: "basic",
  time_range: "year",
});
```

#### Strategy 2: Deep Research

```typescript
// Comprehensive research on topic
tavily_search({
  query: "Next.js App Router data fetching patterns tutorial",
  max_results: 10,
  search_depth: "advanced",
  include_domains: ["nextjs.org", "vercel.com", "medium.com"],
});
```

#### Strategy 3: Recent Updates

```typescript
// Find latest news and updates
tavily_search({
  query: "React 19 breaking changes",
  topic: "news",
  time_range: "month",
  max_results: 5,
});
```

#### Strategy 4: Error Troubleshooting

```typescript
// Search for error solutions
tavily_search({
  query: "TypeError cannot read property of undefined React",
  max_results: 5,
  search_depth: "advanced",
  include_domains: ["stackoverflow.com", "github.com", "react.dev"],
});
```

### **Best Practices**

**DO:**

- ✓ Use specific, detailed queries for better results
- ✓ Filter by time_range for current information
- ✓ Include relevant domains for authoritative sources
- ✓ Use "advanced" search_depth for complex queries
- ✓ Extract full content from promising URLs
- ✓ Cross-reference multiple sources

**DON'T:**

- ✗ Don't use for official library docs (use Context7 instead)
- ✗ Don't trust single source - verify across multiple
- ✗ Don't ignore publication dates
- ✗ Don't skip domain filtering for critical info
- ✗ Don't use outdated results without checking dates

### **Integration with Evaluation**

When evaluating responses, use Tavily to:

1. **Verify Current Best Practices**: Check if recommendations align with industry standards
2. **Find Real-World Examples**: Discover how others solve similar problems
3. **Check for Updates**: Ensure information isn't outdated
4. **Troubleshoot Issues**: Find solutions to errors or problems
5. **Cross-Reference**: Validate claims against multiple sources

### **Common Search Patterns**

```bash
# Pattern 1: Official Documentation Search
tavily_search(query="library-name official documentation feature-name", include_domains=["official-site.com"])

# Pattern 2: Best Practices Research
tavily_search(query="best practices pattern-name 2024", search_depth="advanced", max_results=10)

# Pattern 3: Error Troubleshooting
tavily_search(query="error-message solution", include_domains=["stackoverflow.com", "github.com"])

# Pattern 4: Recent Updates
tavily_search(query="library-name updates changes", topic="news", time_range="month")

# Pattern 5: Tutorial Discovery
tavily_search(query="how to task-description tutorial", search_depth="advanced")
```

### **Advanced Features**

#### Date Filtering

```typescript
// Search within specific date range
tavily_search({
  query: "React 19 migration guide",
  start_date: "2024-01-01",
  end_date: "2024-12-31",
});

// Recent content only
tavily_search({
  query: "Next.js best practices",
  time_range: "month", // last 30 days
});
```

#### Domain Control

```typescript
// Include only trusted sources
tavily_search({
  query: "web security best practices",
  include_domains: ["owasp.org", "mozilla.org", "web.dev", "developer.chrome.com"],
});

// Exclude unreliable sources
tavily_search({
  query: "web development tutorial",
  exclude_domains: ["spam-site.com", "low-quality-blog.com"],
});
```

#### Content Extraction

```typescript
// Extract full article content
tavily_extract({
  urls: ["https://blog.example.com/react-server-components-guide"],
  format: "markdown",
  extract_depth: "advanced", // Gets tables, embedded content
  include_images: true,
});
```

#### Website Crawling

```typescript
// Crawl documentation site
tavily_crawl({
  url: "https://docs.example.com/",
  max_depth: 3,
  limit: 100,
  select_paths: ["/docs/.*", "/guides/.*"],
  exclude_paths: ["/blog/.*"],
  instructions: "Find all API reference pages",
});
```

### **Verification Workflow**

```bash
# Step 1: Search for information
tavily_search({query: "topic", max_results: 5})

# Step 2: Extract promising URLs
tavily_extract({urls: ["url1", "url2"]})

# Step 3: Cross-reference sources
# Compare information from multiple sources

# Step 4: Check publication dates
# Ensure information is current

# Step 5: Verify against official docs
# Use Context7 for official documentation

# Step 6: Document findings
# Record evidence with source URLs
```

### **Quality Assessment**

**High Quality Sources:**

- ✓ Official documentation sites
- ✓ Established technical blogs (Medium, Dev.to)
- ✓ GitHub repositories and discussions
- ✓ Stack Overflow accepted answers
- ✓ MDN Web Docs and W3C standards

**Medium Quality Sources:**

- ⚠ Personal blogs (verify credentials)
- ⚠ Tutorial sites (check date and accuracy)
- ⚠ Forum discussions (multiple confirmations needed)

**Low Quality Sources:**

- ✗ Content farms
- ✗ Outdated articles (>2 years old for fast-moving tech)
- ✗ Unverified claims
- ✗ Sites with poor reputation

### **Limitations**

- Search results depend on web content availability
- May return outdated information if not filtered by date
- Quality varies across sources - always verify
- Cannot access paywalled or restricted content
- May miss very recent updates (indexing delay)

### **Quick Reference**

**When to Use Tavily:**

- ✓ Current best practices and industry standards
- ✓ Troubleshooting and error solutions
- ✓ Tutorials and how-to guides
- ✓ News and recent updates
- ✓ Community insights and discussions
- ✓ Cross-referencing information

**When to Use Context7 Instead:**

- ✗ Official library documentation
- ✗ Exact API signatures
- ✗ Version-specific features
- ✗ Framework code examples
- ✗ Authoritative library guidance

### **Combining Tavily + Context7**

**Optimal Workflow:**

1. **Context7 First**: Check official docs for authoritative info
2. **Tavily Second**: Find best practices, tutorials, real-world examples
3. **Cross-Reference**: Verify Tavily findings against Context7 docs
4. **Document Both**: Cite official docs + community practices
