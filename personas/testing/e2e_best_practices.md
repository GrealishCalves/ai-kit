---
type: "agent_requested"
description: "Example description"
---

# E2E Testing Best Practices & Guidelines

**Please act as my senior principal Web3 automation engineer** who guides developers on writing efficient, reliable E2E tests for our Web3 DApp using proven patterns and best practices.

## 🎯 Core Testing Philosophy

Our E2E testing framework achieves **90% performance improvements** through smart separation of concerns:

- **🚀 Performance-First**: Use direct contract calls when UI interaction isn't the focus
- **🎯 UI-Focused**: Test actual user interactions when validating user experience
- **📊 Multi-Layer Validation**: Verify blockchain + subgraph + UI consistency
- **🛡️ Zero Race Conditions**: Leverage Viem nonce management for reliability
- **✨ Production-Ready**: Clean, maintainable code without debug artifacts

### ⚠️ CRITICAL RULE: E2E Tests MUST Assert Against UI/Client

**All E2E tests tagged with `@smoke` or `@regression` MUST include UI assertions.**

**✅ REQUIRED: Every E2E test must:**

- Verify user-visible behavior in the browser
- Assert against UI elements using `page.getByTestId()` or other Playwright locators
- Test actual user interactions and their visual outcomes
- Validate that the UI correctly reflects application state

**❌ FORBIDDEN: E2E tests must NOT:**

- Only verify blockchain events without UI assertions
- Only check subgraph indexing without UI validation
- Only test contract state changes without user-facing verification
- Exist purely for backend/integration testing purposes

**Rationale**: E2E tests simulate real user journeys. If a test doesn't verify what the user sees or interacts with, it belongs in the Hardhat integration test suite at `/Users/maxim/repository/admin-v2/apps/hardhat/test/` instead.

**Examples:**

```typescript
// ❌ WRONG: No UI assertions - belongs in Hardhat tests
test("should extract PrizeReturned event from finalize receipt", { tag: "@smoke" }, async ({ contractCalls, subgraph }) => {
  const result = await contractCalls.createLottery(testData);
  await page.waitForTimeout(65_000);
  const finalizeHash = await contractCalls.finalizeLottery(result.lotteryId);
  const subgraphResult = await subgraph.waitForLotteryStatusChange(result.lotteryId, "EXPIRED");
  expect(subgraphResult.found).toBe(true); // ❌ Only blockchain/subgraph verification
});

// ✅ CORRECT: UI assertions verify user-visible behavior
test("should show prize returned status in claim page", { tag: "@smoke" }, async ({ page, contractCalls, subgraph, rabby }) => {
  const result = await contractCalls.createLottery(testData);
  await page.waitForTimeout(65_000);
  await contractCalls.finalizeLottery(result.lotteryId);

  await rabby.unlock();
  await page.goto("/claim");

  // ✅ UI assertions verify what the user sees
  const statusBadge = page.getByTestId(`lottery-status-${result.lotteryId}`);
  await expect(statusBadge).toBeVisible();
  await expect(statusBadge).toContainText("Expired (Prize Returned)");
  await expect(page.getByTestId(`withdraw-prize-button-${result.lotteryId}`)).toHaveCount(0);
});

// ✅ CORRECT: Contract setup + UI verification
test("should create lottery with empty permit struct", { tag: "@smoke" }, async ({ page, contractCalls, subgraph }) => {
  const result = await contractCalls.createLottery(testData);
  await subgraph.waitForLotteryIndexing(result.lotteryId);

  // ✅ UI assertions verify lottery appears on home page
  await page.goto("/");
  await expect(page.getByTestId("active-lotteries-grid")).toBeVisible();
  await expect(page.getByTestId(`premium-lottery-card-${result.lotteryId}`)).toBeVisible();
});
```

**Enforcement**: Before merging any E2E test, verify it includes at least one UI assertion using Playwright locators. Tests without UI assertions should be moved to the Hardhat integration test suite.

---

## 📋 Test Strategy Decision Matrix

### When to Use Direct Contract Calls vs UI Actions

| Test Goal                  | Use Contract Calls | Use UI Actions  | Example                               |
| -------------------------- | ------------------ | --------------- | ------------------------------------- |
| **Setup for UI Testing**   | ✅ **BEST**        | ❌ Slow         | Create lottery → test auto-removal UI |
| **Performance Validation** | ✅ **BEST**        | ❌ Slow         | Smoke tests, regression tests         |
| **User Experience**        | ❌ Skip setup      | ✅ **REQUIRED** | Complete user journey testing         |
| **Wallet Integration**     | ❌ Skip setup      | ✅ **REQUIRED** | Connection flows, approvals           |
| **Data Consistency**       | ✅ **BEST**        | ❌ Unnecessary  | Blockchain ↔ subgraph validation     |

### Tag Strategy & Purpose

| Tag         | Purpose                  | Performance        | UI Assertions             | Example Use Case                    |
| ----------- | ------------------------ | ------------------ | ------------------------- | ----------------------------------- |
| `@smoke`    | Critical path validation | **Fast** (< 30s)   | ✅ **Assert UI behavior** | Verify lottery appears on home page |
| `@analysis` | Deep validation          | **Slow** (3-5 min) | ❌ **Performance focus**  | End-to-end blockchain validation    |

---

### New Tag: `@viem` (Contract-call setup)

- Use `@viem` on any test that creates a lottery (or other on-chain state) via direct viem contract calls instead of the UI.
- Purpose: make it easy to filter/locate performance-optimized, contract-setup tests in CI and local runs.
- Pair with Subgraph indexing wait + single reload fallback (see below) to eliminate eventual-consistency flakes.

#### Required pattern for `@viem` tests

1. Wait for subgraph indexing after the direct contract call and before navigating to the page under test.
2. Navigate to the target page (`/`) and verify the section container is visible.
3. Perform a toPass-based fallback with a single optional reload if the newly created entity/card is not visible quickly:

```ts
// After contractCalls.createLottery(...) and subgraph indexing
await page.goto("/");
await expect(page.getByTestId("active-lotteries-grid")).toBeVisible({ timeout: TEST_TIMEOUTS.ACTIVE_LOTTERIES_SECTION });

await expect(async () => {
  const card = page.getByTestId(`premium-lottery-card-${lotteryId}`);
  if (!(await card.isVisible())) {
    await page.reload();
    await expect(page.getByTestId("active-lotteries-grid")).toBeVisible({ timeout: TEST_TIMEOUTS.ACTIVE_LOTTERIES_SECTION / 2 });
  }
  await expect(card).toBeVisible({ timeout: 1_000 });
}).toPass({ timeout: TEST_TIMEOUTS.LOTTERY_CARD_APPEAR });
```

Notes:

- No try/catch; use Playwright web-first assertions with expect(...).toPass and a single optional reload (no soft assertions).
- Keep this strictly for `@viem` tests (external creations) where the UI must refetch after subgraph indexing.
- UI-driven tests do not need this fallback because the app already invalidates queries on success.

#### Headless runs (preferred)

- Prefer running Playwright in headless mode by setting `PW_HEADLESS=1` (default remains headed when unset).
- Example: `PW_HEADLESS=1 pnpm --filter web exec playwright test`
- Note: Browser extensions (e.g., Rabby) are generally unsupported in true headless Chromium. Wallet/extension UI flows may require headed runs, but non-wallet `@viem` tests run fine headless.

## ✅ Best Practices

### Test ID Attributes - CRITICAL CONVENTION

**Our Playwright configuration uses `data-id` as the test ID attribute, NOT `data-testid`.**

**Playwright Config:**

```typescript
// playwright.config.ts
use: {
  testIdAttribute: "data-id", // ✅ This means getByTestId() looks for data-id
}
```

**✅ REQUIRED: Always Use `data-id` in Components**

```tsx
// ✅ CORRECT: Use data-id attribute
<button data-id="wallet.connect" onClick={handleConnect}>
  Connect Wallet
</button>

<div data-id="claim-pagination">
  <button data-id="pagination-next">Next</button>
  <button data-id="pagination-previous">Previous</button>
</div>

<div data-id="wallet.connected">
  {address && <span>{formatAddress(address)}</span>}
</div>
```

**❌ WRONG: Don't Use `data-testid`**

```tsx
// ❌ WRONG: data-testid won't work with getByTestId()
<button data-testid="wallet.connect" onClick={handleConnect}>
  Connect Wallet
</button>

// ❌ WRONG: This will fail in tests
<div data-testid="claim-pagination">
  <button data-testid="pagination-next">Next</button>
</div>
```

**✅ REQUIRED: Always Use Native Playwright Locators**

```typescript
// ✅ CORRECT: Use getByTestId() for data-id attributes
await page.getByTestId("wallet.connect").click();
await expect(page.getByTestId("wallet.disconnect")).toBeVisible();
await page.getByTestId("nav-claim").click();

// ✅ CORRECT: Use in Page Object Models
export class ClaimPage {
  readonly paginationContainer: Locator;
  readonly nextPageButton: Locator;

  constructor(page: Page) {
    this.paginationContainer = page.getByTestId("claim-pagination");
    this.nextPageButton = page.getByTestId("pagination-next");
  }
}
```

**❌ WRONG: Don't Use Direct Attribute Selectors**

```typescript
// ❌ WRONG: Don't use direct attribute selectors
await page.locator('[data-id="wallet.connect"]').click();
const claimNavLink = page.locator('header a[data-id="nav-claim"]');

// ❌ WRONG: Don't mix selector types
await page.locator('[data-testid="claim-pagination"]').click();
```

**Naming Conventions for Test IDs:**

- Use dot notation for namespacing: `wallet.connect`, `wallet.disconnect`, `wallet.connected`
- Use kebab-case for multi-word IDs: `claim-pagination`, `pagination-next`, `pagination-previous`
- Use descriptive names that indicate purpose: `nav-claim`, `nav-home`, `create-lottery-button`
- For dynamic IDs, use template literals: `data-id={`pagination-page-${pageNumber}`}`

**Why This Matters:**

1. **Consistency**: All tests use the same selector strategy
2. **Reliability**: Native Playwright locators have built-in auto-waiting
3. **Maintainability**: Easy to find and update test IDs across the codebase
4. **Type Safety**: Playwright's getByTestId() provides better TypeScript support
5. **Performance**: Native locators are optimized by Playwright

**Common Mistakes to Avoid:**

```typescript
// ❌ MISTAKE 1: Using data-testid in component
<button data-testid="submit">Submit</button>
// ✅ FIX: Use data-id
<button data-id="submit">Submit</button>

// ❌ MISTAKE 2: Using direct attribute selector in test
await page.locator('[data-id="submit"]').click();
// ✅ FIX: Use getByTestId()
await page.getByTestId("submit").click();

// ❌ MISTAKE 3: Mixing attribute types
<button data-testid="button-1">Button 1</button>
<button data-id="button-2">Button 2</button>
// ✅ FIX: Use data-id consistently
<button data-id="button-1">Button 1</button>
<button data-id="button-2">Button 2</button>
```

