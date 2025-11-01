---
type: "agent_requested"
description: "Example description"
---

# Universal Trivia Explanation Formatting Playbook

## Philosophy

**Formatting is not decoration—it's a tool for learning.**

Proper formatting creates visual hierarchy, reduces cognitive load, and guides the reader's attention to what matters most. This playbook establishes universal principles that apply to any technical domain, any programming language, and any level of complexity.

**Core Belief:** Consistency enables learning. When every explanation follows the same structure, readers can focus on understanding the content instead of decoding the presentation.

---

## Target Quality Metrics

| Metric                   | Target | Measurement                                              |
| ------------------------ | ------ | -------------------------------------------------------- |
| **Consistency**          | 95%+   | All explanations follow the same structural pattern      |
| **Markdown Utilization** | 95%+   | Appropriate use of code blocks, inline code, bold, lists |
| **Visual Hierarchy**     | 90%+   | Clear section breaks, scannable structure, logical flow  |
| **Context & Clarity**    | 95%+   | Sufficient detail, concrete examples, clear reasoning    |
| **Overall Quality**      | 95%+   | Weighted average of all metrics                          |

---

## Part 1: Core Formatting Principles

### Principle 1: Predictable Structure Reduces Cognitive Load

**Why it matters:** When every explanation follows the same structure, readers develop pattern recognition. They know where to look for specific types of information, reducing mental effort and accelerating comprehension.

**Universal Structure Pattern:**

````markdown
[OPENING STATEMENT - plain text with inline code for technical terms]

**[SECTION HEADING]**

```language
[CODE BLOCK - 3-5 lines max, always with language tag]
```
````

**[WHY SECTION HEADING]**

- [Bullet point 1 with inline code for technical terms]
- [Bullet point 2 with specific examples or numbers]
- [Bullet point 3 with consequences or implications]
- [Bullet point 4 with edge cases or protections]

