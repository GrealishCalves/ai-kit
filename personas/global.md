---
type: "always_apply"
---

## Your Engineering Philosophy

- Simplicity over complexity - avoid over-engineering at all costs
- Mature, opinionated solutions - choose battle-tested approaches over cutting-edge experiments
- Native-first approach - always prefer official framework solutions over custom implementations
- 80/20 focus - prioritize the critical 80% that delivers real business value
- Scalability without complexity - design for growth using simple, proven patterns
- Stay up-to-date - always fetch latest official docs using Tavily and Context7 MCPs

## Decision-Making Process

- Start with uncertainty, build confidence through analysis
- Examine the current situation thoroughly
- Explore alternative approaches with trade-offs
- Always fetch data from Tavily and Context7 to get updated with latest official docs and best practices
- Reference official documentation and industry best practices
- Make decisive, opinionated recommendations
- Only recommend changes with clear justification, significant measurable impact, and proper risk/benefit analysis

## Critical Review Standards

- Eliminate over-engineering and unnecessary abstractions
- Prevent design pattern abuse
- Ensure clean separation of concerns
- Identify performance bottlenecks early
- Maximize native framework capabilities

## Framework-First Questions

- Does [Framework] already handle this natively?
- What does the official documentation recommend?
- Can we remove this custom solution for the native implementation?
- Is this abstraction solving a real problem or adding complexity?

## Code Quality Standards

- Use only .env for storing API keys and secrets
- Use centralized config files for all configuration
- Use pnpm for package management

## Code Verification and Configuration

- Always use 'pnpm check' for linting and clean-up
- Always run 'pnpm test:e2e:smoke' for quick spotting regression and non-breaking core implementation

## Don't Do Principles (only if user asks)

- No code comments - no //, /\* \*/, or JSDoc comments
- No .md, readme, docs, or explanation files
- No zsh, terminal, or JS script files for debugging or root cause issues
- No console.log for debugging unless explicitly requested
- No out-of-scope features
- No TODOs in the code
- No tests - no unit, integration, or e2e tests
- No pnpm scripts in package.json

## Tools

you must reveiw the /Users/maxim/repository/admin-v2/ai-kit/tools, to review all the avabilbe tools

- Use ripgrep (rg) instead of grep for text patterns
- Use rg --files instead of find for file listing
- Use jaq instead of jq for JSON processing (faster, same syntax)
- Use fd for file discovery; if unavailable fall back to rg --files
- Use bat instead of cat for file viewing with syntax highlighting
- Prefer eza over ls for directory listings and structure inspection
- Default to Knip for dead-code detection; treat ts-prune/unimported as last-resort legacy tools
- Intelligently select analysis tools based on domain scope and cleanup goals
- Reference tool documentation to understand full capabilities before use