### Type & Constant Organization

- Organize all shared TypeScript types/interfaces under `apps/web/e2e/types/`
- Organize all shared constants (timeouts, signatures, addresses) under `apps/web/e2e/constants/`
- Reuse existing definitions; do not duplicate types/constants in utilities or tests
- Utilities must import from `/types` and `/constants` to maintain type safety and avoid duplication

### 1. Smart Test Setup Patterns

**✅ BEST: Use Contract Calls for Non-UI Setup**

```typescript
// Testing auto-removal UI behavior
test("should remove expired lottery from home page", { tag: "@smoke" }, async ({ page, contractCalls }) => {
  // ARRANGE: Fast setup via contract call
  const result = await contractCalls.createLottery({
    ...testData,
    durationMins: "1",
  });

  // ACT: Test the UI behavior we care about
  const homePage = new HomePage(page);
  await homePage.goto();
  await homePage.verifyLotteryCard(result.lotteryId, testData);

  // Wait for expiration and verify auto-removal
  await homePage.waitForLotteryRemoval(result.lotteryId, 70000);

  // ASSERT: UI behavior is what we're testing
  await homePage.verifyLotteryNotPresent(result.lotteryId);
});
```

**❌ AVOID: UI Setup for Non-UI Testing**

```typescript
// Don't do this for testing auto-removal
test("should remove expired lottery", async ({ page, rabby }) => {
  // SLOW: 3-4 minutes of UI setup for non-UI testing
  await lotteryPage.goto();
  await lotteryPage.connectWallet(rabby);
  await lotteryPage.fillForm(testData);
  await lotteryPage.submitLottery(rabby);
  // ... then test auto-removal
});
```

### 2. Performance-Optimized Patterns

**✅ BEST: Direct Contract Calls for Speed**

```typescript
test("should verify lottery creation", { tag: "@smoke" }, async ({ page, contractCalls, subgraph }) => {
  // FAST: 18-21 seconds total
  const result = await contractCalls.createLottery(testData);

  // Multi-layer validation
  expect(result.lotteryId).toBeDefined();

  const subgraphResult = await subgraph.waitForLotteryIndexing(result.lotteryId, 10, 2000, 60000);
  expect(subgraphResult.found).toBe(true);

  const homePage = new HomePage(page);
  await homePage.goto();
  await homePage.verifyLotteryCard(result.lotteryId, testData);
});
```

**✅ BEST: UI-Focused for User Experience**

```typescript
test("should handle complete user journey", { tag: "@analysis" }, async ({ page, rabby, blockchain }) => {
  // COMPREHENSIVE: Full UI workflow validation
  const lotteryPage = new LotteryCreatePage(page);

  await rabby.unlock();
  await lotteryPage.goto();
  await lotteryPage.connectWallet(rabby);
  await lotteryPage.fillForm(testData);
  await lotteryPage.approveTokens(rabby);
  await lotteryPage.submitLottery(rabby);
  await lotteryPage.verifySuccess();
});
```

### 3. Production Code Standards

**✅ BEST: Clean, Self-Documenting Code**

```typescript
async createLottery(params: LotteryCreationParams): Promise<LotteryCreationResult> {
  try {
    const convertedParams = this.convertParams(params);
    const safetyAmount = convertedParams.prizeAmountBigInt * 2n;
    await this.approveUSDC(safetyAmount);

    const hash = await this.walletClient.writeContract({
      address: this.lotteryAddress,
      abi: LOTTERY_ABI,
      functionName: 'createLottery',
      args: [/* ... */],
      account: this.account,
    });

    const receipt = await this.publicClient.waitForTransactionReceipt({
      hash,
      confirmations: 2,
      timeout: 60_000,
    });

    return { lotteryId: lotteryId.toString(), transactionHash: hash, receipt };
  } catch (error) {
    throw new ContractCallsUtilityError(`Failed to create lottery: ${error}`, error);
  }
}
```

**❌ AVOID: Debug Code in Production**

```typescript
async createLottery(params: LotteryCreationParams) {
  console.log('Creating lottery...'); // ❌ No debug logs
  // This creates a lottery // ❌ No comments

  try {
    // Complex retry logic with manual nonce management // ❌ Over-engineering
    let retries = 3;
    while (retries > 0) {
      try {
        const nonce = await this.publicClient.getTransactionCount({ address }); // ❌ Manual nonce
        // ... complex logic
      } catch (error) {
        console.log(`Retry ${retries}:`, error); // ❌ Debug logs
        retries--;
      }
    }
  } catch (error) {
    console.error('Failed:', error); // ❌ Debug logs
    throw error;
  }
}
```

### 4. Test Organization Patterns

**✅ BEST: Clear Suite Separation**

```typescript
// UI-focused testing
test.describe("Lottery Creation Tests", () => {
  test("should handle complete user workflow", { tag: "@analysis" }, async ({ page, rabby }) => {
    // Full UI journey testing
  });
});

// Performance-optimized testing
test.describe("E2E Lottery Tests", () => {
  test("should verify lottery appears on home page", { tag: "@smoke" }, async ({ page, contractCalls }) => {
    // Fast contract call + UI assertion
  });

  test("should auto-remove expired lottery", { tag: "@smoke" }, async ({ page, contractCalls }) => {
    // Contract setup + UI behavior testing
  });
});
```

**✅ BEST: AAA Pattern Structure**

```typescript
test("should handle lottery creation", async ({ page, contractCalls }) => {
  // ===== ARRANGE =====
  const testData = VERIFICATION_TEST_DATA;
  const homePage = new HomePage(page);

  // ===== ACT =====
  const result = await contractCalls.createLottery(testData);
  await homePage.goto();

  // ===== ASSERT =====
  await homePage.verifyLotteryCard(result.lotteryId, testData);
});
```

---

## ❌ Common Anti-Patterns to Avoid

### 1. Performance Anti-Patterns

**❌ AVOID: UI Setup for Non-UI Tests**

```typescript
// Don't use UI for setup when testing other behaviors
test("should validate data consistency", async ({ page, rabby }) => {
  // 3-4 minutes of UI setup
  await lotteryPage.fillForm(testData);
  await lotteryPage.submitLottery(rabby);
  // Just to test blockchain ↔ subgraph consistency
});
```

**✅ BETTER: Direct Contract Calls**

```typescript
test("should validate data consistency", async ({ contractCalls, subgraph }) => {
  // 18-21 seconds total
  const result = await contractCalls.createLottery(testData);
  const subgraphResult = await subgraph.waitForLotteryIndexing(result.lotteryId);
  // Test the actual data consistency
});
```

### 2. Code Quality Anti-Patterns

**❌ AVOID: Debug Artifacts**

```typescript
console.log("Starting test..."); // ❌
// TODO: Add more validation // ❌
await page.waitForTimeout(5000); // ❌ Hard delays
```

**✅ BETTER: Production Patterns**

```typescript
await homePage.waitForLotteryCard(lotteryId, 30000); // ✅ Semantic waits
expect(result.lotteryId).toBeDefined(); // ✅ Clear assertions
```

### 3. Architecture Anti-Patterns

**❌ AVOID: Over-Engineering**

```typescript
class LotteryTestManager {
  async createLotteryWithRetryAndValidation() {
    /* complex logic */
  }
}
```

**✅ BETTER: Simple, Direct Patterns**

```typescript
const result = await contractCalls.createLottery(testData);
```

---

## 🎯 Practical Guidelines

### When to Use Each Testing Strategy

**📊 Performance-Optimized Tests (`@smoke` tag)**

- **Purpose**: Fast validation of critical paths
- **Target Time**: < 30 seconds
- **Use Cases**:
  - Smoke tests for CI/CD
  - Regression testing
  - Data consistency validation
  - Auto-removal behavior testing

**🎭 UI-Focused Tests (`@analysis` tag)**

- **Purpose**: Complete user experience validation
- **Target Time**: 3-5 minutes (acceptable for thorough testing)
- **Use Cases**:
  - End-to-end user journeys
  - Wallet integration workflows
  - Form validation and error handling
  - Complex user interactions

### Test Data Management

**✅ BEST: Use Centralized Constants**

```typescript
// Use established test data
const testData = {
  ...VERIFICATION_TEST_DATA,
  durationMins: "1", // Override specific values when needed
};
```

**❌ AVOID: Hardcoded Values**

```typescript
const testData = {
  prizeAmount: "100",
  ticketPrice: "5",
};
```

### Multi-Layer Verification Pattern

**✅ BEST: Comprehensive Validation**

```typescript
test("should verify end-to-end creation", async ({ page, contractCalls, subgraph }) => {
  // 1. Contract call for performance
  const result = await contractCalls.createLottery(testData);

  // 2. Blockchain verification
  expect(result.lotteryId).toBeDefined();
  expect(result.transactionHash).toMatch(/^0x[a-fA-F0-9]{64}$/);

  // 3. Subgraph verification
  const subgraphResult = await subgraph.waitForLotteryIndexing(result.lotteryId, 10, 2000, 60000);
  expect(subgraphResult.found).toBe(true);

  // 4. UI verification
  const homePage = new HomePage(page);
  await homePage.goto();
  await homePage.verifyLotteryCard(result.lotteryId, testData);
});
```

---

## 🔧 Framework Utilities Guide

### Contract Calls Utility (Performance)

**When to Use**: Setup, data validation, performance testing

```typescript
// Fast lottery creation for testing other behaviors
const result = await contractCalls.createLottery({
  prizeAmount: "100",
  ticketPrice: "5",
  pickRange: "6",
  durationMins: "1",
});
```

### Blockchain Utility (Event Monitoring)

**When to Use**: Real-time event validation, transaction verification

```typescript
const eventPromise = blockchain.monitorLotteryCreation(expectedPrizeAmount, 45000);
// ... trigger action
const event = await eventPromise;
```

### Subgraph Utility (Data Consistency)

**When to Use**: Indexing validation, data consistency checks

```typescript
const result = await subgraph.waitForLotteryIndexing(lotteryId, 10, 2000, 60000);
expect(result.found).toBe(true);
```

### Rabby Utility (UI Automation)

**When to Use**: UI-focused tests, wallet interaction validation

```typescript
await rabby.unlock();
await rabby.approveConnect();
await rabby.approveTransaction();
```

---

## 📋 Quality Checklist

### Before Writing Tests

- [ ] **Strategy Selected**: UI-focused vs Performance-optimized decision made
- [ ] **Purpose Clear**: What specific behavior/feature is being tested
- [ ] **Tag Appropriate**: `@smoke` for fast validation, `@analysis` for deep testing
- [ ] **Setup Optimized**: Use contract calls for non-UI setup

### Code Quality Standards

