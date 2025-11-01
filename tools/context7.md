## Context7 - Official Documentation & Code Snippets Retrieval

### **What It Does**

- Fetches up-to-date official documentation for libraries and frameworks
- Retrieves code snippets and examples from official sources
- Provides library-specific best practices and patterns
- Returns documentation focused on specific topics/features
- Supports version-specific documentation lookup

### **Why Use It**

- **Always Current**: Gets latest official documentation, not outdated information from training data
- **Authoritative Source**: Direct from official library documentation, not third-party interpretations
- **Code Examples**: Real working code snippets and usage patterns from official sources
- **Version-Aware**: Can fetch docs for specific library versions to check compatibility
- **Topic-Focused**: Narrow down to specific features/topics you need instead of entire documentation
- **API Verification**: Confirm exact function signatures, parameters, and return types
- **Migration Support**: Access version-specific breaking changes and migration guides

### **When to Use Context7**

**Primary Use Cases:**

1. **Verifying API Signatures**: Check exact function/method signatures
2. **Framework Features**: Confirm if a feature exists in a specific version
3. **Best Practices**: Get official recommendations from library maintainers
4. **Code Examples**: Find official usage examples and patterns
5. **Migration Guides**: Check breaking changes between versions
6. **Configuration Options**: Verify available config parameters

**Examples:**

- "How does React's useEffect hook work in v18?"
- "What are the authentication parameters in NextAuth?"
- "How to configure Vite build options?"
- "What's the correct syntax for Prisma schema relations?"

### **Essential Usage Pattern**

```bash
# Step 1: Resolve library ID first
resolve-library-id("react")
# Returns: /facebook/react

# Step 2: Get documentation with optional topic focus
get-library-docs({
  context7CompatibleLibraryID: "/facebook/react",
  topic: "hooks useState useEffect",
  tokens: 5000
})

# For specific version:
get-library-docs({
  context7CompatibleLibraryID: "/facebook/react/v18.0.0",
  topic: "concurrent features",
  tokens: 3000
})
```

### **Common Library Examples**

```bash
# Frontend Frameworks
resolve-library-id("react")          # /facebook/react
resolve-library-id("next.js")        # /vercel/next.js
resolve-library-id("vue")            # /vuejs/core

# State Management & Data Fetching
resolve-library-id("tanstack-query") # /TanStack/query
resolve-library-id("redux")          # /reduxjs/redux
resolve-library-id("zustand")        # /pmndrs/zustand

# Testing & Development
resolve-library-id("vitest")         # /vitest-dev/vitest
resolve-library-id("playwright")     # /microsoft/playwright
resolve-library-id("jest")           # /jestjs/jest

# Build Tools & Bundlers
resolve-library-id("vite")           # /vitejs/vite
resolve-library-id("webpack")        # /webpack/webpack
resolve-library-id("esbuild")        # /evanw/esbuild

# Backend & Database
resolve-library-id("express")        # /expressjs/express
resolve-library-id("prisma")         # /prisma/prisma
resolve-library-id("drizzle")        # /drizzle-team/drizzle-orm
```

### **Best Practices**

**DO:**

- ✓ Always resolve library ID first before fetching docs
- ✓ Use specific topics to narrow down results
- ✓ Specify version when checking version-specific features
- ✓ Use for official API verification
- ✓ Cross-reference with actual codebase after getting docs

**DON'T:**

- ✗ Don't assume library names - always resolve first
- ✗ Don't skip version checking for breaking changes
- ✗ Don't trust memory - always fetch current docs
- ✗ Don't use for general web searches (use Tavily instead)

### **Verification Workflow**

```bash
# 1. Check if feature exists in library
resolve-library-id("react")
get-library-docs({
  context7CompatibleLibraryID: "/facebook/react",
  topic: "useTransition concurrent rendering"
})

# 2. Verify exact API signature
get-library-docs({
  context7CompatibleLibraryID: "/facebook/react",
  topic: "useState hook parameters return"
})

# 3. Check version compatibility
get-library-docs({
  context7CompatibleLibraryID: "/facebook/react/v17.0.0",
  topic: "breaking changes migration v18"
})

# 4. Get code examples
get-library-docs({
  context7CompatibleLibraryID: "/facebook/react",
  topic: "useEffect cleanup example usage"
})
```

### **Integration with Evaluation**

When evaluating responses, use Context7 to:

1. **Verify Claims**: Check if stated features actually exist
2. **Confirm Syntax**: Validate code examples against official docs
3. **Check Versions**: Ensure version-specific details are accurate
4. **Find Alternatives**: Discover official recommended approaches
5. **Validate Best Practices**: Confirm recommendations align with official guidance

### **Common Patterns**

```bash
# Pattern 1: Quick API Check
resolve-library-id("library-name") → get-library-docs(topic="specific-feature")

# Pattern 2: Version Migration
get-library-docs(v1.0.0, topic="feature") → get-library-docs(v2.0.0, topic="feature")

# Pattern 3: Best Practice Lookup
get-library-docs(topic="best practices recommended patterns")

# Pattern 4: Code Example Retrieval
get-library-docs(topic="example usage tutorial getting-started")
```

### **Limitations**

- Only works for libraries with official documentation
- Requires exact library ID (must resolve first)
- Token limit affects amount of documentation returned
- May not have docs for very new or niche libraries
- Does not search general web content (use Tavily for that)

### **Quick Reference**

**When to Use Context7:**

- ✓ Official library documentation
- ✓ API signatures and parameters
- ✓ Code examples from official sources
- ✓ Version-specific features
- ✓ Framework best practices

**When to Use Tavily Instead:**

- ✗ General web searches
- ✗ Blog posts and tutorials
- ✗ Community discussions
- ✗ News and updates
- ✗ Troubleshooting guides