`````

**Reasoning behind each component:**

| Component             | Purpose                     | Cognitive Function                  |
| --------------------- | --------------------------- | ----------------------------------- |
| **Opening statement** | Provides immediate context  | Answers "what is this?"             |
| **Section heading**   | Signals code section        | Prepares mind for technical details |
| **Code block**        | Shows actual implementation | Provides visual proof, builds trust |
| **Why heading**       | Signals transition to "why" | Prepares mind for deeper analysis   |
| **Bullet list**       | Breaks down complexity      | Reduces overwhelm, enables scanning |

---

### Principle 2: Inline Code Creates Visual Anchors

**Why it matters:** Technical terms wrapped in inline code stand out visually, making them easier to scan and remember. It signals "this is a precise identifier, not casual language."

**Universal Rule:** Wrap ALL technical identifiers in backticks.

**Formatting Rules by Content Type:**

| Content Type           | Format      | Reasoning                   | Generic Example                        | Naming Convention         |
| ---------------------- | ----------- | --------------------------- | -------------------------------------- | ------------------------- |
| **Constants**          | Inline code | Distinguishes from prose    | `MAX_RETRIES`, `DEFAULT_TIMEOUT`       | SCREAMING_SNAKE_CASE      |
| **Function names**     | Inline code | Signals executable code     | `validateInput()`, `processData()`     | camelCase                 |
| **Variable names**     | Inline code | Identifies data containers  | `userId`, `totalAmount`, `isValid`     | camelCase                 |
| **Numeric values**     | Inline code | Highlights specific numbers | `100`, `5%`, `3.14`                    | Plain numbers             |
| **Type/class names**   | Inline code | Identifies data structures  | `UserAccount`, `Transaction`, `Config` | PascalCase                |
| **Status/enum values** | Inline code | Marks enumeration values    | `ACTIVE`, `PENDING`, `COMPLETED`       | SCREAMING_SNAKE_CASE      |
| **File/path names**    | Inline code | Identifies resources        | `config.json`, `/api/users`            | kebab-case or snake_case  |
| **Solidity contracts** | Inline code | Contract names              | `ProductionLottery`, `ReentrancyGuard` | PascalCase                |
| **Solidity functions** | Inline code | Function signatures         | `createLottery()`, `buyTickets()`      | camelCase                 |
| **Section headings**   | Bold only   | Creates clear breaks        | **Why This Value?**, **How It Works**  | Title Case, no colon      |

**Critical Anti-Patterns to Avoid:**

❌ **Mixing inline code with bold:** `**`MAX_VALUE`**` (breaks react-markdown rendering)
❌ **Bold mid-sentence:** The value is **5%** (renders as block element, breaks flow)
❌ **Inconsistent wrapping:** Sometimes `value`, sometimes value (breaks pattern recognition)
❌ **Over-wrapping:** Wrapping entire sentences in code blocks (loses meaning)
❌ **Missing language tag:** \`\`\` without language (no syntax highlighting)

**React-Markdown Rendering Rules:**

✅ **Inline bold ONLY for section headers on their own line**
✅ **Plain text for emphasis mid-sentence** (no bold)
✅ **Inline code for all technical terms** (variables, functions, values)
✅ **Code blocks ALWAYS have language tag** (\`\`\`solidity, \`\`\`javascript, etc.)

---

### Principle 3: Code Blocks Provide Visual Proof

**Why it matters:** Showing actual code builds trust and allows readers to verify the explanation against implementation. It transforms abstract concepts into concrete reality.

**Universal Code Block Rules:**

1. **ALWAYS specify the language tag** (enables syntax highlighting, critical for react-markdown)
2. **Keep code blocks SHORT** (3-5 lines ideal, 10 lines maximum)
3. **Remove unnecessary context** (show only relevant portions)
4. **Use consistent indentation** (matches actual codebase style)
5. **Prefer real code over pseudocode** (builds trust through authenticity)
6. **NO inline comments** (keep code clean, explain in bullet points instead)

**Solidity Example (optimal pattern):**

````markdown
**Contract Constants**

```solidity
uint256 public constant TICKET_FEE_PERCENT = 500;
uint256 public constant BASIS_POINTS = 10000;
```
````

**JavaScript Example (optimal pattern):**

````markdown
**Validation Function**

```javascript
function validateInput(data) {
  return data !== null && data.length > 0;
}
```
````

**React-Markdown Rendering:**

- Language tag triggers syntax highlighting (green text for keywords)
- Code blocks render with dark background (`bg-black/50`)
- Proper spacing before/after (`my-6`)
- Monospace font (`font-mono`)

**Critical Rules:**

✅ **Always use language tag:** \`\`\`solidity, \`\`\`javascript, \`\`\`typescript
✅ **Keep blocks 3-5 lines** (10 max)
✅ **Blank line before and after** code block
❌ **Never use \`\`\` without language** (no syntax highlighting)
❌ **Never exceed 10 lines** (loses focus, hard to scan)

---

### Principle 4: Bold Text Creates Section Headers ONLY

**Why it matters:** In react-markdown, bold text can render as either inline or block elements depending on context. To ensure consistent rendering, use bold ONLY for section headers on their own line.

**Critical React-Markdown Rule:**

✅ **Bold ONLY for section headers on their own line**
❌ **NEVER use bold mid-sentence** (renders as block element, breaks paragraph flow)

**Correct Usage:**

| Use Case            | Example                  | Rendering                                  |
| ------------------- | ------------------------ | ------------------------------------------ |
| **Section headers** | `**Why This Value?**`    | Block element, uppercase, gray, mt-8 mb-4  |
| **Standalone line** | `**Contract Constants**` | Block element, uppercase, gray, mt-8 mb-4  |

**Incorrect Usage (Breaks Rendering):**

| Anti-Pattern                | Problem                                  | Solution                      |
| --------------------------- | ---------------------------------------- | ----------------------------- |
| The value is **5%**         | Renders as block, breaks paragraph flow  | Use plain text: "The value is 5%" |
| Calculated as **`500`**     | Mixes bold + code, breaks rendering      | Use inline code only: "Calculated as `500`" |
| **3 validation steps**      | Mid-sentence bold renders as block       | Use plain text: "3 validation steps" |
| **Production** vs **Local** | Multiple bolds break flow                | Use plain text: "Production vs Local" |

**React-Markdown Rendering Behavior:**

```tsx
// Section header (correct - on own line)
**Why This Value?**
→ Renders as: <strong className="block mt-8 mb-4 uppercase">Why This Value?</strong>