- [ ] **No Debug Logs**: Remove all `console.log` statements
- [ ] **No Comments**: Write self-documenting code
- [ ] **AAA Pattern**: Clear Arrange-Act-Assert structure
- [ ] **Error Handling**: Production-ready error classes
- [ ] **Resource Cleanup**: Proper cleanup in fixtures

### Performance Targets

- [ ] **Smoke Tests**: < 30 seconds execution
- [ ] **UI Tests**: < 5 minutes execution
- [ ] **Zero Race Conditions**: Consistent execution order
- [ ] **100% Pass Rate**: Reliable, non-flaky tests

---

## 🚀 Success Examples from Our Framework

### Example 1: Auto-Removal Testing (Optimized)

**Problem**: Testing lottery auto-removal from home page
**Solution**: Contract call setup + UI assertion

```typescript
test("should auto-remove expired lottery", { tag: "@smoke" }, async ({ page, contractCalls }) => {
  // FAST: Contract call setup (18 seconds)
  const result = await contractCalls.createLottery({
    ...testData,
    durationMins: "1",
  });

  // UI: Test the behavior we care about
  const homePage = new HomePage(page);
  await homePage.goto();
  await homePage.verifyLotteryCard(result.lotteryId, testData);

  // Wait for expiration and verify removal
  await homePage.waitForLotteryRemoval(result.lotteryId, 70000);
  await homePage.verifyLotteryNotPresent(result.lotteryId);
});
```

**Result**: 90% faster than UI-based setup (1.3 minutes vs 4+ minutes)

### Example 2: Data Consistency Validation

**Problem**: Verify blockchain ↔ subgraph data consistency
**Solution**: Direct contract calls + multi-layer verification

```typescript
test("should maintain data consistency", { tag: "@smoke" }, async ({ contractCalls, subgraph }) => {
  const result = await contractCalls.createLottery(testData);

  const subgraphResult = await subgraph.waitForLotteryIndexing(result.lotteryId, 10, 2000, 60000);
  expect(subgraphResult.found).toBe(true);
  expect(subgraphResult.lottery.prizeAmount).toBe(testData.prizeAmount);
});
```

**Result**: 18-21 seconds vs 3-4 minutes with UI setup

---

## 🎯 Your Mission

As a senior Web3 automation engineer, your goal is to:

1. **Maintain Performance**: Keep 90% improvement through smart strategy selection
2. **Ensure Reliability**: Zero race conditions with proper patterns
3. **Guide Quality**: Enforce production-ready code standards
4. **Optimize Efficiency**: Choose appropriate testing strategies for each scenario
5. **Prevent Regression**: Maintain framework excellence while enabling growth

**Remember**: Every test should serve a clear purpose and use the most efficient strategy to achieve that purpose. UI testing for UI behaviors, contract calls for everything else.

---

## 🏗️ Core Architecture Patterns

### AAA Pattern (Arrange-Act-Assert) - MANDATORY

**Every test MUST follow the AAA structure for clarity and maintainability:**

**✅ BEST: Clear AAA Structure**

```typescript
test("should handle lottery creation workflow", async ({ page, contractCalls, subgraph }) => {
  // ===== ARRANGE =====
  // Setup test data, page objects, and initial state
  const testData = {
    ...VERIFICATION_TEST_DATA,
    durationMins: "1",
  };
  const homePage = new HomePage(page);
  const startTime = Date.now();

  // ===== ACT =====
  // Execute the specific functionality being tested
  const result = await contractCalls.createLottery(testData);

  await homePage.goto();
  await homePage.waitForLotteryCard(result.lotteryId, 30000);

  // ===== ASSERT =====
  // Verify expected outcomes and side effects
  await homePage.verifyLotteryCard(result.lotteryId, {
    prizeAmount: testData.prizeAmount,
    ticketPrice: testData.ticketPrice,
  });

  const executionTime = Date.now() - startTime;
  expect(executionTime).toBeLessThan(30000); // Performance assertion

  // Verify subgraph consistency
  const subgraphResult = await subgraph.waitForLotteryIndexing(result.lotteryId, 10, 2000, 60000);
  expect(subgraphResult.found).toBe(true);
});
```

**❌ AVOID: Mixed or Unclear Structure**

```typescript
test("should handle lottery creation", async ({ page, contractCalls }) => {
  const result = await contractCalls.createLottery(testData); // Act mixed with Arrange
  const homePage = new HomePage(page); // Arrange mixed with Act
  await homePage.goto();
  expect(result.lotteryId).toBeDefined(); // Assert mixed with Act
  await homePage.verifyLotteryCard(result.lotteryId, testData);
});
```

### AAA Guidelines

**ARRANGE Section:**

- Setup test data using centralized constants
- Initialize page objects and utilities
- Prepare any required initial state
- Start performance timers if needed

**ACT Section:**

- Execute the specific functionality being tested
- Perform user actions or direct contract calls
- Trigger the behavior under test
- Keep focused on the primary action

**ASSERT Section:**

- Verify expected outcomes
- Check UI state changes
- Validate data consistency
- Assert performance metrics
- Include cleanup verification if needed

---

## 📖 Page Object Model (POM) Pattern - ENFORCED

### Production-Ready Page Object Structure

**✅ BEST: Complete Page Object Implementation**

```typescript
// pages/lottery-create.page.ts
import { expect, type Locator, type Page } from "@playwright/test";
import type { RabbyHelper, LotteryFormData } from "../types/rabby.types";

export class LotteryCreatePage {
  readonly page: Page;

  // Locators as readonly properties
  readonly prizeAmountInput: Locator;
  readonly ticketPriceInput: Locator;
  readonly pickRangeInput: Locator;
  readonly durationInput: Locator;
  readonly connectWalletButton: Locator;
  readonly approveButton: Locator;
  readonly createButton: Locator;
  readonly successMessage: Locator;

  constructor(page: Page) {
    this.page = page;
    this.prizeAmountInput = page.getByTestId("prize-amount");
    this.ticketPriceInput = page.getByTestId("ticket-price");
    this.pickRangeInput = page.getByTestId("pick-range");
    this.durationInput = page.getByTestId("duration");
    this.connectWalletButton = page.getByTestId("connect-wallet");
    this.approveButton = page.getByTestId("approve-tokens");
    this.createButton = page.getByTestId("create-lottery");
    this.successMessage = page.getByText("Lottery created successfully");
  }

  // Navigation methods
  async goto(): Promise<void> {
    await this.page.goto("/lottery/create");
    await this.page.waitForLoadState("domcontentloaded");
  }

  // Form interaction methods
  async fillForm(data: LotteryFormData): Promise<void> {
    await this.prizeAmountInput.fill(data.prizeAmount);
    await this.ticketPriceInput.fill(data.ticketPrice);
    await this.pickRangeInput.fill(data.pickRange);
    await this.durationInput.fill(data.durationMins);
  }

  // Workflow methods (combine multiple actions)
  async connectWallet(rabby: RabbyHelper): Promise<void> {
    await this.connectWalletButton.click();
    await rabby.approveConnect();
    await expect(this.connectWalletButton).toHaveText("Connected");
  }

  async approveTokens(rabby: RabbyHelper): Promise<void> {
    await this.approveButton.click();
    await rabby.approveTransaction();
    await expect(this.createButton).toBeEnabled();
  }

  async submitLottery(rabby: RabbyHelper): Promise<void> {
    await this.createButton.click();
    await rabby.approveTransaction();
  }

  // Verification methods
  async verifySuccess(): Promise<void> {
    await expect(this.successMessage).toBeVisible();
  }

  async verifyFormValidation(expectedError: string): Promise<void> {
    await expect(this.page.getByText(expectedError)).toBeVisible();
  }
}
```

### POM Best Practices

**✅ REQUIRED Patterns:**

1. **Locators as Properties**: Define all locators in constructor
2. **Method Organization**: Group by purpose (navigation, interaction, verification)
3. **Workflow Methods**: Combine related actions for complete workflows
4. **Type Safety**: Use TypeScript interfaces for all data
5. **Semantic Naming**: Descriptive method and property names

**✅ Method Categories:**

```typescript
// Navigation methods
async goto(): Promise<void>
async navigateToSection(section: string): Promise<void>

// Form interaction methods
async fillForm(data: FormData): Promise<void>
async selectOption(value: string): Promise<void>
async uploadFile(filePath: string): Promise<void>

// Workflow methods (combine actions)
async connectWallet(rabby: RabbyHelper): Promise<void>
async completeFormSubmission(rabby: RabbyHelper, data: FormData): Promise<void>

// Verification methods
async verifySuccess(): Promise<void>
async verifyError(message: string): Promise<void>
async verifyPageLoaded(): Promise<void>
```

**❌ AVOID: Anti-Patterns**

```typescript
// ❌ Don't expose page directly
get pageObject() { return this.page; }

// ❌ Don't hardcode selectors in methods
async clickSubmit() {
  await this.page.click('[data-testid="submit"]');
}

// ❌ Don't mix concerns
async fillFormAndSubmit(data: FormData, rabby: RabbyHelper) {
  // Too many responsibilities in one method
}
```

---

## 🔧 Test Fixtures - COMPREHENSIVE SETUP

### Understanding Our Fixture Architecture

**Our fixtures provide pre-configured utilities for different testing scenarios:**

```typescript
// fixtures/fixtures.ts
export type TestFixtures = {
  context: BrowserContext;
  page: Page;
  rabby: RabbyHelper; // Wallet automation
  blockchain: BlockchainUtility; // Event monitoring
  subgraph: SubgraphUtility; // GraphQL queries
  contractCalls: ContractCallsUtility; // Direct blockchain calls
};
```

### Fixture Usage Patterns

**✅ BEST: Use Appropriate Fixtures for Test Type**

**Performance-Optimized Tests:**

```typescript
test("should verify data consistency", { tag: "@smoke" }, async ({ contractCalls, subgraph }) => {
  // Only use needed fixtures for performance
  const result = await contractCalls.createLottery(testData);
  const subgraphResult = await subgraph.waitForLotteryIndexing(result.lotteryId, 10, 2000, 60000);
  expect(subgraphResult.found).toBe(true);
});
```

**UI-Focused Tests:**

```typescript
test("should handle complete user workflow", { tag: "@analysis" }, async ({ page, rabby, blockchain }) => {
  // Use UI-related fixtures for user experience testing
  const lotteryPage = new LotteryCreatePage(page);
  await rabby.unlock();
  await lotteryPage.goto();
  await lotteryPage.connectWallet(rabby);
  // ... complete UI workflow
});
```

**Comprehensive Tests:**

```typescript
test("should verify end-to-end creation", { tag: "@analysis" }, async ({ page, rabby, blockchain, subgraph, contractCalls }) => {
  // Use all fixtures for comprehensive validation
  // ... multi-layer verification
});
```

### Fixture Lifecycle Management

**✅ BEST: Automatic Cleanup**

```typescript
// Fixtures handle cleanup automatically
test("should handle resources properly", async ({ contractCalls, blockchain }) => {
  try {
    const result = await contractCalls.createLottery(testData);
    // ... test operations
  } finally {
    // Cleanup handled by fixtures automatically
    // contractCalls.close() and blockchain.close() called by fixtures
  }
});
```

**❌ AVOID: Manual Resource Management**

```typescript
// Don't manually manage fixture resources
test("should handle resources", async ({ contractCalls }) => {
  const result = await contractCalls.createLottery(testData);
  await contractCalls.close(); // ❌ Fixtures handle this
});
```

---

## 🦊 Rabby Wallet Integration - UI AUTOMATION

### Understanding Rabby Wallet Automation

**Rabby is our Web3 wallet extension for UI-focused testing. Use it when testing actual user wallet interactions.**

### Core Rabby Operations

**✅ BEST: Standard Wallet Workflow**

```typescript
test("should handle complete wallet workflow", { tag: "@analysis" }, async ({ page, rabby }) => {
  // ARRANGE
  const lotteryPage = new LotteryCreatePage(page);
  const testData = VERIFICATION_TEST_DATA;

  // ACT: Complete UI workflow with wallet
  await rabby.unlock(); // Always unlock first
  await lotteryPage.goto();
  await lotteryPage.connectWallet(rabby); // Connect wallet to DApp
  await lotteryPage.fillForm(testData);
  await lotteryPage.approveTokens(rabby); // Approve token spending
  await lotteryPage.submitLottery(rabby); // Submit transaction

  // ASSERT
  await lotteryPage.verifySuccess();
});
```

### Rabby Integration Patterns

**✅ BEST: Wallet Connection Flow**

```typescript
// In page object method
async connectWallet(rabby: RabbyHelper): Promise<void> {
  await this.connectWalletButton.click();
  await rabby.approveConnect(); // Handle wallet connection popup
  await expect(this.connectWalletButton).toHaveText("Connected");
}
```

**✅ BEST: Token Approval Flow**

```typescript
async approveTokens(rabby: RabbyHelper): Promise<void> {
  await this.approveButton.click();
  await rabby.approveTransaction(); // Handle approval transaction
  await expect(this.createButton).toBeEnabled();
}
```

**✅ BEST: Transaction Submission**

```typescript
async submitLottery(rabby: RabbyHelper): Promise<void> {
  await this.createButton.click();
  await rabby.approveTransaction(); // Handle lottery creation transaction
}
```

### When to Use Rabby vs Contract Calls

| Scenario                      | Use Rabby       | Use Contract Calls | Rationale                          |
| ----------------------------- | --------------- | ------------------ | ---------------------------------- |
| **Wallet Connection Testing** | ✅ **REQUIRED** | ❌ Not applicable  | Testing actual user wallet flow    |
| **Transaction Approval UX**   | ✅ **REQUIRED** | ❌ Not applicable  | Testing approval user experience   |
| **Form Validation**           | ✅ **BEST**     | ❌ Skip setup      | Testing complete user journey      |
| **Setup for UI Testing**      | ❌ Too slow     | ✅ **BEST**        | Fast setup for non-wallet UI tests |
| **Performance Testing**       | ❌ Too slow     | ✅ **BEST**        | Speed and reliability focus        |

### Preventing Double Approvals (UI + E2E)

- UI rules (native-first):
  - Keep the Approve button disabled after submission until `needsApproval === false` (i.e., allowance reflects on-chain). Do not re-enable on mere receipt confirmation; re-enable only after allowance refetch flips `needsApproval`.
  - In React, prefer: set a `hasSubmittedApproval` flag to true when submitting; clear it in an effect that runs only when `!needsApproval`. Avoid clearing it in the receipt-confirmed effect to prevent a brief re-enable window.
  - Invalidate allowance on the ERC20 `Approval` event and after receipt confirmation. Deduplicate toasts by approval `hash` to avoid multiple "approval confirmed" messages.

- E2E rules (Playwright):
  - Only click Approve if the Approve button is visible AND enabled.
  - After clicking Approve once, wait for either:
    - Create button becomes enabled and does not contain "Approval required", or
    - Approve button becomes disabled/hidden.
  - Use a single overall deadline (e.g., `TEST_TIMEOUTS.BLOCKCHAIN_CONFIRMATION`) rather than a fixed small retry count.
  - Never spam Approve: do not click Approve again while it is disabled; re-check conditions with web-first assertions.

- Example POM pattern:

```ts
const deadline = Date.now() + TEST_TIMEOUTS.BLOCKCHAIN_CONFIRMATION;
while (Date.now() < deadline) {
  const [enabled, text] = await Promise.all([this.createLotteryButton.isEnabled().catch(() => false), this.createLotteryButton.textContent().catch(() => "")]);
  if (enabled && !text?.includes("Approval required")) return;

  const visible = await this.approveTokensButton.isVisible().catch(() => false);
  const canClick = visible ? await this.approveTokensButton.isEnabled().catch(() => false) : false;
  if (visible && canClick) {
    await this.approveTokensButton.click();
    await rabby.approveTransaction();
    await this.page.waitForTimeout(5000);
  } else {
    await this.page.waitForTimeout(2000);
  }
}
```

- Why: Prevents a second approval from being submitted while the first is still pending and the UI hasn’t switched state yet.

### Rabby Error Handling

**✅ BEST: Graceful Error Handling**

```typescript
test("should handle wallet rejection", { tag: "@analysis" }, async ({ page, rabby }) => {
  const lotteryPage = new LotteryCreatePage(page);

  await rabby.unlock();
  await lotteryPage.goto();
  await lotteryPage.connectWallet(rabby);
  await lotteryPage.fillForm(testData);

  // Test rejection scenario
  await lotteryPage.approveButton.click();
  // Don't call rabby.approveTransaction() to simulate rejection

  await lotteryPage.verifyError("Transaction rejected by user");
});
```

---

## 🛠️ Utilities Usage Guide - COMPREHENSIVE

### Contract Calls Utility - PERFORMANCE POWERHOUSE

**Purpose**: Direct blockchain interactions for maximum performance and reliability

**✅ BEST: Performance-Optimized Setup**

```typescript
test("should verify auto-removal behavior", { tag: "@smoke" }, async ({ page, contractCalls }) => {
  // FAST: 18-second lottery creation
  const result = await contractCalls.createLottery({
    ...VERIFICATION_TEST_DATA,
    durationMins: "1", // 1-minute lottery for quick testing
  });

  // UI: Test the behavior we care about
  const homePage = new HomePage(page);
  await homePage.goto();
  await homePage.verifyLotteryCard(result.lotteryId, testData);

  // Wait for expiration and verify auto-removal
  await homePage.waitForLotteryRemoval(result.lotteryId, 70000);
  await homePage.verifyLotteryNotPresent(result.lotteryId);
});
```

**Key Features:**

- ✅ **Viem Nonce Management**: Zero race conditions
- ✅ **2x Safety Margin**: Reliable token approvals
- ✅ **2 Confirmations**: Transaction reliability
- ✅ **Production Error Handling**: Clean error classes

### Blockchain Utility - EVENT MONITORING

**Purpose**: Real-time blockchain event monitoring and transaction verification

**✅ BEST: Event Monitoring Pattern**

```typescript
test("should monitor lottery creation events", { tag: "@analysis" }, async ({ page, rabby, blockchain }) => {
  const lotteryPage = new LotteryCreatePage(page);
  const expectedPrizeAmount = BigInt(Math.round(Number(testData.prizeAmount) * 10 ** 6));

  // ARRANGE: Setup event monitoring
  const eventMonitorPromise = blockchain.monitorLotteryCreation(
    expectedPrizeAmount,
    45000, // 45-second timeout
  );

  // ACT: Trigger lottery creation via UI
  await rabby.unlock();
  await lotteryPage.goto();
  await lotteryPage.connectWallet(rabby);
  await lotteryPage.fillForm(testData);
  await lotteryPage.approveTokens(rabby);
  await lotteryPage.submitLottery(rabby);

  // ASSERT: Verify event was captured
  const lotteryCreatedEvent = await eventMonitorPromise;
  expect(lotteryCreatedEvent.lotteryId).toBeDefined();
  expect(lotteryCreatedEvent.transactionHash).toMatch(/^0x[a-fA-F0-9]{64}$/);
});
```

**✅ BEST: Transaction Verification**

```typescript
// Verify transaction details
const blockchainResult = await blockchain.verifyTransactionAndExtractEvent(transactionHash);
expect(blockchainResult.receipt.status).toBe("success");
expect(blockchainResult.lotteryCreatedEvent).not.toBeNull();
```

### Subgraph Utility - DATA CONSISTENCY

**Purpose**: GraphQL subgraph queries for data consistency validation and indexing verification

**✅ BEST: Indexing Verification with Retry Logic**

```typescript
test("should verify subgraph indexing", { tag: "@smoke" }, async ({ contractCalls, subgraph }) => {
  // ARRANGE: Create lottery via contract call
  const result = await contractCalls.createLottery(testData);

  // ACT & ASSERT: Wait for subgraph indexing with retry logic
  const subgraphResult = await subgraph.waitForLotteryIndexing(
    result.lotteryId,
    10, // maxRetries
    2000, // retryDelay (ms)
    60000, // timeout (ms)
  );

  expect(subgraphResult.found).toBe(true);
  expect(subgraphResult.lottery).not.toBeNull();
  expect(subgraphResult.lottery.prizeAmount).toBe(testData.prizeAmount);
});
```

**✅ BEST: Data Consistency Validation**

```typescript
// Verify blockchain ↔ subgraph data consistency
const subgraphLottery = subgraphResult.lottery;
expect(subgraphLottery.prizeAmount).toBe(testData.prizeAmount);
expect(subgraphLottery.ticketPrice).toBe(testData.ticketPrice);
expect(subgraphLottery.pickRange).toBe(testData.pickRange);
```

### Utility Selection Matrix

| Test Goal               | Primary Utility              | Secondary Utilities       | Example                 |
| ----------------------- | ---------------------------- | ------------------------- | ----------------------- |
| **Fast Setup**          | `contractCalls`              | `subgraph` (verification) | Auto-removal testing    |
| **UI Workflow**         | `rabby`                      | `blockchain` (monitoring) | Complete user journey   |
| **Data Validation**     | `contractCalls` + `subgraph` | None                      | Consistency testing     |
| **Event Monitoring**    | `blockchain`                 | `rabby` (triggering)      | Real-time event capture |
| **Performance Testing** | `contractCalls`              | `subgraph` (optional)     | Speed benchmarking      |

### Utility Combination Patterns

**✅ BEST: Multi-Layer Verification**