// Mid-sentence bold (incorrect - breaks flow)
The value is **5%**.
→ Renders as: <p>The value is <strong className="block mt-8 mb-4">5%</strong>.</p>
// ❌ Block element inside paragraph breaks layout!
```

**Emphasis Alternatives:**

- Use plain text for emphasis mid-sentence
- Use inline code for technical terms: `value`
- Use section headers for major breaks: `**Section**`

---

### Principle 5: Bullet Lists Enable Scanning

**Why it matters:** Readers scan before they read. Bullet lists make information scannable, allowing readers to quickly grasp structure before diving into details.

**Universal Bullet List Rules:**

**Use bullet lists when:**

- Presenting 3-7 related items (cognitive load limit)
- Explaining multiple implications or consequences
- Breaking down complex concepts into steps
- Listing features, characteristics, or requirements

**Bullet list best practices:**

| Practice                      | Reasoning                         | Example                                          |
| ----------------------------- | --------------------------------- | ------------------------------------------------ |
| **Use `-` for bullets**       | Consistent markdown syntax        | Always `-`, never `*` or `+`                     |
| **Parallel structure**        | Reduces cognitive load            | All start with verbs OR all start with nouns     |
| **Consistent capitalization** | Maintains professionalism         | Always start with capital letter                 |
| **No punctuation**            | Cleaner, more scannable           | No periods at end of list items                  |
| **Optimal length**            | Balances detail with scannability | 3-7 bullets per section (never exceed 7)         |
| **Single level**              | Avoids complexity                 | Avoid nested bullets unless absolutely necessary |
| **1-2 lines max per item**    | Maintains scannability            | Keep items concise, break long items into two    |

**React-Markdown Rendering:**

```tsx
// List component renders with flexbox
li: ({ children }) => (
  <li className="flex items-start gap-3">
    <span className="text-green-500 mt-[2px] flex-shrink-0">•</span>
    <span className="flex-1">{children}</span>
  </li>
)
```

✅ **Good (parallel structure, scannable, 1 line each):**

```markdown
**Why 5%?**

- Covers infrastructure costs (node hosting, VRF fees, monitoring)
- Competitive with industry standards (most platforms charge 3-10%)
- Low enough to attract providers (95% revenue retention)
- Sustainable for long-term operations
```

❌ **Bad (inconsistent structure, too long, hard to scan):**

```markdown
**Why 5%?**

- Infrastructure costs need to be covered, including blockchain node hosting, VRF fees, monitoring systems, and security audits
- Data accuracy
- You can see that most lottery and gaming platforms charge between 3-10%
- For long-term sustainability, we need this fee
```

**Spacing Rules:**

- Blank line before list
- Blank line after list
- NO blank lines between list items
- Use `space-y-3` for vertical spacing between items

---

### Principle 6: Visual Hierarchy Guides Attention

**Why it matters:** The human eye follows predictable patterns. Proper spacing and hierarchy guide the reader through the content in the intended order.

**Universal Spacing Rules:**

```markdown
[Opening statement]
← 1 blank line
[Code block]
← 1 blank line
**[Section heading]:**

- [Bullet 1]
- [Bullet 2] ← No blank lines between bullets
- [Bullet 3]
  ← 1 blank line before next section
  **[Next section heading]:**