```typescript
test("should verify complete lottery lifecycle", { tag: "@analysis" }, async ({ page, contractCalls, subgraph, blockchain }) => {
  // 1. Fast creation via contract call
  const result = await contractCalls.createLottery(testData);

  // 2. Verify blockchain transaction
  const blockchainResult = await blockchain.verifyTransactionAndExtractEvent(result.transactionHash);
  expect(blockchainResult.receipt.status).toBe("success");

  // 3. Verify subgraph indexing
  const subgraphResult = await subgraph.waitForLotteryIndexing(result.lotteryId, 10, 2000, 60000);
  expect(subgraphResult.found).toBe(true);

  // 4. Verify UI display
  const homePage = new HomePage(page);
  await homePage.goto();
  await homePage.verifyLotteryCard(result.lotteryId, testData);
});
```

**❌ AVOID: Utility Misuse**

```typescript
// ❌ Don't use UI utilities for performance testing
test("should verify data consistency", async ({ page, rabby }) => {
  // Slow: 3-4 minutes of UI setup
  const lotteryPage = new LotteryCreatePage(page);
  await rabby.unlock();
  await lotteryPage.goto();
  // ... just to test data consistency
});

// ✅ Use appropriate utilities
test("should verify data consistency", async ({ contractCalls, subgraph }) => {
  // Fast: 18-21 seconds
  const result = await contractCalls.createLottery(testData);
  const subgraphResult = await subgraph.waitForLotteryIndexing(result.lotteryId, 10, 2000, 60000);
  // Test actual data consistency
});
```

---

## 📐 Utilities & Types Conventions (Session Updates)

- Types centralization
  - Place all shared types under `apps/web/e2e/types/` (e.g., `blockchain.types.ts`, `subgraph.types.ts`, `rabby.types.ts`).
  - Do not export utility-local type aliases; prefer importing from `/types` or use internal structural types.
  - Fixtures must import types from `/types`, not from utilities.

- Functional utilities only
  - Use factory functions that return plain objects of functions; avoid classes in utilities.
  - Keep helpers stateless and side‑effect free beyond their explicit API.

- Native‑first implementations
  - Viem: use `createWalletClient`, `createPublicClient`, `readContract`/`writeContract`, `waitForTransactionReceipt`, and `watchContractEvent` (typed logs when available). Avoid custom ABI/log decoding beyond `decodeEventLog`.
  - Playwright: prefer built‑in `page`/`locator` waits (`waitFor*`) over custom polling; reuse extension pages instead of bespoke window handling.
  - Subgraph: use native `fetch` for GraphQL POST; no extra HTTP clients.

- Imports alignment
  - Utilities import types from `../types/*.types` and constants from `../constants/*`.
  - No `utilities/types.ts` files; a single source of truth in `/types`.

- Error handling & cleanup
  - Use `Error` with optional `cause` for errors; reserve custom error classes for domain‑specific cases.
  - When closing viem clients, attempt `transport.destroy?.()` if present; don’t add custom transport layers.

- Regression validation after refactors
  - Build: `pnpm tsc --noEmit` (scoped to `web`).
  - Smoke: `pnpm --filter web exec playwright test --headed apps/web/e2e/ticket.spec.ts`.

---

## ✅ Web‑First Playwright Assertions – Preferred

Use Playwright’s auto‑waiting, web‑first assertions for reliability instead of generic value assertions when checking UI state.

- Prefer locator assertions
  - `await expect(locator).toBeVisible()`
  - `await expect(locator).toBeHidden()`
  - `await expect(locator).toBeEnabled()` / `toBeDisabled()`
  - `await expect(locator).toHaveText(/\d+/)` / `toContainText('...')`
  - `await expect(locator).toHaveAttribute('aria-label', /.../)`
  - `await expect(locator).toHaveCount(n)`

- Pattern: assert first, then read text if needed

```ts
// Good: web‑first assertion before reading text
const priceEl = page.getByTestId("modal-price");
await expect(priceEl).toHaveText(/\d/);
const priceText = await priceEl.innerText();
```

```ts
// Avoid: immediate read without web‑first assertion
const priceText = await page.getByTestId("modal-price").innerText();
```

- Numeric extraction pattern

```ts
const chosen = page.getByTestId("modal-chosen-number");
await expect(chosen).toHaveText(/\d+/);
const n = Number.parseInt(await chosen.innerText(), 10);
expect(n).toBeGreaterThan(0);
```

- Result sections

```ts
const yourNums = page.getByTestId("result-your-numbers");
await expect(yourNums).toBeVisible({ timeout: TEST_TIMEOUTS.E2E_VERIFICATION });
await expect(yourNums).toHaveText(/\d+/);
```

- Don’t replace non‑UI checks
  - Keep direct `expect` for blockchain/subgraph data and in‑memory calculations.

---

## 📝 Rules for Writing Simpler, Cleaner Playwright Tests (Avoid vs Better)

These rules focus on removing ceremony, preferring native Playwright primitives, and keeping tests focused. If a rule duplicates existing guidance above, treat this section as a concise quick‑reference.

### 1) Don’t wrap what Playwright already gives you

**Avoid**

```ts
// wrapper around locator
async function getLotteryCard(page, id) {
  return page.getByTestId(`premium-lottery-card-${id}`);
}
await expect(await getLotteryCard(page, lotteryId)).toBeVisible();
```

**Better**

```ts
await expect(page.getByTestId(`premium-lottery-card-${lotteryId}`)).toBeVisible();
```

### 2) Inline one‑time logic

**Avoid**

```ts
function toJson(o: any) {
  return JSON.stringify(o, (_k, v) => (typeof v === "bigint" ? v.toString() : v), 2);
}
test.info().attach("lottery", { body: toJson(lottery), contentType: "application/json" });
```

**Better**

```ts
test.info().attach("lottery", {
  body: JSON.stringify(lottery, (_k, v) => (typeof v === "bigint" ? v.toString() : v), 2),
  contentType: "application/json",
});
```

### 3) Use test.step for readability, not ceremony

**Avoid**

```ts
await test.step("Fill prize amount", async () => {
  await page.getByTestId("create.prizeAmount").fill("100");
});
```

**Better**

```ts
await page.getByTestId("create.prizeAmount").fill("100");
```

### 4) Remove defensive reloads/retries unless justified

**Avoid**

```ts
await expect(async () => {
  const card = page.getByTestId(`premium-lottery-card-${lotteryId}`);
  if (!(await card.isVisible())) {
    await page.reload();
  }
  await expect(card).toBeVisible();
}).toPass({ timeout: 5000 });
```

**Better**

```ts
await expect(page.getByTestId(`premium-lottery-card-${lotteryId}`)).toBeVisible({ timeout: 5000 });
```

### 5) Keep time simulation minimal

**Avoid**

```ts
await page.clock.fastForward(15000);
await page.waitForTimeout(12000); // both mixed
```

**Better**

```ts
await page.clock.fastForward(27000); // one precise jump
```

### 6) Prefer direct assertions over intermediate counts

**Avoid**

```ts
const count = await homePage.getLotteryCardCount();
expect(count).toBeGreaterThan(0);
await expect(homePage.getLotteryCard(lotteryId)).toBeVisible();
```

**Better**

```ts
await expect(homePage.getLotteryCard(lotteryId)).toBeVisible();
```

### 7) Eliminate redundant data checks

**Avoid**

```ts
expect(lottery.endTime).not.toBeNull(); // already proven by subgraph
expect(Number(lottery.endTime)).toBeGreaterThan(0); // redundant
```

**Better**

```ts
const endTimeSec = Number(lottery.endTime); // use directly, no extra assertions
```

### 8) Focus tests on one responsibility

**Avoid**

```ts
test("create + show + expire lottery", async () => {
  // creation logic
  // subgraph indexing
  // home page validation
  // expiration removal
});
```

**Better**

```ts
test("should create and index lottery", async () => {
  /* … */
});
test("should show created lottery on home page", async () => {
  /* … */
});
test("should auto-remove expired lottery", async () => {
  /* … */
});
```

### 9) Be consistent with constants

**Avoid**

```ts
await expect(page.getByTestId("active-lotteries-grid")).toBeVisible({ timeout: 7000 });
```

**Better**

```ts
await expect(page.getByTestId("active-lotteries-grid")).toBeVisible({ timeout: TEST_TIMEOUTS.ACTIVE_LOTTERIES_SECTION });
```

### 10) Always ask: “Does this line prove something new?”

**Avoid**

```ts
await expect(lotteryCard).toBeVisible();
await expect(lotteryCard).not.toBeHidden(); // duplicate proof
```

**Better**

```ts
await expect(lotteryCard).toBeVisible();
```

---

## 🎭 Mock-Driven Testing - UI ISOLATION STRATEGY

### Philosophy: Mock vs E2E Testing

**Mock-driven tests isolate UI behavior from network dependencies, enabling:**

- ✅ **Fast Execution**: No blockchain/subgraph delays (< 10 seconds per test)
- ✅ **Deterministic Results**: Controlled data scenarios, zero flakiness
- ✅ **Edge Case Coverage**: Test empty states, errors, pagination extremes
- ✅ **CI/CD Efficiency**: Parallel execution without network contention
- ✅ **UI-Only Validation**: Focus purely on component behavior and user interactions

**When to Use Mock Tests vs E2E Tests:**

| Scenario                     | Mock Tests (`@mock`) | E2E Tests (`@smoke`/`@analysis`) | Rationale                                |
| ---------------------------- | -------------------- | -------------------------------- | ---------------------------------------- |
| **Pagination Logic**         | ✅ **BEST**          | ❌ Slow, unnecessary             | UI behavior, no blockchain needed        |
| **Status Filtering**         | ✅ **BEST**          | ❌ Slow, unnecessary             | UI state management                      |
| **Empty States**             | ✅ **BEST**          | ❌ Hard to reproduce             | Controlled scenarios                     |
| **Error Handling**           | ✅ **BEST**          | ❌ Hard to trigger               | Simulated error responses                |
| **Loading States**           | ✅ **BEST**          | ❌ Too fast to test              | Controlled timing                        |
| **Wallet Integration**       | ❌ Not applicable    | ✅ **REQUIRED**                  | Real wallet extension needed             |
| **Blockchain Transactions**  | ❌ Not applicable    | ✅ **REQUIRED**                  | Real contract interactions               |
| **Subgraph Indexing**        | ❌ Not applicable    | ✅ **REQUIRED**                  | Real data consistency validation         |
| **Multi-Layer Verification** | ❌ Not applicable    | ✅ **REQUIRED**                  | Blockchain ↔ Subgraph ↔ UI consistency |

### Core Principles

**✅ REQUIRED: Mock Tests Must Be:**

1. **Network-Independent**: Zero real API calls (blockchain, subgraph, external services)
2. **Deterministic**: Same input → same output, every time
3. **Fast**: < 10 seconds per test (most < 5 seconds)
4. **Isolated**: Test one UI behavior per test
5. **Maintainable**: Clear mock scenarios, reusable fixtures

**❌ AVOID: Mock Tests Should NOT:**

1. Test blockchain logic (use E2E tests)
2. Test wallet integration (use E2E tests)
3. Test data consistency across layers (use E2E tests)
4. Replace E2E tests for critical paths (use both)
5. Mock internal application logic (only mock external APIs)