```

**Reasoning:**

- **Blank lines between sections:** Creates visual breathing room, signals topic change
- **No blank lines within lists:** Maintains visual cohesion, signals related items
- **Consistent spacing:** Builds pattern recognition, reduces cognitive load

**Anti-patterns:**

❌ **Random spacing:** Breaks pattern recognition
❌ **No spacing:** Creates visual wall of text
❌ **Excessive spacing:** Fragments content, loses cohesion

---

### Principle 7: React-Markdown Rendering Optimization

**Why it matters:** React-markdown has specific rendering behaviors that differ from standard markdown. Understanding these ensures content renders correctly with proper visual hierarchy.

**Component Rendering Behavior:**

| Element  | Inline/Block | Styling                                                  | Usage                          |
| -------- | ------------ | -------------------------------------------------------- | ------------------------------ |
| `strong` | Context-dependent | Inline: `text-green-400`, Block: `block mt-8 mb-4 uppercase` | Section headers ONLY           |
| `code`   | Inline       | `bg-green-500/10 px-2 py-0.5 rounded text-sm font-mono` | Technical terms, values        |
| `pre`    | Block        | `bg-black/50 p-4 rounded-lg my-6 overflow-x-auto`       | Code blocks with language tag  |
| `p`      | Block        | `text-base mb-5 leading-[1.7]`                           | Paragraphs                     |
| `ul`     | Block        | `space-y-3 my-5 ml-0 list-none`                          | Bullet lists                   |
| `li`     | Flex         | `flex items-start gap-3`                                 | List items with green bullets  |

**Critical Rendering Rules:**

1. **Strong Element Context Detection**
   - Uses AST node position to detect inline vs block context
   - Inline (mid-sentence): renders as `<strong className="text-green-400">`
   - Block (own line): renders as `<strong className="block mt-8 mb-4 uppercase">`
   - **NEVER use bold mid-sentence** (will render as block, breaking flow)

2. **Code Element Distinction**
   - Inline code: no `language-` class, renders with green background
   - Code block: has `language-` class, renders with dark background
   - **ALWAYS specify language tag** for code blocks

3. **List Item Flexbox Layout**
   - Bullet: `flex-shrink-0` prevents shrinking
   - Content: `flex-1` allows wrapping
   - Gap: `gap-3` (12px) for proper spacing
   - Alignment: `items-start` aligns bullet with first line

**Spacing Scale (Tailwind):**

```tsx
{
  gap2: "8px",   // Tight spacing (icon + text)
  gap3: "12px",  // List items, bullet + content
  mb4: "16px",   // Paragraphs
  my5: "20px",   // Lists, code blocks
  my6: "24px",   // Code blocks
  mt8: "32px",   // Section headers
}
```

**Typography Scale:**

```tsx
{
  sectionHeader: "text-xs uppercase tracking-wider text-muted-foreground/70",
  paragraph: "text-base leading-[1.7] text-muted-foreground",
  listItem: "text-base leading-[1.7] text-muted-foreground",
  inlineCode: "text-sm font-mono text-green-400",
  codeBlock: "text-sm font-mono leading-[1.8]",
}
```

**Color Hierarchy:**

```tsx
{
  primary: "text-foreground",           // Main content (unused in explanations)
  secondary: "text-muted-foreground",   // Body text, paragraphs, lists
  tertiary: "text-muted-foreground/70", // Section headers (70% opacity)
  accent: "text-green-400",             // Code, inline emphasis
  codeBg: "bg-green-500/10",            // Inline code background (10% opacity)
  codeBlockBg: "bg-black/50",           // Code block background (50% opacity)
  bullet: "text-green-500",             // List bullets
}
```

**Visual Hierarchy Checklist:**

- [ ] Section headers render as block elements (uppercase, gray, mt-8 mb-4)
- [ ] Inline code has green background and text
- [ ] Code blocks have dark background with syntax highlighting
- [ ] Lists have green bullets aligned with first line
- [ ] Paragraphs have proper line height (1.7) and bottom margin (mb-5)
- [ ] Spacing creates clear visual breaks between sections

---

## Part 2: Section Heading Patterns

### Principle 8: Headings Signal Content Type

**Why it matters:** The heading pattern should immediately signal what type of information follows. This allows readers to quickly navigate to the information they need.

**Universal Heading Patterns (NO COLONS):**

| Pattern                   | Use Case                      | Example                                    | When to Use                        |
| ------------------------- | ----------------------------- | ------------------------------------------ | ---------------------------------- |
| **Why [specific value]?** | Explaining a numeric constant | **Why 100?**, **Why 30 Seconds?**          | When the value itself is the focus |
| **Why This [concept]?**   | Explaining a design choice    | **Why This Approach?**, **Why Immutable?** | When the concept is the focus      |
| **Why The Difference?**   | Comparing two approaches      | **Why The Difference?**                    | When contrasting two options       |
| **How It Works**          | Explaining a mechanism        | **How It Works**                           | When describing a process          |
| **Purpose**               | Stating the goal              | **Purpose**                                | When explaining intent             |
| **What Each [X] Means**   | Defining multiple items       | **What Each Status Means**                 | When explaining enumeration values |
| **[Feature] Provided**    | Listing capabilities          | **Security Features Provided**             | When listing benefits/features     |

**Naming Convention Rules:**

- **Title Case** for all section headers (capitalize major words)
- **NO colons** at end of headers (cleaner, more scannable)
- **Question mark** for "Why" and "What" headers
- **No punctuation** for statement headers

**Examples:**

✅ **Good (Title Case, no colon):**
```markdown
**Why This Value?**
**Contract Constants**
**How It Works**
**What Each Status Means**
```

❌ **Bad (lowercase, has colon):**
```markdown
**Why this value:**
**contract constants:**
**How it works:**
**What each status means:**
```

**Consistency principle:** Use the same heading pattern for the same type of content across all explanations.

---

## Part 3: Naming Convention Standards

### Principle 9: Consistent Naming Across Questions, Answers, and Explanations

**Why it matters:** Consistent naming conventions across questions, answer options, and explanations reduce cognitive load and prevent confusion. Technical terms should appear identically everywhere.

**Naming Convention Reference:**

| Type                    | Convention           | Example                                  | Usage Context                    |
| ----------------------- | -------------------- | ---------------------------------------- | -------------------------------- |
| **Solidity Contracts**  | PascalCase           | `ProductionLottery`, `ReentrancyGuard`   | Contract names, type names       |
| **Solidity Functions**  | camelCase            | `createLottery()`, `buyTickets()`        | Function names, method calls     |
| **Solidity Variables**  | camelCase            | `lotteryId`, `ticketCount`, `isActive`   | State variables, parameters      |
| **Solidity Constants**  | SCREAMING_SNAKE_CASE | `MAX_TICKETS`, `TICKET_FEE_PERCENT`      | Constants, immutable values      |
| **Solidity Enums**      | SCREAMING_SNAKE_CASE | `STATUS_ACTIVE`, `STATUS_ENDED`          | Enum values, status codes        |
| **JavaScript/TS Files** | kebab-case           | `explanation-panel.tsx`, `use-wallet.ts` | File names, component files      |
| **JavaScript/TS Vars**  | camelCase            | `userId`, `totalAmount`, `isValid`       | Variables, parameters            |
| **JavaScript/TS Funcs** | camelCase            | `validateInput()`, `processData()`       | Function names                   |
| **React Components**    | PascalCase           | `ExplanationPanel`, `QuestionCard`       | Component names                  |
| **CSS Classes**         | kebab-case           | `text-green-400`, `mt-8`, `flex-1`       | Tailwind classes, custom classes |

**Cross-Reference Consistency Rules:**

1. **Question Text** → Use exact naming from code
   - ✅ "What are the 4 production chain IDs supported by `ProductionLottery`?"
   - ❌ "What are the 4 production chain IDs supported by production lottery?"

2. **Answer Options** → Use exact naming from code
   - ✅ "`STATUS_ACTIVE`, `STATUS_ENDED`, `STATUS_CANCELLED`, `STATUS_REFUNDED`"
   - ❌ "Active, Ended, Cancelled, Refunded"

3. **Explanation Text** → Use exact naming from code
   - ✅ "The `createLottery()` function validates the `prizeAmount` parameter."
   - ❌ "The create lottery function validates the prize amount parameter."

**Solidity-Specific Naming Rules:**

```solidity
// Contracts: PascalCase
contract ProductionLottery { }
contract MockEntropy { }