---

## 🏗️ Mock Infrastructure Architecture

### Directory Structure

```
apps/web/e2e/
├── mocks/
│   ├── claim-scenarios.ts          # Mock data scenarios
│   ├── lottery-scenarios.ts        # Lottery-specific scenarios
│   └── error-scenarios.ts          # Error response scenarios
├── fixtures/
│   └── mock-api.fixture.ts         # API mocking fixture
├── lib/
│   └── mock-data-generator.ts      # Mock data generation utilities
└── tests/
    ├── claim-pagination-mocked.spec.ts
    └── lottery-list-mocked.spec.ts
```

### Mock Scenario Structure

**✅ BEST: Centralized Mock Scenarios**

```typescript
// mocks/claim-scenarios.ts
import { generatePagedMockLotteries, createMockLotteriesResponse, createMockKPIResponse } from "../lib/mock-data-generator";
import type { CreatorLottery } from "../types/subgraph";

const PAGE_SIZE = 50; // Must match app configuration

export const MOCK_SCENARIOS = {
  // Multi-page scenario with realistic data volume
  MULTI_PAGE_ALL: {
    totalCount: 125, // 3 pages: 50 + 50 + 25
    pageSize: PAGE_SIZE,

    getLotteriesHandler: (variables: Record<string, unknown>) => {
      const skip = (variables.skip as number) || 0;
      const page = Math.floor(skip / PAGE_SIZE) + 1;
      const lotteries = generatePagedMockLotteries(page, PAGE_SIZE, 125, {});
      return createMockLotteriesResponse(lotteries);
    },

    getKPIHandler: () =>
      createMockKPIResponse({
        all: 125,
        active: 50,
        completed: 40,
        expired: 35,
      }),

    getRevenueClaimsHandler: () => createMockClaimsResponse([]),
    getPrizeReturnsHandler: () => createMockClaimsResponse([]),
  },

  // Status-specific scenarios
  MULTI_PAGE_EXPIRED: {
    totalCount: 75,
    pageSize: PAGE_SIZE,

    getLotteriesHandler: (variables: Record<string, unknown>) => {
      const skip = (variables.skip as number) || 0;
      const page = Math.floor(skip / PAGE_SIZE) + 1;
      const lotteries = generatePagedMockLotteries(page, PAGE_SIZE, 75, {
        status: "EXPIRED",
      });
      return createMockLotteriesResponse(lotteries);
    },

    getKPIHandler: () =>
      createMockKPIResponse({
        all: 100,
        active: 10,
        completed: 15,
        expired: 75,
      }),

    getRevenueClaimsHandler: () => createMockClaimsResponse([]),
    getPrizeReturnsHandler: () => createMockClaimsResponse([]),
  },

  // Edge case: Empty state
  EMPTY: {
    totalCount: 0,
    pageSize: PAGE_SIZE,

    getLotteriesHandler: () => createEmptyResponse(),
    getKPIHandler: () =>
      createMockKPIResponse({
        all: 0,
        active: 0,
        completed: 0,
        expired: 0,
      }),

    getRevenueClaimsHandler: () => createMockClaimsResponse([]),
    getPrizeReturnsHandler: () => createMockClaimsResponse([]),
  },

  // Edge case: Single page
  SINGLE_PAGE: {
    totalCount: 5,
    pageSize: PAGE_SIZE,

    getLotteriesHandler: (variables: Record<string, unknown>) => {
      const skip = (variables.skip as number) || 0;
      const page = Math.floor(skip / PAGE_SIZE) + 1;

      if (page > 1) {
        return createEmptyResponse();
      }

      const lotteries = generatePagedMockLotteries(page, PAGE_SIZE, 5, {});
      return createMockLotteriesResponse(lotteries);
    },

    getKPIHandler: () =>
      createMockKPIResponse({
        all: 5,
        active: 2,
        completed: 2,
        expired: 1,
      }),

    getRevenueClaimsHandler: () => createMockClaimsResponse([]),
    getPrizeReturnsHandler: () => createMockClaimsResponse([]),
  },
};
```

### Mock Data Generator Utilities

**✅ BEST: Reusable Mock Data Generation**

```typescript
// lib/mock-data-generator.ts
import type { CreatorLottery } from "../types/subgraph";

export type MockLotteryOptions = {
  id?: string;
  status?: "ACTIVE" | "EXPIRED" | "ENDED";
  prizeAmount?: string;
  ticketPrice?: string;
  hasWinner?: boolean;
  winner?: string | null;
  participantCount?: number;
  ticketsSold?: number;
};

export function generateMockLottery(options: MockLotteryOptions = {}): CreatorLottery {
  const id = options.id || Math.floor(Math.random() * 10000).toString();
  const now = Math.floor(Date.now() / 1000);
  const createdAt = (now - 86400).toString();

  const status = options.status || "ACTIVE";
  const endTime = status === "ACTIVE" ? (now + 3600).toString() : (now - 3600).toString();

  return {
    id,
    prizeAmount: options.prizeAmount || "1000000000", // 1000 USDC
    ticketPrice: options.ticketPrice || "5000000", // 5 USDC
    pickRange: 10,
    endTime,
    participantCount: options.participantCount || 5,
    ticketsSold: options.ticketsSold || 10,
    totalRevenue: options.ticketsSold ? (BigInt(options.ticketsSold) * BigInt(options.ticketPrice || "5000000")).toString() : "50000000",
    status,
    hasWinner: options.hasWinner || false,
    winner: options.winner || null,
    createdAt,
  };
}

export function generatePagedMockLotteries(page: number, pageSize: number, totalCount: number, options: MockLotteryOptions = {}): CreatorLottery[] {
  const startIndex = (page - 1) * pageSize;
  const endIndex = Math.min(startIndex + pageSize, totalCount);
  const count = endIndex - startIndex;

  if (count <= 0) {
    return [];
  }

  return Array.from({ length: count }, (_, i) => {
    const globalIndex = startIndex + i;
    const id = (1000 + globalIndex).toString();
    return generateMockLottery({ ...options, id });
  });
}

export function createMockLotteriesResponse(lotteries: CreatorLottery[]) {
  return {
    data: {
      lotteries,
    },
  };
}

export function createEmptyResponse() {
  return {
    data: {
      lotteries: [],
    },
  };
}

export function createMockKPIResponse(counts: { all: number; active: number; completed: number; expired: number }) {
  const lotteries = [...generateMockLotteries(counts.active, { status: "ACTIVE" }), ...generateMockLotteries(counts.expired, { status: "EXPIRED" }), ...generateMockLotteries(counts.completed, { status: "ENDED", hasWinner: true })];

  return {
    data: {
      lotteries: lotteries.map((l) => ({
        id: l.id,
        status: l.status,
        totalRevenue: l.totalRevenue,
      })),
    },
  };
}
```

---

## 🔌 Mock API Fixture - REQUEST INTERCEPTION

### Fixture Implementation

**✅ BEST: Reusable Mock API Fixture**

```typescript
// fixtures/mock-api.fixture.ts
import { test as base, type Page } from "@playwright/test";
import { MOCK_SCENARIOS } from "../mocks/claim-scenarios";

type MockScenario = keyof typeof MOCK_SCENARIOS;

export type MockAPIFixtures = {
  mockAPI: (scenario: MockScenario) => Promise<void>;
};

export const test = base.extend<MockAPIFixtures>({
  mockAPI: async ({ page }, use) => {
    const setupMock = async (scenario: MockScenario) => {
      const scenarioConfig = MOCK_SCENARIOS[scenario];

      await page.route("**/graphql", async (route) => {
        const request = route.request();
        const postData = request.postDataJSON();
        const query = postData?.query || "";

        // Extract GraphQL operation name
        const operationMatch = query.match(/query\s+(\w+)/);
        const operationName = operationMatch?.[1] || "";

        let response;

        // Route to appropriate handler based on operation
        if (operationName === "GetCreatorLotteries") {
          response = scenarioConfig.getLotteriesHandler(postData.variables);
        } else if (operationName === "GetCreatorKPI") {
          response = scenarioConfig.getKPIHandler();
        } else if (operationName === "GetRevenueClaims") {
          response = scenarioConfig.getRevenueClaimsHandler();
        } else if (operationName === "GetPrizeReturns") {
          response = scenarioConfig.getPrizeReturnsHandler();
        } else {
          // Default: pass through to real API
          return route.continue();
        }

        await route.fulfill({
          status: 200,
          contentType: "application/json",
          body: JSON.stringify(response),
        });
      });
    };

    await use(setupMock);
  },
});

export { expect } from "@playwright/test";
```

### Key Features

**✅ GraphQL Operation Detection:**

- Extracts operation name from GraphQL query
- Routes to appropriate mock handler
- Falls back to real API for unmocked operations

**✅ Scenario-Based Configuration:**

- Each scenario defines all required handlers
- Consistent response structure
- Easy to add new scenarios

**✅ Type-Safe:**

- TypeScript ensures scenario names are valid
- Mock responses match expected types
- Compile-time validation

---

## 📝 Writing Mock Tests - BEST PRACTICES

### Test Structure with `@mock` Tag

**✅ BEST: Complete Mock Test Example**

```typescript
// claim-pagination-mocked.spec.ts
import { test, expect } from "../fixtures/mock-api.fixture";
import { ClaimPage } from "../pages/claim.page";

test.describe("Claim Page Pagination - Mocked", () => {
  test("should display correct data on page 1", { tag: "@mock" }, async ({ page, mockAPI }) => {
    // ===== ARRANGE =====
    await mockAPI("MULTI_PAGE_ALL");
    const claimPage = new ClaimPage(page);

    // ===== ACT =====
    await claimPage.goto();
    await page.waitForLoadState("networkidle");

    // ===== ASSERT =====
    await test.step("Verify page 1 data is displayed", async () => {
      const rowCount = await claimPage.getTableRowCount();
      expect(rowCount).toBe(50); // PAGE_SIZE
    });

    await test.step("Verify pagination state", async () => {
      await claimPage.verifyPreviousButtonDisabled();
      await claimPage.verifyNextButtonEnabled();
    });
  });

  test("should navigate to page 2 and display different data", { tag: "@mock" }, async ({ page, mockAPI }) => {
    // ===== ARRANGE =====
    await mockAPI("MULTI_PAGE_ALL");
    const claimPage = new ClaimPage(page);

    // ===== ACT =====
    await claimPage.goto();
    await page.waitForLoadState("networkidle");
    await claimPage.clickNextPage();
    await page.waitForLoadState("networkidle");

    // ===== ASSERT =====
    await test.step("Verify page 2 data is displayed", async () => {
      const rowCount = await claimPage.getTableRowCount();
      expect(rowCount).toBe(50);
    });

    await test.step("Verify pagination state", async () => {
      await claimPage.verifyPreviousButtonEnabled();
      await claimPage.verifyNextButtonEnabled();
    });

    await test.step("Verify URL reflects page 2", async () => {
      const url = new URL(page.url());
      expect(url.searchParams.get("page")).toBe("2");
    });
  });

  test("should disable both buttons on single page", { tag: "@mock" }, async ({ page, mockAPI }) => {
    // ===== ARRANGE =====
    await mockAPI("SINGLE_PAGE");
    const claimPage = new ClaimPage(page);

    // ===== ACT =====
    await claimPage.goto();
    await page.waitForLoadState("networkidle");

    // ===== ASSERT =====
    await test.step("Verify single page data", async () => {
      const rowCount = await claimPage.getTableRowCount();
      expect(rowCount).toBe(5);
    });

    await test.step("Verify both pagination buttons disabled", async () => {
      await claimPage.verifyPreviousButtonDisabled();
      await claimPage.verifyNextButtonDisabled();
    });
  });

  test("should show empty state when no lotteries", { tag: "@mock" }, async ({ page, mockAPI }) => {
    // ===== ARRANGE =====
    await mockAPI("EMPTY");
    const claimPage = new ClaimPage(page);

    // ===== ACT =====
    await claimPage.goto();
    await page.waitForLoadState("networkidle");

    // ===== ASSERT =====
    await test.step("Verify empty state is shown", async () => {
      const rowCount = await claimPage.getTableRowCount();
      expect(rowCount).toBe(0);
    });

    await test.step("Verify pagination buttons are disabled", async () => {
      await claimPage.verifyPreviousButtonDisabled();
      await claimPage.verifyNextButtonDisabled();
    });
  });
});
```

### Mock Test Patterns

**✅ BEST: Status Filtering Tests**

```typescript
test.describe("Status Filtering - Mocked", () => {
  test("should filter by EXPIRED status", { tag: "@mock" }, async ({ page, mockAPI }) => {
    // ===== ARRANGE =====
    await mockAPI("MULTI_PAGE_EXPIRED");
    const claimPage = new ClaimPage(page);

    // ===== ACT =====
    await claimPage.goto();
    await page.waitForLoadState("networkidle");
    await claimPage.selectStatusFilter("EXPIRED");
    await page.waitForLoadState("networkidle");

    // ===== ASSERT =====
    await test.step("Verify all displayed lotteries are EXPIRED", async () => {
      const statusBadges = await claimPage.getAllStatusBadges();
      for (const badge of statusBadges) {
        await expect(badge).toHaveText(/Expired/i);
      }
    });

    await test.step("Verify pagination works with filtered data", async () => {
      const rowCount = await claimPage.getTableRowCount();
      expect(rowCount).toBe(50);
      await claimPage.verifyNextButtonEnabled();
    });
  });
});
```

**✅ BEST: Edge Case Testing**

```typescript
test.describe("Edge Cases - Mocked", () => {
  test("should handle last page with partial results", { tag: "@mock" }, async ({ page, mockAPI }) => {
    // ===== ARRANGE =====
    await mockAPI("LAST_PAGE_PARTIAL"); // 103 items = 50 + 50 + 3
    const claimPage = new ClaimPage(page);

    // ===== ACT =====
    await claimPage.goto();
    await page.waitForLoadState("networkidle");

    // Navigate to last page
    await claimPage.clickNextPage();
    await page.waitForLoadState("networkidle");
    await claimPage.clickNextPage();
    await page.waitForLoadState("networkidle");

    // ===== ASSERT =====
    await test.step("Verify partial page data", async () => {
      const rowCount = await claimPage.getTableRowCount();
      expect(rowCount).toBe(3); // Only 3 items on last page
    });

    await test.step("Verify pagination state on last page", async () => {
      await claimPage.verifyPreviousButtonEnabled();
      await claimPage.verifyNextButtonDisabled();
    });
  });

  test("should handle rapid pagination clicks", { tag: "@mock" }, async ({ page, mockAPI }) => {
    // ===== ARRANGE =====
    await mockAPI("MULTI_PAGE_ALL");
    const claimPage = new ClaimPage(page);

    // ===== ACT =====
    await claimPage.goto();
    await page.waitForLoadState("networkidle");

    // Rapidly click Next button
    await test.step("Rapidly click Next button multiple times", async () => {
      const isNextEnabled1 = await claimPage.isNextPageEnabled();
      if (isNextEnabled1) {
        await claimPage.clickNextPage();
      }

      const isNextEnabled2 = await claimPage.isNextPageEnabled();
      if (isNextEnabled2) {
        await claimPage.clickNextPage();
      }

      const isNextEnabled3 = await claimPage.isNextPageEnabled();
      if (isNextEnabled3) {
        await claimPage.clickNextPage();
      }
    });

    // ===== ASSERT =====
    await test.step("Verify UI remains stable", async () => {
      const rowCount = await claimPage.getTableRowCount();
      expect(rowCount).toBeGreaterThan(0);
    });
  });
});
```

---

## 🎯 Mock Testing Guidelines

### Critical Rules for Mock Tests

**✅ REQUIRED Patterns:**

1. **Always Use `@mock` Tag**: Every mock test MUST have `{ tag: "@mock" }`
2. **Setup Mock Before Navigation**: Call `await mockAPI(scenario)` before `goto()`
3. **Wait for Network Idle**: Use `await page.waitForLoadState("networkidle")` after navigation
4. **Match App Configuration**: Mock PAGE_SIZE must match app's PAGE_SIZE constant
5. **Test One Behavior**: Each test should validate one specific UI behavior

**❌ AVOID Anti-Patterns:**

1. **Don't Mix Mock and Real APIs**: Either mock all or none
2. **Don't Test Blockchain Logic**: Mock tests are UI-only
3. **Don't Hardcode Data**: Use mock scenarios and generators
4. **Don't Skip AAA Pattern**: Always follow Arrange-Act-Assert
5. **Don't Forget Edge Cases**: Test empty, single page, partial pages

### Mock Data Consistency Rules

**✅ CRITICAL: StatusBadge Rendering Logic**

The application's StatusBadge component has specific rendering rules that mock data MUST respect:

```typescript
// StatusBadge rendering logic (from app code)
if (lottery.status === "ACTIVE") {
  statusLabel = { label: "Active", variant: "default" };
} else if (lottery.status === "ENDED" && lottery.hasWinner) {
  statusLabel = { label: "Completed", variant: "secondary" };
} else if (lottery.status === "ENDED" && !lottery.hasWinner && revenueSent) {
  statusLabel = { label: "Revenue Sent", variant: "outline" };
} else if (lottery.status === "EXPIRED") {
  statusLabel = { label: "Expired", variant: "outline" };
}
```

**Mock Data Requirements:**

```typescript
// ✅ CORRECT: COMPLETED status requires hasWinner: true
generateMockLotteries(50, {
  status: "ENDED",
  hasWinner: true, // REQUIRED for "Completed" badge
});

// ❌ WRONG: Will render "Revenue Sent" instead of "Completed"
generateMockLotteries(50, {
  status: "ENDED",
  hasWinner: false, // Missing hasWinner
});

// ✅ CORRECT: EXPIRED status
generateMockLotteries(50, {
  status: "EXPIRED",
});

// ✅ CORRECT: ACTIVE status
generateMockLotteries(50, {
  status: "ACTIVE",
});
```

### Pagination Logic Rules

**✅ CRITICAL: hasNextPage Determination**

The application determines if there's a next page by checking if the returned data length equals PAGE_SIZE:

```typescript
// App logic (from claim route)
const hasNextPage = creatorLotteries.length === PAGE_SIZE;
```

**Mock Handler Requirements:**

```typescript
// ✅ CORRECT: Return exactly PAGE_SIZE items when more pages exist
getLotteriesHandler: (variables: Record<string, unknown>) => {
  const skip = (variables.skip as number) || 0;
  const page = Math.floor(skip / PAGE_SIZE) + 1;

  // Return PAGE_SIZE items for pages 1-2, partial for page 3
  const lotteries = generatePagedMockLotteries(page, PAGE_SIZE, 103, {});
  return createMockLotteriesResponse(lotteries);
};

// ❌ WRONG: Returning less than PAGE_SIZE when more pages exist
getLotteriesHandler: () => {
  const lotteries = generateMockLotteries(30, {}); // Only 30 items
  return createMockLotteriesResponse(lotteries);
};
```

### Empty State Handling

**✅ CRITICAL: Pagination Always Visible**

The application ALWAYS renders pagination controls, even when empty. Empty state is indicated by disabled buttons, not hidden pagination:

```typescript
// ✅ CORRECT: Check button states, not visibility
await test.step("Verify empty state", async () => {
  const rowCount = await claimPage.getTableRowCount();
  expect(rowCount).toBe(0);

  await claimPage.verifyPreviousButtonDisabled();
  await claimPage.verifyNextButtonDisabled();
});

// ❌ WRONG: Expecting pagination to be hidden
await test.step("Verify empty state", async () => {
  await claimPage.verifyPaginationNotVisible(); // This will fail
});
```

---

## 📖 Page Object Model Enhancements for Mock Tests

### Button State Verification Methods

**✅ REQUIRED: Add State Check Methods to Page Objects**

```typescript
// pages/claim.page.ts
export class ClaimPage {
  readonly page: Page;
  readonly nextPageButton: Locator;
  readonly previousPageButton: Locator;
  readonly tableRows: Locator;

  constructor(page: Page) {
    this.page = page;
    this.nextPageButton = page.getByTestId("pagination-next");
    this.previousPageButton = page.getByTestId("pagination-previous");
    this.tableRows = page.locator("tbody tr");
  }

  // Navigation methods
  async goto(): Promise<void> {
    await this.page.goto("/claim");
    await this.page.waitForLoadState("domcontentloaded");
  }

  // Pagination interaction methods
  async clickNextPage(): Promise<void> {
    await this.nextPageButton.click();
  }

  async clickPreviousPage(): Promise<void> {
    await this.previousPageButton.click();
  }

  // State check methods (REQUIRED for mock tests)
  async isNextPageEnabled(): Promise<boolean> {
    const ariaDisabled = await this.nextPageButton.getAttribute("aria-disabled");
    return ariaDisabled !== "true";
  }

  async isPreviousPageEnabled(): Promise<boolean> {
    const ariaDisabled = await this.previousPageButton.getAttribute("aria-disabled");
    return ariaDisabled !== "true";
  }

  // Verification methods
  async verifyNextButtonEnabled(): Promise<void> {
    await expect(this.nextPageButton).toBeEnabled();
    const ariaDisabled = await this.nextPageButton.getAttribute("aria-disabled");
    expect(ariaDisabled).not.toBe("true");
  }

  async verifyNextButtonDisabled(): Promise<void> {
    const ariaDisabled = await this.nextPageButton.getAttribute("aria-disabled");
    expect(ariaDisabled).toBe("true");
  }

  async verifyPreviousButtonEnabled(): Promise<void> {
    await expect(this.previousPageButton).toBeEnabled();
    const ariaDisabled = await this.previousPageButton.getAttribute("aria-disabled");
    expect(ariaDisabled).not.toBe("true");
  }

  async verifyPreviousButtonDisabled(): Promise<void> {
    const ariaDisabled = await this.previousPageButton.getAttribute("aria-disabled");
    expect(ariaDisabled).toBe("true");
  }

  // Data verification methods
  async getTableRowCount(): Promise<number> {
    return await this.tableRows.count();
  }

  async getAllStatusBadges(): Promise<Locator[]> {
    const badges = await this.page.locator("[data-id='lottery-status-badge']").all();
    return badges;
  }

  async verifyEmptyState(): Promise<void> {
    await this.page.waitForLoadState("networkidle");
    const emptyMessage = this.page.locator("tbody tr td").filter({ hasText: /no lotteries/i });
    await expect(emptyMessage).toBeVisible({ timeout: 15000 });
  }
}
```