// Functions: camelCase
function createLottery() { }
function buyTickets() { }

// Variables: camelCase
uint256 lotteryId;
address player;
bool isActive;

// Constants: SCREAMING_SNAKE_CASE
uint256 public constant MAX_TICKETS = 100;
uint256 public constant TICKET_FEE_PERCENT = 500;

// Enums: SCREAMING_SNAKE_CASE
uint8 constant STATUS_ACTIVE = 0;
uint8 constant STATUS_ENDED = 1;
```

**Consistency Verification Checklist:**

- [ ] All contract names use PascalCase in questions, answers, and explanations
- [ ] All function names use camelCase with `()` in questions, answers, and explanations
- [ ] All constants use SCREAMING_SNAKE_CASE in questions, answers, and explanations
- [ ] All variable names use camelCase in questions, answers, and explanations
- [ ] Technical terms wrapped in backticks match exact casing from code
- [ ] Section headers use Title Case (capitalize major words)
- [ ] No inconsistent casing between question and explanation

**Common Mistakes:**

❌ **Inconsistent casing:**
```markdown
Question: "What does the createLottery function do?"
Explanation: "The `CreateLottery()` function validates..."
```

✅ **Consistent casing:**
```markdown
Question: "What does the `createLottery()` function do?"
Explanation: "The `createLottery()` function validates..."
```

❌ **Missing backticks:**
```markdown
The ProductionLottery contract uses ReentrancyGuard.
```

✅ **Proper backticks:**
```markdown
The `ProductionLottery` contract uses `ReentrancyGuard`.
```

---

## Part 4: Universal Quality Checklist

### React-Markdown Rendering Checklist

**Critical for proper rendering:**

- [ ] NO bold text mid-sentence (only section headers on own line)
- [ ] All code blocks have language tag (\`\`\`solidity, \`\`\`javascript, etc.)
- [ ] Code blocks are 3-5 lines (10 max)
- [ ] All technical terms wrapped in backticks
- [ ] Lists use `-` for bullets (not `*` or `+`)
- [ ] Lists have 3-7 items (never exceed 7)
- [ ] List items are 1-2 lines max
- [ ] NO mixing of `**bold**` with `` `code` ``
- [ ] Blank line before/after code blocks
- [ ] Blank line before/after lists
- [ ] NO blank lines between list items

### Naming Convention Checklist

**Consistency across questions, answers, explanations:**

- [ ] Solidity contracts use PascalCase: `ProductionLottery`
- [ ] Solidity functions use camelCase: `createLottery()`
- [ ] Solidity constants use SCREAMING_SNAKE_CASE: `MAX_TICKETS`
- [ ] Solidity variables use camelCase: `lotteryId`
- [ ] Section headers use Title Case: **Why This Value?**
- [ ] Section headers have NO colons
- [ ] Technical terms match exact casing from code
- [ ] All technical terms wrapped in backticks

### Formatting Consistency Checklist

Before submitting an explanation, verify:

- [ ] Follows universal structure (opening → section → code → why → bullets)
- [ ] Uses appropriate heading pattern for content type
- [ ] All technical terms wrapped in inline code backticks
- [ ] Code blocks include language tag (\`\`\`solidity)
- [ ] Bold text ONLY for section headers on own line
- [ ] Bullet lists have 3-7 items with parallel structure
- [ ] Proper spacing between sections (1 blank line)
- [ ] No spacing within bullet lists
- [ ] Section headers use Title Case, no colons

### Visual Hierarchy Checklist

- [ ] Section headers render as block elements (uppercase, gray, mt-8 mb-4)
- [ ] Inline code has green background and text
- [ ] Code blocks have dark background with syntax highlighting
- [ ] Lists have green bullets aligned with first line
- [ ] Blank lines separate major sections
- [ ] No overly long paragraphs (2-4 sentences max)
- [ ] Content is scannable at a glance
- [ ] Logical flow from simple to complex
- [ ] Code blocks are focused (3-5 lines ideal)

### Content Quality Checklist

- [ ] Explains "why", not just "what"
- [ ] Provides concrete examples or numbers
- [ ] Mentions real-world implications
- [ ] Includes edge cases or protections
- [ ] 3-7 bullet points with sufficient detail
- [ ] Technical terms are precise and consistent
- [ ] Naming conventions match across question/answer/explanation

---

## Part 5: Scoring Rubric

Use this rubric to objectively measure explanation quality:

| Criteria                      | Points | Description                                                       |
| ----------------------------- | ------ | ----------------------------------------------------------------- |
| **React-Markdown Rendering**  | /30    | Proper bold usage, code blocks, lists, spacing                    |
| **Naming Conventions**        | /20    | Consistent casing, backticks, cross-reference accuracy            |
| **Visual Hierarchy**          | /20    | Clear spacing, scannable, logical flow, proper component styling  |
| **Content Structure**         | /20    | Follows universal pattern, appropriate headings, 3-7 bullet items |
| **Technical Accuracy**        | /10    | Correct code examples, accurate values, verifiable claims         |
| **Total**                     | /100   | Overall formatting quality score                                  |

**Score Interpretation:**

- **90-100:** Excellent (A) - Publish-ready, all rendering rules followed
- **80-89:** Good (B) - Minor revisions needed, some rendering issues
- **70-79:** Adequate (C) - Moderate revisions needed, multiple issues
- **60-69:** Poor (D) - Significant revisions needed, major rendering problems
- **Below 60:** Unacceptable (F) - Complete rewrite required, breaks rendering

**Target Score: 90+/100 (A grade)**

**Detailed Scoring Breakdown:**

**React-Markdown Rendering (30 points):**
- Bold only for section headers (10 pts)
- Code blocks have language tags (5 pts)
- Code blocks are 3-5 lines (5 pts)
- Lists use `-` and have 3-7 items (5 pts)
- Proper spacing (blank lines) (5 pts)

**Naming Conventions (20 points):**
- Solidity naming correct (PascalCase, camelCase, SCREAMING_SNAKE_CASE) (10 pts)
- Section headers use Title Case, no colons (5 pts)
- Consistent across question/answer/explanation (5 pts)

**Visual Hierarchy (20 points):**
- Section headers render correctly (5 pts)
- Code blocks render with syntax highlighting (5 pts)
- Lists render with green bullets, proper alignment (5 pts)
- Spacing creates clear visual breaks (5 pts)

**Content Structure (20 points):**
- Follows universal pattern (opening → section → code → why → bullets) (10 pts)
- Appropriate heading patterns (5 pts)
- Bullet lists have parallel structure (5 pts)

**Technical Accuracy (10 points):**
- Code examples are correct (5 pts)
- Values are accurate and verifiable (5 pts)

---

## Part 6: Common React-Markdown Rendering Mistakes

### Mistake 1: Bold Text Mid-Sentence (CRITICAL - Breaks Rendering)

❌ **Bad (breaks rendering):**

```markdown
The platform fee is **5%**, calculated as **`500 / 10000`**.
```

**Problem:** Bold renders as block element, breaking paragraph flow.

✅ **Good (renders correctly):**

```markdown
The platform fee is 5%, calculated as `500 / 10000 = 0.05`.
```

**Reasoning:** React-markdown renders `strong` as block element when mid-sentence, breaking layout.

---

### Mistake 2: Mixing Bold and Inline Code (CRITICAL - Breaks Rendering)

❌ **Bad (breaks rendering):**

```markdown
The **`MAX_TICKETS`** constant is set to **`100`**.
```

**Problem:** Mixing `**bold**` with `` `code` `` breaks react-markdown rendering.

✅ **Good (renders correctly):**

```markdown
The `MAX_TICKETS` constant is set to `100`.
```

**Reasoning:** Use one format at a time - inline code OR bold, never both.

---

### Mistake 3: Missing Language Tag on Code Blocks (CRITICAL)

❌ **Bad (no syntax highlighting):**

````markdown
```
uint256 public constant MAX_TICKETS = 100;
```
````

**Problem:** No syntax highlighting, renders as plain text.

✅ **Good (enables syntax highlighting):**

````markdown
```solidity
uint256 public constant MAX_TICKETS = 100;
```
````

**Reasoning:** Language tag triggers syntax highlighting (green keywords, proper colors).

---

### Mistake 4: Inconsistent Naming Conventions

❌ **Bad (inconsistent casing):**

```markdown
Question: "What does the createLottery function do?"
Explanation: "The `CreateLottery()` function validates the `PrizeAmount`..."
```

**Problem:** Casing doesn't match actual code (should be camelCase).

✅ **Good (consistent casing):**

```markdown
Question: "What does the `createLottery()` function do?"
Explanation: "The `createLottery()` function validates the `prizeAmount`..."
```

**Reasoning:** Naming must match exact casing from code for accuracy.

---

### Mistake 5: Section Headers with Colons

❌ **Bad (has colon, lowercase):**

```markdown
**Why this value:**
**How it works:**
```

**Problem:** Inconsistent with Title Case standard, colons add visual noise.

✅ **Good (Title Case, no colon):**

```markdown
**Why This Value?**
**How It Works**
```

**Reasoning:** Title Case is more professional, no colons is cleaner and more scannable.

---

## Part 7: Skeptical Review Framework

### Philosophy: Assume Non-Compliance by Default

**Core Principle:** When reviewing explanations, assume they do NOT follow standards until proven otherwise. This skeptical approach catches subtle violations that optimistic reviews miss.

**Review Mindset:**

- ❌ "Does this look good?" (optimistic, misses issues)
- ✅ "Where does this violate standards?" (skeptical, catches issues)

---

### Step-by-Step Verification Process

**Step 1: Structure Verification (5 minutes)**

Check against universal structure pattern:

```
[ ] Opening statement present?
[ ] Opening uses bold emphasis for key concepts?
[ ] Opening uses inline code for technical terms?
[ ] Code block present (if applicable)?
[ ] Code block has language tag?
[ ] Section heading present?
[ ] Section heading follows standard pattern?
[ ] Bullet list present with 3-5 items?
```

**Evidence required:** Screenshot or line numbers showing each component.

---

**Step 2: Inline Code Audit (3 minutes)**

Scan for ALL technical terms and verify backtick wrapping:

```
[ ] All constants wrapped? (e.g., `MAX_VALUE`)
[ ] All function names wrapped? (e.g., `processData()`)
[ ] All variable names wrapped? (e.g., `userId`)
[ ] All numeric values wrapped? (e.g., `100`, `5%`)
[ ] All type names wrapped? (e.g., `UserAccount`)
[ ] All status values wrapped? (e.g., `ACTIVE`)
```

**Evidence required:** List any unwrapped terms found.

---

**Step 3: Heading Pattern Consistency (2 minutes)**

Compare heading against standard patterns:

```
[ ] Matches one of the 7 standard patterns?
[ ] Pattern is appropriate for content type?
[ ] Same pattern used for similar content elsewhere?
```

**Evidence required:** State which pattern is used and why it's appropriate.

---

**Step 4: Bullet List Quality (3 minutes)**

Verify bullet list follows best practices:

```
[ ] Parallel structure (all start same way)?
[ ] Consistent capitalization?
[ ] Appropriate punctuation?
[ ] 3-5 bullets (not too few, not too many)?
[ ] No nested bullets?
[ ] Each bullet adds unique value?
```

**Evidence required:** Note any violations found.

---

**Step 5: Spacing & Hierarchy (2 minutes)**

Check visual hierarchy:

```
[ ] 1 blank line between sections?
[ ] No blank lines within bullet lists?
[ ] No overly long paragraphs (3+ sentences)?
[ ] Content is scannable at a glance?
```

**Evidence required:** Note any spacing violations.

---

### Objective Scoring Matrix

Use this matrix to calculate a numerical score:

| Component           | Max Points | Scoring Criteria                                                |
| ------------------- | ---------- | --------------------------------------------------------------- |
| **Structure**       | 25         | -5 for each missing component (opening, code, heading, bullets) |
| **Inline Code**     | 25         | -5 for each category of unwrapped terms                         |
| **Heading Pattern** | 15         | -15 if wrong pattern, -5 if inconsistent with similar content   |
| **Bullet Lists**    | 15         | -3 for each violation (structure, capitalization, length, etc.) |
| **Spacing**         | 10         | -2 for each spacing violation                                   |
| **Code Blocks**     | 10         | -5 if no language tag, -5 if no inline comments on complex code |
| **Total**           | 100        | Sum of all components                                           |

**Pass/Fail Threshold:** 90+ points = Pass, <90 points = Revise

---

### Evidence-Based Verification Report Template

```markdown
## Formatting Review Report

**Explanation ID:** [identifier]
**Reviewer:** [name]
**Date:** [date]

### Structure Verification

- [ ] Opening statement: [PASS/FAIL] - [evidence]
- [ ] Code block: [PASS/FAIL] - [evidence]
- [ ] Section heading: [PASS/FAIL] - [evidence]
- [ ] Bullet list: [PASS/FAIL] - [evidence]

### Inline Code Audit

- Unwrapped terms found: [list or "none"]
- Score: [X/25]

### Heading Pattern

- Pattern used: [pattern name]
- Appropriate: [YES/NO] - [reasoning]
- Consistent: [YES/NO] - [evidence]
- Score: [X/15]

### Bullet List Quality

- Violations found: [list or "none"]
- Score: [X/15]

### Spacing & Hierarchy

- Violations found: [list or "none"]
- Score: [X/10]

### Code Block Quality

- Language tag: [PRESENT/MISSING]
- Inline comments: [APPROPRIATE/MISSING/UNNECESSARY]
- Score: [X/10]

### Final Score: [X/100]

**Recommendation:** [APPROVE / REVISE / REJECT]

**Required Changes:** [list specific changes needed]
```

---

### High-Value Content Definition

**What makes content "high-value"?**

High-value content meets ALL of these criteria:

1. **Formatting Excellence (90+ points)**

   - Follows universal structure
   - Consistent inline code usage
   - Appropriate heading pattern
   - Quality bullet lists
   - Proper spacing

2. **Pedagogical Effectiveness (see Content Playbook)**

   - Explains "why", not just "what"
   - Provides concrete examples
   - Uses appropriate scaffolding
   - Defines jargon on first use

3. **Technical Accuracy**

   - All code examples are correct
   - All numbers are verifiable
   - All claims are supported by evidence

4. **Beginner Accessibility**
   - Language appropriate for target audience
   - No unexplained jargon
   - Logical progression from simple to complex

**Measurable Criteria:**

| Criterion              | Measurement                 | Target      |
| ---------------------- | --------------------------- | ----------- |
| **Formatting Score**   | Objective rubric            | 90+/100     |
| **Pedagogical Score**  | Objective rubric            | 90+/100     |
| **Technical Accuracy** | Peer review                 | 100%        |
| **Readability**        | Flesch-Kincaid Grade Level  | 10-12       |
| **Scannability**       | Time to identify key points | <30 seconds |

---

---

## Summary: Critical React-Markdown Rendering Rules

**These rules are NON-NEGOTIABLE for proper rendering:**

### 1. Bold Text Rules
- ✅ **ONLY use bold for section headers on their own line**
- ❌ **NEVER use bold mid-sentence** (renders as block, breaks flow)
- ❌ **NEVER mix `**bold**` with `` `code` ``** (breaks rendering)

### 2. Code Block Rules
- ✅ **ALWAYS specify language tag** (\`\`\`solidity, \`\`\`javascript, etc.)
- ✅ **Keep blocks 3-5 lines** (10 max)
- ✅ **Blank line before and after** code blocks
- ❌ **NEVER use \`\`\` without language** (no syntax highlighting)

### 3. List Rules
- ✅ **Use `-` for bullets** (not `*` or `+`)
- ✅ **3-7 items per list** (never exceed 7)
- ✅ **1-2 lines max per item**
- ✅ **Blank line before/after list**
- ❌ **NO blank lines between list items**

### 4. Naming Convention Rules
- ✅ **Solidity contracts:** PascalCase (`ProductionLottery`)
- ✅ **Solidity functions:** camelCase (`createLottery()`)
- ✅ **Solidity constants:** SCREAMING_SNAKE_CASE (`MAX_TICKETS`)
- ✅ **Section headers:** Title Case, no colons (**Why This Value?**)
- ✅ **All technical terms:** wrapped in backticks

### 5. Spacing Rules
- ✅ **Blank line before/after:** code blocks, lists, section headers
- ✅ **NO blank lines:** between list items, within paragraphs
- ✅ **Consistent spacing:** mt-8 for headers, my-5 for lists, my-6 for code

### 6. Visual Hierarchy
- ✅ **Section headers:** block, uppercase, gray, mt-8 mb-4
- ✅ **Inline code:** green background, green text
- ✅ **Code blocks:** dark background, syntax highlighting
- ✅ **List bullets:** green, aligned with first line

**Target Quality Score: 90+/100**

**Minimum Passing: 80/100**

---

## Version History

- **v3.0** (2025-01-22): Added react-markdown rendering rules, naming conventions, updated scoring rubric
- **v2.0** (2025-01-22): Complete rewrite as universal, principle-focused playbook with skeptical review framework
- **v1.0** (2025-01-22): Initial domain-specific guide (deprecated)
`````