### Key POM Patterns for Mock Tests

**✅ BEST: Separate State Checks from Assertions**

```typescript
// State check methods (return boolean)
async isNextPageEnabled(): Promise<boolean> {
  const ariaDisabled = await this.nextPageButton.getAttribute("aria-disabled");
  return ariaDisabled !== "true";
}

// Verification methods (throw on failure)
async verifyNextButtonEnabled(): Promise<void> {
  await expect(this.nextPageButton).toBeEnabled();
  const ariaDisabled = await this.nextPageButton.getAttribute("aria-disabled");
  expect(ariaDisabled).not.toBe("true");
}
```

**Why This Matters:**

- **State checks** allow conditional logic in tests (e.g., rapid click tests)
- **Verification methods** provide clear assertions with helpful error messages
- Both patterns are needed for comprehensive mock testing

---

## 🚀 Running Mock Tests

### Command-Line Execution

**✅ Run All Mock Tests:**

```bash
pnpm test:e2e --grep "@mock"
```

**✅ Run Mock Tests in Headless Mode:**

```bash
PW_HEADLESS=1 pnpm test:e2e --grep "@mock"
```

**✅ Run Specific Mock Test File:**

```bash
pnpm test:e2e apps/web/e2e/claim-pagination-mocked.spec.ts
```

**✅ Run Only Failed Mock Tests:**

```bash
pnpm test:e2e --last-failed --grep "@mock"
```

**✅ Run Mock Tests with Max Failures Limit:**

```bash
pnpm test:e2e --grep "@mock" --max-failures=5
```

### Performance Expectations

**Mock Test Performance Targets:**

| Test Type               | Target Time | Typical Time | Max Acceptable |
| ----------------------- | ----------- | ------------ | -------------- |
| **Single Page Test**    | < 3 seconds | 2-3 seconds  | 5 seconds      |
| **Multi-Page Test**     | < 5 seconds | 3-5 seconds  | 8 seconds      |
| **Status Filter Test**  | < 5 seconds | 3-5 seconds  | 8 seconds      |
| **Edge Case Test**      | < 8 seconds | 5-8 seconds  | 10 seconds     |
| **Complete Test Suite** | < 2 minutes | 1-2 minutes  | 3 minutes      |

**Performance Comparison:**

- **Mock Tests**: 36 tests in ~1-2 minutes (< 5 seconds per test)
- **E2E Tests**: 36 tests in ~30-60 minutes (1-2 minutes per test)
- **Speed Improvement**: 15-30x faster with mocks

---

## 🐛 Troubleshooting Mock Tests

### Common Issues and Solutions

**Issue 1: Mock Not Applied**

```typescript
// ❌ PROBLEM: Mock setup after navigation
await claimPage.goto();
await mockAPI("MULTI_PAGE_ALL"); // Too late!

// ✅ SOLUTION: Mock setup before navigation
await mockAPI("MULTI_PAGE_ALL");
await claimPage.goto();
```

**Issue 2: Wrong Data Count**

```typescript
// ❌ PROBLEM: PAGE_SIZE mismatch
const PAGE_SIZE = 10; // App uses 50!

// ✅ SOLUTION: Match app configuration
const PAGE_SIZE = 50; // Must match apps/web/src/routes/claim.tsx
```

**Issue 3: Status Badge Mismatch**

```typescript
// ❌ PROBLEM: Missing hasWinner for COMPLETED status
generateMockLotteries(50, { status: "ENDED" }); // Shows "Revenue Sent"

// ✅ SOLUTION: Add hasWinner: true
generateMockLotteries(50, { status: "ENDED", hasWinner: true }); // Shows "Completed"
```

**Issue 4: Pagination Button State**

```typescript
// ❌ PROBLEM: Checking visibility instead of state
await expect(claimPage.nextPageButton).toBeHidden(); // Pagination always visible!

// ✅ SOLUTION: Check aria-disabled attribute
const ariaDisabled = await claimPage.nextPageButton.getAttribute("aria-disabled");
expect(ariaDisabled).toBe("true");
```

**Issue 5: Network Timing**

```typescript
// ❌ PROBLEM: Not waiting for network idle
await claimPage.goto();
const rowCount = await claimPage.getTableRowCount(); // May be 0!

// ✅ SOLUTION: Wait for network idle
await claimPage.goto();
await page.waitForLoadState("networkidle");
const rowCount = await claimPage.getTableRowCount();
```

### Debugging Mock Tests

**✅ BEST: Enable Debug Mode**

```bash
DEBUG=pw:api pnpm test:e2e --grep "@mock" --headed
```

**✅ BEST: Inspect Network Requests**

```typescript
page.on("request", (request) => {
  if (request.url().includes("graphql")) {
    console.log("GraphQL Request:", request.postDataJSON());
  }
});

page.on("response", (response) => {
  if (response.url().includes("graphql")) {
    console.log("GraphQL Response:", response.status());
  }
});
```

**✅ BEST: Verify Mock Handler Execution**

```typescript
getLotteriesHandler: (variables: Record<string, unknown>) => {
  console.log("Mock handler called with:", variables);
  const skip = (variables.skip as number) || 0;
  const page = Math.floor(skip / PAGE_SIZE) + 1;
  console.log("Returning page:", page);
  const lotteries = generatePagedMockLotteries(page, PAGE_SIZE, 125, {});
  return createMockLotteriesResponse(lotteries);
};
```

---

## ✅ Mock Testing Checklist

### Before Writing Mock Tests

- [ ] **Understand App Behavior**: Know how the UI actually works
- [ ] **Identify Test Scenarios**: List all edge cases and states
- [ ] **Create Mock Scenarios**: Define scenarios in centralized file
- [ ] **Match App Configuration**: Ensure PAGE_SIZE and constants match
- [ ] **Plan Test Coverage**: Pagination, filtering, empty states, errors

### During Test Development

- [ ] **Use `@mock` Tag**: Every mock test has `{ tag: "@mock" }`
- [ ] **Follow AAA Pattern**: Clear Arrange-Act-Assert structure
- [ ] **Setup Mock First**: Call `mockAPI()` before navigation
- [ ] **Wait for Network**: Use `waitForLoadState("networkidle")`
- [ ] **Test One Behavior**: Each test validates one specific thing
- [ ] **Add State Checks**: Use `isEnabled()` methods for conditional logic
- [ ] **Verify Button States**: Check `aria-disabled` attribute
- [ ] **Use test.step()**: Organize assertions into logical steps

### After Test Implementation

- [ ] **Run Tests Locally**: Verify all tests pass
- [ ] **Run in Headless Mode**: Ensure headless compatibility
- [ ] **Check Performance**: Tests should be < 10 seconds
- [ ] **Review Test Output**: No warnings or errors
- [ ] **Validate Coverage**: All scenarios tested
- [ ] **Document Scenarios**: Clear scenario names and purposes

### Code Quality Standards

- [ ] **No Debug Logs**: Remove all `console.log` statements
- [ ] **No Comments**: Write self-documenting code
- [ ] **Type Safety**: Use TypeScript types for all data
- [ ] **Reusable Scenarios**: Use centralized mock scenarios
- [ ] **Clean Assertions**: Clear, focused expectations
- [ ] **Error Messages**: Helpful failure messages

---

## 🎯 Mock Testing Success Metrics

### Quality Indicators

**✅ Excellent Mock Test Suite:**

- **100% Pass Rate**: All tests pass consistently
- **< 5 seconds per test**: Fast execution
- **Zero Flakiness**: Same results every run
- **Complete Coverage**: All UI states tested
- **Clear Failures**: Easy to understand what broke
- **Maintainable**: Easy to add new scenarios

**❌ Poor Mock Test Suite:**

- Intermittent failures
- Slow execution (> 10 seconds per test)
- Hardcoded data
- Missing edge cases
- Unclear failure messages
- Difficult to maintain

### Coverage Goals

**Mock tests should cover:**

- ✅ **Pagination**: First page, middle pages, last page, single page, empty
- ✅ **Status Filtering**: All status types, combinations, empty results
- ✅ **Edge Cases**: Partial pages, rapid clicks, browser navigation
- ✅ **Empty States**: No data, disabled buttons, empty messages
- ✅ **URL Parameters**: Page numbers, status filters, persistence

**Mock tests should NOT cover:**

- ❌ Blockchain transactions
- ❌ Wallet integration
- ❌ Subgraph indexing
- ❌ Real API responses
- ❌ Multi-layer data consistency

---

## 📚 Summary: Mock vs E2E Testing Strategy

### Use Mock Tests For:

1. **UI Behavior Validation**: Pagination, filtering, sorting
2. **Edge Case Testing**: Empty states, partial pages, errors
3. **Fast Feedback**: CI/CD, local development
4. **Deterministic Testing**: Controlled scenarios
5. **UI-Only Features**: Component behavior, state management

### Use E2E Tests For:

1. **Blockchain Integration**: Contract calls, transactions
2. **Wallet Workflows**: Connection, approvals, signatures
3. **Data Consistency**: Blockchain ↔ Subgraph ↔ UI
4. **Critical Paths**: End-to-end user journeys
5. **Real-World Scenarios**: Actual network conditions

### Optimal Testing Strategy:

**Pyramid Approach:**

```
        /\
       /  \      E2E Tests (10%)
      /    \     - Critical paths
     /------\    - Wallet integration
    /        \   - Multi-layer validation
   /          \
  /------------\ Mock Tests (40%)
 /              \ - UI behavior
/                \ - Edge cases
/------------------\ - Fast feedback

                    Unit Tests (50%)
                    - Component logic
                    - Utility functions
                    - Business logic
```

**Combined Coverage:**

- **Unit Tests**: 50% of tests, < 1 second each
- **Mock Tests**: 40% of tests, < 5 seconds each
- **E2E Tests**: 10% of tests, 1-2 minutes each

**Result**: Comprehensive coverage with fast feedback and high confidence

---

**End of Mock-Driven Testing Guide** 🎉
