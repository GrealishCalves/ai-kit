---
type: "agent_requested"
description: "Example description"
---

# Local Testing Principles & Patterns

**Purpose**: Universal guide for writing and maintaining production-grade Hardhat tests

---

## 🎯 Document Purpose

This document establishes **universal testing standards** for smart contract testing with Hardhat + TypeScript + Viem. It serves as the **single source of truth** for writing local tests that:

1. **Prevent false negatives** - Tests fail when contracts are broken
2. **Ensure financial precision** - Exact wei-level calculations
3. **Maintain consistency** - All tests follow identical patterns
4. **Eliminate anti-patterns** - Documented mistakes are never repeated

**Scope**: Local Hardhat testing with MockEntropy (production uses Pyth Entropy VRF)

---

## Core Philosophy

**Tests must prove correctness through adversarial validation, not just verify happy paths.**

Every test follows these immutable principles:

1. **Assume contracts are broken** - Tests must prove they work
2. **Zero tolerance for false negatives** - Passing tests = genuine correctness
3. **Native-first approach** - Use framework APIs directly
4. **Financial precision** - Exact wei-level calculations, no approximations
5. **Complete state validation** - Verify ALL affected parties and state changes
6. **Simplicity over complexity** - 3 tickets prove the same concept as 100 tickets (Added 2025-01-15)

---

## Critical Contract Behaviors (Must Know)

### Lottery Ends on First Win

**CRITICAL**: Once a winner is found, the lottery status becomes `STATUS_ENDED` and `hasWinner = true`. All subsequent ticket purchases are **refunded**, not processed.

```solidity
// ProductionLottery.sol - _entropyCallback
if (L.hasWinner) {
    uint256 refundAmount = uint256(cachedTicketPrice) * cachedTicketCount;
    IERC20(cachedPrizeToken).safeTransfer(cachedPlayer, refundAmount);
    emit RefundProcessed(...);
}
```

**Impact on Tests:**

```typescript
// ❌ WRONG: Expecting 100 tickets to be processed
for (let i = 0; i < 100; i++) {
  await buyTicketsNative(...);
}
// If ticket #5 wins, tickets #6-100 are refunded, not processed!

// ✅ CORRECT: Use deterministic VRF to control when lottery ends
await buyTicketsNative(..., [1], { skipVRFFulfillment: true }); // Loss
await buyTicketsNative(..., [1], { skipVRFFulfillment: true }); // Loss
await buyTicketsNative(..., [2], { skipVRFFulfillment: true }); // Will win
// Manually fulfill: 2 losses, then 1 win
```

### Each Ticket Purchase Creates Separate VRF Request

**CRITICAL**: Each `buyTicketsNative` call creates a unique VRF request with its own sequence number. You must fulfill ALL requests individually.

```typescript
// ❌ WRONG: Only fulfilling last request
for (let i = 0; i < 10; i++) {
  await buyTicketsNative(..., { skipVRFFulfillment: true });
}
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
await fulfillMockVRFWithWin(..., seq, ...); // Only processes 1 ticket!

// ✅ CORRECT: Fulfill all requests
await buyTicketsNative(..., { skipVRFFulfillment: true }); // seq-2
await buyTicketsNative(..., { skipVRFFulfillment: true }); // seq-1
await buyTicketsNative(..., { skipVRFFulfillment: true }); // seq
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
await fulfillMockVRFWithLoss(..., seq - 2n, 100);
await fulfillMockVRFWithLoss(..., seq - 1n, 100);
await fulfillMockVRFWithWin(..., seq, ...);
```

### Tier-Based Pick Range Validation

**CRITICAL**: Max pick range is calculated based on prize amount tier.

```typescript
// Tier calculation:
const normalizedPrize = (prizeAmount + 999_999) / 1_000_000; // Ceiling division

// Tier 1: ≤100,000 USDC → max = 1× normalized prize
// Tier 2: 100,000-1,000,000 USDC → max = 2× normalized prize
// Tier 3: ≥1,000,000 USDC → max = 3× normalized prize

// ❌ WRONG: Pick range exceeds tier limit
const prizeAmount = 7_777_777n; // ~7.78 USDC
const normalizedPrize = 8; // (7_777_777 + 999_999) / 1_000_000
const maxPickRange = 8 * 1; // Tier 1
await createLotteryNative(..., { prizeAmount, pickRange: 10 }); // FAILS: InvalidPickRange()

// ✅ CORRECT: Pick range within tier limit
await createLotteryNative(..., { prizeAmount, pickRange: 8 }); // PASSES
```

---

## AAA Pattern (Arrange-Act-Assert)

**100% MANDATORY** - All tests MUST follow strict AAA separation.

### Structure

```typescript
it("should perform specific operation", async function () {
  this.timeout(60000);

  // ARRANGE: Setup only - ZERO assertions allowed
  const { contracts, publicClient, viem } = await setupLocalEnvironment();
  const walletClients = await viem.getWalletClients();
  const owner = walletClients[0];
  const provider = walletClients[1];
  const player = walletClients[2];

  await setupTokensForWallet(contracts, publicClient, owner, provider, 20_000_000n);
  await setupTokensForWallet(contracts, publicClient, owner, player, 1_000_000n);

  // ACT: Single operation under test
  const { lotteryId } = await createLotteryNative(contracts, publicClient, provider, {
    prizeAmount: 10_000_000n,
    ticketPrice: 100_000n,
    pickRange: 10,
    duration: 3600,
  });

  // ASSERT: ALL validations here only
  const lotteryInfo = await contracts.lottery.read.getLotteryInfo([lotteryId]);
  expect(lotteryInfo.prizeAmount).to.equal(10_000_000n);
  expect(lotteryInfo.ticketPrice).to.equal(100_000n);
  expect(lotteryInfo.status).to.equal(LOTTERY_STATUS.ACTIVE);
});
```

### Critical Rules

**ARRANGE Phase:**

- ✅ Setup wallets, tokens, contracts
- ✅ Call helper functions (createLotteryNative, setupTokensForWallet)
- ❌ **NEVER** add assertions
- ❌ **NEVER** validate intermediate state
- ❌ **NEVER** check if setup worked (trust helpers)

**ACT Phase:**

- ✅ Single operation being tested
- ✅ Capture transaction hash/receipt if needed
- ❌ **NEVER** multiple operations (split into separate tests)

**ASSERT Phase:**

- ✅ Validate ALL affected state changes
- ✅ Check balances for ALL parties (provider, player, treasury, platform)
- ✅ Verify events with exact parameters
- ✅ Use exact values (no ranges, no approximations)
- ✅ Add descriptive assertion messages

---

## Transaction Confirmation Pattern

**CRITICAL**: Always use Viem's native `confirmations` parameter.

### Confirmation Strategy

```typescript
// ✅ CORRECT: Critical state changes (lottery creation)
const receipt = await publicClient.waitForTransactionReceipt({
  hash: createTx,
  confirmations: 1, // Local Hardhat: always use 1
});

// ✅ CORRECT: Standard transactions (ticket purchase, VRF callback)
const receipt = await publicClient.waitForTransactionReceipt({
  hash: buyTx,
  confirmations: 1,
});

// ❌ NEVER: Arbitrary time delays
await new Promise((resolve) => setTimeout(resolve, 2000)); // FORBIDDEN

// ❌ NEVER: Missing confirmations parameter
await publicClient.waitForTransactionReceipt({ hash: tx }); // INCOMPLETE
```

### Why confirmations: 1 for Local Tests

**Local Hardhat Network Behavior:**

- Uses auto-mining (mines blocks on demand)
- `confirmations: 2` waits for second block → never mines without new transaction → timeout
- `confirmations: 1` waits for transaction's own block → instant completion
- **Performance**: 120x faster (60s → 0.5s per test)

**Evidence**: All 271 passing tests use `confirmations: 1`

---

## Wallet Separation Pattern

**MANDATORY**: Always use separate wallets for different roles.

### Standard Wallet Assignment

```typescript
const walletClients = await viem.getWalletClients();
const owner = walletClients[0]; // Token owner (can mint)
const provider = walletClients[1]; // Lottery creator (funds prize)
const player = walletClients[2]; // Ticket buyer
const player2 = walletClients[3]; // Additional player (for multi-player tests)
```

### Token Distribution Pattern

**CRITICAL RULE**: Only the contract deployer (owner) can mint tokens. All other wallets receive tokens via transfer.

```typescript
// ✅ CORRECT: Realistic token flow (Owner mints → transfers to participants)
await setupTokensForWallet(contracts, publicClient, owner, provider, 20_000_000n);
await setupTokensForWallet(contracts, publicClient, owner, player, 1_000_000n);

// ❌ WRONG: Direct minting to participants (unrealistic)
await contracts.token.write.mint([provider.account.address, 20_000_000n]);
// This will FAIL - only owner can mint!

// ❌ WRONG: Provider or player trying to mint
await contracts.token.write.mint([player.account.address, 1_000_000n], {
  account: player.account,
});
// This will FAIL - only owner has minting privileges!
```

**Rationale**:

1. Mimics production where token owner distributes tokens to users
2. Only contract deployer has minting privileges (Ownable pattern)
3. Ensures tests validate realistic token flows
4. Prevents confusion about who can perform token operations

**setupTokensForWallet Implementation**:

```typescript
// Helper internally does: owner.mint() → owner.transfer(target)
export async function setupTokensForWallet(contracts: any, publicClient: any, ownerWallet: any, targetWallet: any, amount: bigint): Promise<void> {
  // Step 1: Owner mints tokens to themselves
  const mintTx = await contracts.token.write.mint([ownerWallet.account.address, amount], {
    account: ownerWallet.account,
  });
  await publicClient.waitForTransactionReceipt({ hash: mintTx, confirmations: 1 });

  // Step 2: Owner transfers to target wallet
  const transferTx = await contracts.token.write.transfer([targetWallet.account.address, amount], {
    account: ownerWallet.account,
  });
  await publicClient.waitForTransactionReceipt({ hash: transferTx, confirmations: 1 });
}
```

---

## Financial Precision Standards

**ZERO TOLERANCE** for approximations in financial calculations.

### Exact Value Assertions

```typescript
// ✅ CORRECT: Exact wei-level precision
const expectedPlatformFee = (ticketCost * 500n) / 10_000n; // 5%
const expectedNetRevenue = (ticketCost * 9_500n) / 10_000n; // 95%

expect(treasuryBalanceAfter - treasuryBalanceBefore).to.equal(expectedPlatformFee);
expect(providerBalanceAfter - providerBalanceAfterCreate).to.equal(expectedNetRevenue);

// ❌ FORBIDDEN: Tolerance-based assertions
expect(balance).to.be.closeTo(expected, 1000); // NEVER
expect(balance).to.be.approximately(expected, 0.01); // NEVER

// ❌ FORBIDDEN: Range-based assertions for exact values
expect(balance).to.be.greaterThan(0); // Too vague
expect(status).to.be.greaterThanOrEqual(0); // Accepts wrong values
```

### Complete State Validation

```typescript
// ✅ CORRECT: Validate ALL affected parties
const treasuryBalanceBefore = await contracts.token.read.balanceOf([treasuryAddress]);
const providerBalanceBefore = await contracts.token.read.balanceOf([provider.account.address]);
const playerBalanceBefore = await contracts.token.read.balanceOf([player.account.address]);

// ... perform operation ...

const treasuryBalanceAfter = await contracts.token.read.balanceOf([treasuryAddress]);
const providerBalanceAfter = await contracts.token.read.balanceOf([provider.account.address]);
const playerBalanceAfter = await contracts.token.read.balanceOf([player.account.address]);

expect(treasuryBalanceAfter - treasuryBalanceBefore).to.equal(expectedPlatformFee);
expect(providerBalanceAfter - providerBalanceBefore).to.equal(expectedNetRevenue);
expect(playerBalanceAfter - playerBalanceBefore).to.equal(-ticketCost);

// ❌ WRONG: Partial validation (only one party)
expect(playerBalanceAfter).to.equal(playerBalanceBefore - ticketCost); // Incomplete
```

---

## Time Manipulation Safety

**CRITICAL**: Time can only move forward (mainnet equivalence).

### Safe Time Manipulation

```typescript
// ✅ SAFE: Forward time movement
export async function setTime(timestamp: number, publicClient: any): Promise<void> {
  const currentBlock = await publicClient.getBlock({ blockTag: "latest" });
  const currentTimestamp = Number(currentBlock.timestamp);

  // SAFETY CHECK: Prevent unrealistic scenarios
  if (timestamp < currentTimestamp) {
    throw new Error(`Cannot set time backward: current=${currentTimestamp}, target=${timestamp}. ` + `This would create an unrealistic scenario that can't happen on mainnet.`);
  }

  await publicClient.request({
    method: "evm_setNextBlockTimestamp",
    params: [timestamp],
  });
  await publicClient.request({
    method: "evm_mine",
    params: [],
  });
}

// ✅ SAFE: Increase time by duration
export async function increaseTime(seconds: number, publicClient: any): Promise<void> {
  await publicClient.request({
    method: "evm_increaseTime",
    params: [seconds],
  });
  await publicClient.request({
    method: "evm_mine",
    params: [],
  });
}
```

### Mainnet Equivalence Table

| Test Action            | Mainnet Equivalent       | Contract Sees                   | Risk Score |
| ---------------------- | ------------------------ | ------------------------------- | ---------- |
| `increaseTime(3600)`   | Wait 1 hour              | `block.timestamp` += 3600       | 0/10 ✅    |
| `setTime(endTime - 5)` | Wait until 5s before end | `block.timestamp` = endTime - 5 | 0/10 ✅    |
| `mine()`               | New block mined          | New block                       | 0/10 ✅    |

### Usage Example

```typescript
// Test ticket purchase 2 seconds before lottery end
const lotteryInfo = await contracts.lottery.read.getLotteryInfo([lotteryId]);
const endTime = Number(lotteryInfo.endTime);
await setTime(endTime - 2, publicClient); // Jump to edge case timing
await buyTicketsNative(contracts, publicClient, player, lotteryId, [5], {
  skipVRFFulfillment: true,
});
```

---

## VRF Manipulation Patterns

**PRINCIPLE**: Control VRF outcomes for deterministic testing while maintaining diversity.

### Critical VRF Rules (Updated 2025-01-15)

**RULE 1: Automatic VRF Fulfillment is Non-Deterministic**

```typescript
// ❌ WRONG: This has random win/loss outcome
await buyTicketsNative(contracts, publicClient, player, lotteryId, [1]);
// With pickRange=2, this has 50% chance of winning!

// ✅ CORRECT: Always use skipVRFFulfillment for deterministic tests
await buyTicketsNative(contracts, publicClient, player, lotteryId, [1], {
  skipVRFFulfillment: true,
});
```

**RULE 2: Use pickRange=100 for Reliable Loss Fulfillment**

```typescript
// ❌ WRONG: Small pickRange generates out-of-range losing numbers
await fulfillMockVRFWithLoss(contracts.mockEntropy, publicClient, player, seq, 2);
// pickRange=2 → losing number = 2/2 + 4 = 5 (out of range!)

// ✅ CORRECT: Use pickRange=100 for reliable loss generation
await fulfillMockVRFWithLoss(contracts.mockEntropy, publicClient, player, seq, 100);
// pickRange=100 → losing number = 100/2 + 4 = 54 (reliable loss)
```

**RULE 3: Understand fulfillMockVRFWithLoss Signature**

```typescript
// Function signature:
fulfillMockVRFWithLoss(
  mockEntropy: any,
  publicClient: any,
  walletClient: any,
  sequenceNumber: bigint,
  pickRange: number,        // NOT the losing number!
  losingNumberOffset = 0
)

// How it calculates losing number:
const baseLosingNumber = Math.floor(pickRange / 2) + 4;
const losingNumber = baseLosingNumber + losingNumberOffset;
```

### VRF Helper Functions

```typescript
// ✅ Controlled win scenario
await buyTicketsNative(contracts, publicClient, player, lotteryId, [7], {
  skipVRFFulfillment: true,
});
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
await fulfillMockVRFWithWin(contracts.mockEntropy, publicClient, player, seq, 7, 10);

// ✅ Controlled loss scenario (ALWAYS use pickRange=100)
await buyTicketsNative(contracts, publicClient, player, lotteryId, [5], {
  skipVRFFulfillment: true,
});
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
await fulfillMockVRFWithLoss(contracts.mockEntropy, publicClient, player, seq, 100);

// ✅ Random value (property-based testing)
await buyTicketsNative(contracts, publicClient, player, lotteryId, [randomNumber], {
  skipVRFFulfillment: true,
});
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
await fulfillMockVRFWithRandomValue(contracts.mockEntropy, publicClient, player, seq);
```

### Deterministic Multi-Ticket Pattern (Recommended)

**PRINCIPLE**: Keep tests simple - 2-3 tickets prove the same concept as 100 tickets.

```typescript
// ✅ CORRECT: Simple, deterministic, maintainable
const { lotteryId } = await createLotteryNative(contracts, publicClient, provider, {
  prizeAmount: 2_000_000n,
  ticketPrice: 1_000_000n,
  pickRange: 2,
  duration: 3600,
});

// Buy 2 losing tickets
await buyTicketsNative(contracts, publicClient, player, lotteryId, [1], {
  skipVRFFulfillment: true,
});
await buyTicketsNative(contracts, publicClient, player, lotteryId, [1], {
  skipVRFFulfillment: true,
});

// Buy 1 winning ticket
await buyTicketsNative(contracts, publicClient, player, lotteryId, [2], {
  skipVRFFulfillment: true,
});

// Fulfill all VRF requests deterministically
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
await fulfillMockVRFWithLoss(contracts.mockEntropy, publicClient, player, seq - 2n, 100);
await fulfillMockVRFWithLoss(contracts.mockEntropy, publicClient, player, seq - 1n, 100);
await fulfillMockVRFWithWin(contracts.mockEntropy, publicClient, player, seq, 2, 2);

// Validate: 3 tickets × 0.95 USDC = 2.85 USDC revenue - 2 USDC prize = 0.85 USDC profit
const providerNetChange = providerBalanceAfter - providerBalanceBefore;
expect(providerNetChange).to.equal(850_000n);
expect(providerNetChange).to.be.greaterThan(0n);
```

**Why This Pattern Works:**

- ✅ Deterministic: All outcomes controlled
- ✅ Simple: Easy to understand and maintain
- ✅ Fast: Only 3 VRF fulfillments
- ✅ Valuable: Still proves provider profitability concept
- ✅ Reliable: No random failures

### VRF Diversity Testing

**CRITICAL**: Test multiple random values that produce same outcome.

```typescript
// ✅ CORRECT: Test same winning number with different random values
for (let variant = 0; variant < 3; variant++) {
  await buyTicketsNative(contracts, publicClient, player, lotteryId, [5], {
    skipVRFFulfillment: true,
  });
  const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
  await fulfillMockVRFWithWin(contracts.mockEntropy, publicClient, player, seq, 5, 10, variant);

  // variant=0 → randomValue=4   → (4 % 10) + 1 = 5 ✅
  // variant=1 → randomValue=14  → (14 % 10) + 1 = 5 ✅
  // variant=2 → randomValue=24  → (24 % 10) + 1 = 5 ✅

  const lotteryInfo = await contracts.lottery.read.getLotteryInfo([lotteryId]);
  expect(lotteryInfo.hasWinner).to.be.true;
}
```

---

## Event Validation Standards

**PRINCIPLE**: Validate exact event counts and all parameters.

### Exact Event Count Validation

```typescript
// ✅ CORRECT: Exact count
const logs = await publicClient.getLogs({
  address: contracts.lottery.address,
  event: parseAbiItem("event LotteryCreated(...)"),
  fromBlock: "earliest",
});
expect(logs).to.have.lengthOf(1); // Exact count

// ❌ WRONG: Vague existence check
expect(logs.length).to.be.greaterThan(0); // Too vague
```

### Complete Parameter Validation

```typescript
// ✅ CORRECT: Validate ALL event parameters
const event = logs[0];
expect(event.args.lotteryId).to.equal(lotteryId);
expect(event.args.prizeProvider).to.equal(provider.account.address);
expect(event.args.prizeAmount).to.equal(10_000_000n);
expect(event.args.ticketPrice).to.equal(100_000n);
expect(event.args.pickRange).to.equal(10n);

// ❌ WRONG: Partial validation
expect(event.args.lotteryId).to.equal(lotteryId); // Only one field
```

---

## Entity Tracking Patterns

**PRINCIPLE**: Use event-driven detection for accurate entity tracking.

### Lottery ID Detection

```typescript
// ✅ CORRECT: Event-driven lottery ID (from createLotteryNative helper)
const { lotteryId, hash } = await createLotteryNative(contracts, publicClient, provider, {
  prizeAmount: 10_000_000n,
  ticketPrice: 100_000n,
  pickRange: 10,
  duration: 3600,
});
// Helper reads ID from actual LotteryCreated event

// ❌ WRONG: Counter-based ID (race condition risk)
await contracts.lottery.write.createLottery([...]);
const lotteryId = await contracts.lottery.read.lotteryCounter(); // May be wrong if concurrent
```

### VRF Sequence Number Detection

```typescript
// ✅ CORRECT: Event-driven sequence number
await buyTicketsNative(contracts, publicClient, player, lotteryId, [5], {
  skipVRFFulfillment: true,
});
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
// Helper reads from actual VRFRequested event

// ❌ WRONG: Assuming sequence numbers
const seq = 1n; // May be wrong if multiple requests
```

---

## Provider vs Player vs Platform Entities

**CRITICAL**: Understand and validate revenue flows for each entity.

### Entity Definitions

1. **Provider** (Lottery Creator)
   - Funds prize pool
   - Receives 95% of ticket revenue (net revenue)
   - Wallet: `walletClients[1]`

2. **Player** (Ticket Buyer)
   - Pays ticket cost
   - Receives prize if wins
   - Receives refund if lottery ends without winner
   - Wallet: `walletClients[2]`, `walletClients[3]`, etc.

3. **Platform** (Treasury)
   - Receives 5% of ticket revenue (platform fee)
   - Contract address: `await contracts.lottery.read.treasury()`

### Revenue Flow Validation

```typescript
// ✅ CORRECT: Complete revenue flow validation
const treasuryAddress = await contracts.lottery.read.treasury();
const treasuryBalanceBefore = await contracts.token.read.balanceOf([treasuryAddress]);
const providerBalanceBefore = await contracts.token.read.balanceOf([provider.account.address]);
const playerBalanceBefore = await contracts.token.read.balanceOf([player.account.address]);

const { lotteryId } = await createLotteryNative(contracts, publicClient, provider, {
  prizeAmount: 100_000_000n,
  ticketPrice: 1_000_000n,
  pickRange: 100,
  duration: 3600,
});

const providerBalanceAfterCreate = await contracts.token.read.balanceOf([provider.account.address]);

await buyTicketsNative(contracts, publicClient, player, lotteryId, [50], {
  skipVRFFulfillment: true,
});

const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
await fulfillMockVRFWithLoss(contracts.mockEntropy, publicClient, player, seq, 99, 100);

const treasuryBalanceAfter = await contracts.token.read.balanceOf([treasuryAddress]);
const providerBalanceAfter = await contracts.token.read.balanceOf([provider.account.address]);
const playerBalanceAfter = await contracts.token.read.balanceOf([player.account.address]);

// ASSERT: Validate complete revenue distribution
const ticketCost = 1_000_000n;
const expectedPlatformFee = (ticketCost * 500n) / 10_000n; // 5%
const expectedNetRevenue = (ticketCost * 9_500n) / 10_000n; // 95%

expect(treasuryBalanceAfter - treasuryBalanceBefore).to.equal(expectedPlatformFee);
expect(providerBalanceAfter - providerBalanceAfterCreate).to.equal(expectedNetRevenue);
expect(playerBalanceAfter - playerBalanceBefore).to.equal(-ticketCost);

// Verify conservation: platform fee + net revenue = ticket cost
expect(expectedPlatformFee + expectedNetRevenue).to.equal(ticketCost);
```

---

## Mainnet Equivalence Guardrails

**RULE**: Local manipulations must be indistinguishable from Base mainnet behavior. If contracts can observe a test-only shortcut, we are hiding real bugs.

### Allowed Actions (Mirror Production)

- Interact through public/external functions only; drive all state changes via transactions
- Advance time or blocks with `increaseTime`/`hardhat_mine` to simulate on-chain progress
- Use helper utilities that wrap real calls (`setupTokensForWallet`, `buyTicketsNative`, VRF helpers)
- Pay VRF fees exactly as required (`{ value: fee }`) even on deterministic runs
- Configure wallets and balances through owner mint → transfer flows (never magic value injections)

### Forbidden Shortcuts (Break Mainnet Equivalence)

- `hardhat_setStorageAt`, `hardhat_setBalance`, or any direct state mutation without contract interaction
- Manually setting lottery status, counters, or balances through scripts or Hardhat console
- Bypassing VRF fees (e.g., forcing zero-value requests) or refunding without invoking contract logic
- Calling internal functions like `_entropyCallback` directly from tests
- Re-using cached reads after mutations instead of fetching fresh chain state

**Impact**: Previous regressions came from skipping VRF fees and altering state directly. Keeping tests mainnet-equivalent ensures those bugs stay detectable.

---

## MockEntropy Control Pattern

**Purpose**: Provide deterministic VRF outcomes while keeping `ProductionLottery` logic under test.

### Core Usage

```typescript
// Force deterministic win without touching internal callbacks
await buyTicketsNative(contracts, publicClient, player, lotteryId, [7], {
  winningNumber: 7,
});

// Pause fulfillment when you need to inspect pending state
await buyTicketsNative(contracts, publicClient, player, lotteryId, [12], {
  skipVRFFulfillment: true,
});

const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
await fulfillMockVRFWithLoss(contracts.mockEntropy, publicClient, provider, seq, 42, 100);
```

### Guardrails

- ✅ Treat MockEntropy as the VRF oracle: configure outcomes, then assert on `ProductionLottery` effects
- ✅ Read sequence numbers/events via helpers (never guess hard-coded values)
- ✅ Use manual fulfillment only when tests require custom timing
- ❌ Do NOT write assertions against MockEntropy storage/events; focus on contract state that matters to users
- ❌ Do NOT skip fee payment or craft impossible sequences; helpers already mirror production flows
- ❌ Do NOT call `_entropyCallback` directly or craft fake receipts—let MockEntropy drive the callback

**Goal**: Deterministic randomness without masking bugs. Every MockEntropy interaction must reflect a scenario the real Pyth oracle could produce.

---

## VRF Fee Handling

**CRITICAL**: MockEntropy mimics Pyth Entropy fee behavior (not zero fees).

### VRF Fee Validation

```typescript
// ✅ CORRECT: Validate VRF fee payment
const vrfFee = await contracts.lottery.read.getVRFFee();

const playerEthBalanceBefore = await publicClient.getBalance({
  address: player.account.address,
});

await buyTicketsNative(contracts, publicClient, player, lotteryId, [5], {
  skipVRFFulfillment: true,
});

const playerEthBalanceAfter = await publicClient.getBalance({
  address: player.account.address,
});

// Player pays VRF fee in ETH (not USDC)
expect(playerEthBalanceBefore - playerEthBalanceAfter).to.be.greaterThan(vrfFee);
// Note: Actual cost includes gas, so use greaterThan for ETH balance checks
```

### VRF Fee in Tests

```typescript
// MockEntropy charges fee (mimics Pyth Entropy)
const vrfFee = await contracts.mockEntropy.read.getFee([contracts.lottery.address]);
expect(vrfFee).to.be.greaterThan(0n); // Not zero

// buyTicketsNative automatically handles VRF fee payment
await buyTicketsNative(contracts, publicClient, player, lotteryId, [5]);
// Helper sends correct ETH value for VRF fee
```

---

## Block and Time Manipulation

**PRINCIPLE**: Use fresh timestamp calculations to avoid stale values.

### Fresh Timestamp Pattern

```typescript
// ✅ CORRECT: Fresh timestamp calculation
const currentBlock = await publicClient.getBlock({ blockTag: "latest" });
const targetTime = Number(currentBlock.timestamp) + 3600;
await setTime(targetTime, publicClient);

// ❌ WRONG: Stale timestamp (other operations may mine blocks)
const currentTime = await time.latest();
const targetTime = currentTime + 3600;
// ... other operations that mine blocks ...
await setTime(targetTime, publicClient); // targetTime is now stale!
```

### Block Mining

```typescript
// ✅ CORRECT: Mine blocks when needed
await publicClient.request({
  method: "hardhat_mine",
  params: ["0x1"], // Mine 1 block
});

// ✅ CORRECT: Mine multiple blocks
await publicClient.request({
  method: "hardhat_mine",
  params: ["0x5"], // Mine 5 blocks
});
```

---

## Test Isolation and Setup

**PRINCIPLE**: Each test is completely independent.

### Per-Test Setup

```typescript
// ✅ CORRECT: Each test calls setupLocalEnvironment
it("should perform operation", async function () {
  this.timeout(60000);

  const { contracts, publicClient, viem } = await setupLocalEnvironment();
  // ... test logic ...
});

// ❌ WRONG: Shared setup in before() hook
let contracts, publicClient, viem;
before(async () => {
  const env = await setupLocalEnvironment();
  contracts = env.contracts;
  publicClient = env.publicClient;
  viem = env.viem;
});
```

**Rationale**: Avoids Node.js ESM module loading issues and ensures true test independence.

### Test Timeout

```typescript
// ✅ CORRECT: Explicit timeout for each test
it("should perform operation", async function () {
  this.timeout(60000); // 60 seconds
  // ... test logic ...
});

// ❌ WRONG: No timeout (uses default 2 seconds)
it("should perform operation", async () => {
  // ... test logic ...
});
```

---

## Error Validation Patterns

**PRINCIPLE**: Use specific custom error assertions, never generic reverts.

### Custom Error Assertions

```typescript
// ✅ CORRECT: Specific custom error with viem.assertions
await viem.assertions.revertWithCustomError(
  contracts.lottery.write.createLottery([...], { account: player.account }),
  contracts.lottery,
  "InvalidPrizeAmount"
);

// ✅ CORRECT: Custom error with arguments
await viem.assertions.revertWithCustomError(
  contracts.lottery.write.buyTicketsPackedNoReferral([lotteryId, packedNumbers, emptyPermit], {
    account: player.account,
  }),
  contracts.lottery,
  "LotteryNotActive",
  [lotteryId]
);

// ❌ WRONG: Generic revert
await expect(
  contracts.lottery.write.createLottery([...])
).to.be.reverted; // Too vague

// ❌ WRONG: String matching
await expect(
  contracts.lottery.write.createLottery([...])
).to.be.revertedWith("Invalid prize amount"); // Fragile
```

---

## False Negative Prevention

**PRINCIPLE**: Tests must fail when contracts are broken.

### Break Testing Validation

Before trusting a test, verify it can detect bugs:

1. **Intentionally break the contract** (temporarily modify contract code)
2. **Run the test** - it MUST fail
3. **Restore the contract** - test MUST pass
4. **Document the validation** - test is now trusted

### The False Negative Anti-Pattern (CRITICAL)

**Definition**: A test that passes by verifying a function fails, without ever testing if the function can succeed.

**Why it's dangerous**:

1. ✅ Tests pass (green checkmarks everywhere)
2. ✅ Code coverage shows 100% (all lines executed)
3. ❌ Function is completely useless (always reverts)
4. ❌ No one notices because tests are green

**Example from CHA-66 Dead Code Analysis**:

```typescript
// ❌ BAD: Only tests failure paths
describe("withdrawUnawardedPrize()", () => {
  it("should reject when lottery is not expired", async () => {
    await viem.assertions.revertWithCustomError(
      contracts.lottery.write.withdrawUnawardedPrize([lotteryId]),
      contracts.lottery,
      "LotteryNotExpired"  // ← EXPECTS REVERT
    );
  });

  it("should reject when caller is not provider", async () => {
    await viem.assertions.revertWithCustomError(
      contracts.lottery.write.withdrawUnawardedPrize([lotteryId]),
      contracts.lottery,
      "NotPrizeProvider"  // ← EXPECTS REVERT
    );
  });

  it("should reject when prize amount is zero", async () => {
    await viem.assertions.revertWithCustomError(
      contracts.lottery.write.withdrawUnawardedPrize([lotteryId]),
      contracts.lottery,
      "NothingToClaim"  // ← EXPECTS REVERT
    );
  });

  // ← MISSING: "should successfully withdraw when all conditions pass"
  // This test never existed because success is mathematically impossible!
});

// ✅ GOOD: Tests both success AND failure
describe("finalizeLottery()", () => {
  it("should successfully return prize when lottery expires without winner", async () => {
    // ARRANGE
    const { lotteryId } = await createLotteryNative(...);
    await increaseTime(3601, publicClient);
    const providerBalanceBefore = await contracts.token.read.balanceOf([provider.account.address]);

    // ACT
    await contracts.lottery.write.finalizeLottery([lotteryId], {
      account: provider.account,
    });

    // ASSERT: Verify success
    const providerBalanceAfter = await contracts.token.read.balanceOf([provider.account.address]);
    expect(providerBalanceAfter - providerBalanceBefore).to.equal(prizeAmount);

    const lotteryInfo = await contracts.lottery.read.getLotteryInfo([lotteryId]);
    expect(lotteryInfo.status).to.equal(LOTTERY_STATUS.EXPIRED);
    expect(lotteryInfo.prizeAmount).to.equal(0n);
  });

  it("should reject when lottery is still active", async () => {
    await viem.assertions.revertWithCustomError(
      contracts.lottery.write.finalizeLottery([lotteryId]),
      contracts.lottery,
      "LotteryNotExpirableYet"
    );
  });
});
```

**The Smoking Gun Pattern**:

```typescript
// 🚨 THIS TEST PROVES THE FUNCTION IS DEAD CODE
it("should reject withdrawUnawardedPrize when prize already withdrawn via finalizeLottery", async () => {
  // Step 1: Call finalizeLottery (sets STATUS_EXPIRED + prizeAmount = 0)
  await contracts.lottery.write.finalizeLottery([lotteryId]);

  // Step 2: Verify prize is ZERO
  const lotteryData = await contracts.lottery.read.getLotteryInfo([lotteryId]);
  expect(lotteryData.prizeAmount).to.equal(0n); // ← Explicitly verifies ZERO

  // Step 3: Try to withdraw
  await viem.assertions.revertWithCustomError(
    contracts.lottery.write.withdrawUnawardedPrize([lotteryId]),
    contracts.lottery,
    "NothingToClaim", // ← ALWAYS reverts!
  );
});

// This test PROVES:
// 1. finalizeLottery() sets prizeAmount = 0 ✅
// 2. withdrawUnawardedPrize() requires prizeAmount > 0 ✅
// 3. Therefore, withdrawUnawardedPrize() ALWAYS fails after finalizeLottery() ✅
// 4. Since finalizeLottery() is the ONLY way to set STATUS_EXPIRED... ✅
// 5. The function is provably dead code! ✅
```

**Prevention Rule**: Every public function MUST have at least one test that verifies successful execution.

**Enforcement Checklist**:

- [ ] Does every public function have a success test?
- [ ] Are all error conditions tested?
- [ ] Do test names match actual assertions?
- [ ] Can the success condition actually be reached?

**Reference**: See `FINAL_AUDIT_REPORT.md` for complete analysis of CHA-66 dead code case study.

### Common False Negative Patterns to Avoid

```typescript
// ❌ PATTERN 1: Tolerance Trap
expect(prizeAmount).to.be.closeTo(inputAmount, inputAmount / 10n); // 10% tolerance hides bugs

// ❌ PATTERN 2: Range Deception
expect(status).to.be.greaterThanOrEqual(0); // Accepts any status ≥ 0

// ❌ PATTERN 3: Type Mirage
expect(winningNumber).to.be.a("bigint"); // Type check without business logic

// ❌ PATTERN 4: Partial Validation
expect(userBalance).to.equal(expectedUserBalance); // Missing contract balance check

// ❌ PATTERN 5: Existence Validator
expect(result).toBeTruthy(); // Tests existence, not correctness
expect(events.length).toBeGreaterThan(0); // Doesn't verify event content

// ❌ PATTERN 6: Only Testing Failure (THE MOST DANGEROUS)
describe("myFunction()", () => {
  it("should reject when condition A fails", () => { ... });
  it("should reject when condition B fails", () => { ... });
  // ← MISSING: "should succeed when all conditions pass"
});
```

### Correct Patterns

```typescript
// ✅ PATTERN 1: Exact Financial Validation
const expectedAfterFee = (inputAmount * 95n) / 100n;
expect(prizeAmount).to.equal(expectedAfterFee, "Must be exactly 95% of input");

// ✅ PATTERN 2: Exact State Validation
expect(status).to.equal(LOTTERY_STATUS.ACTIVE, "Must be exactly ACTIVE status");

// ✅ PATTERN 3: Type + Constraint + Logic Validation
expect(winningNumber).to.be.a("bigint");
expect(Number(winningNumber)).to.be.greaterThan(0);
expect(Number(winningNumber)).to.be.lessThanOrEqual(pickRange);
expect(won).to.equal(playerNumbers.includes(Number(winningNumber)));

// ✅ PATTERN 4: Complete Transaction Validation
expect(userBalance).to.equal(expectedUserBalance);
expect(contractBalance).to.equal(expectedContractBalance);
expect(totalSupply).to.equal(expectedTotalSupply);

// ✅ PATTERN 5: Exact Value Validation
expect(result.value).to.equal(expectedValue);
expect(events).to.have.lengthOf(1); // Exact count
expect(events[0].args.amount).to.equal(calculatedAmount);

// ✅ PATTERN 6: Always Test Success First
describe("myFunction()", () => {
  it("should succeed when all conditions pass", async () => {
    const result = await myFunction();
    expect(result).to.equal(expectedValue);
  });

  it("should reject when condition A fails", async () => { ... });
  it("should reject when condition B fails", async () => { ... });
});
```

---

## Test Naming Conventions

**PRINCIPLE**: Test names must clearly describe expected behavior.

### Naming Pattern

```typescript
// ✅ CORRECT: Descriptive, action-oriented names
it("should transfer exact prize when player wins with 30-minute delayed VRF callback", async () => {});
it("should deduct ticket cost when player loses with 59-minute delayed VRF callback", async () => {});
it("should reject purchase with insufficient ETH for VRF fee", async () => {});
it("should distribute exactly 5% to treasury and 95% to provider", async () => {});

// ❌ WRONG: Vague or implementation-focused names
it("handles VRF callback", async () => {}); // Too vague
it("test ticket purchase", async () => {}); // Not descriptive
it("VRF works", async () => {}); // Unclear expectation
```

### Naming Rules

1. **Always start with "should"**
2. **Describe the expected outcome**, not the action
3. **Include context** when relevant (timing, conditions, edge cases)
4. **Be specific** about what's being validated

---

## Test Organization and Structure

**PRINCIPLE**: Organize tests by domain and functionality.

### Directory Structure

```
test/local/
├── unit/                    # Unit tests (single function/feature)
│   ├── claim/              # Prize withdrawal, refunds
│   ├── finalization/       # Lottery finalization
│   ├── financial/          # Revenue distribution, token conservation
│   ├── permit/             # EIP-2612 permit functionality
│   ├── platform/           # Constructor, pending requests
│   ├── provider/           # Lottery creation
│   ├── referral/           # Referral system
│   ├── security/           # Security validations
│   ├── status/             # Status transitions
│   ├── ticket/             # Ticket purchase
│   ├── treasury/           # Treasury operations
│   ├── view/               # View functions
│   └── vrf/                # VRF callbacks, timeouts
└── integration/            # Integration tests (multi-step flows)
    ├── critical-path-happy.test.ts
    ├── critical-path-no-winner.test.ts
    ├── critical-path-timeout.test.ts
    ├── race-conditions.test.ts
    ├── mainnet-scenarios-*.test.ts
    └── ...
```

### Test File Naming

```
{domain}-{feature}-{aspect}.test.ts

Examples:
- lottery-creation-state.test.ts
- lottery-creation-validation.test.ts
- purchase-validation-vrf-fee.test.ts
- revenue-distribution.test.ts
```

---

## Helper Function Patterns

**PRINCIPLE**: Helpers are stateless, explicit, and reusable.

### Stateless Helper Design

```typescript
// ✅ CORRECT: Stateless with explicit parameters
export async function createLotteryNative(
  contracts: any,
  publicClient: any,
  providerWallet: any,
  params: {
    prizeAmount: bigint;
    ticketPrice: bigint;
    pickRange: number;
    duration: number;
  },
): Promise<{ lotteryId: bigint; hash: Hash }> {
  // Direct operations only
  const tx = await contracts.lottery.write.createLottery([...args]);
  await publicClient.waitForTransactionReceipt({ hash: tx, confirmations: 1 });
  const lotteryId = await contracts.lottery.read.lotteryCounter();
  return { lotteryId, hash: tx };
}

// ❌ WRONG: Stateful with hidden dependencies
export class LotteryHelper {
  private contracts: any; // Hidden state
  async createLottery(params: any) {
    /* uses internal state */
  }
}
```

### Helper Function Categories

1. **Setup Helpers** (`test-setup.ts`)
   - `setupLocalEnvironment()` - Environment initialization
   - `setupTokensForWallet()` - Token distribution

2. **Lottery Helpers** (`lottery-helpers.ts`)
   - `createLotteryNative()` - Lottery creation
   - `buyTicketsNative()` - Ticket purchase

3. **VRF Helpers** (`vrf-helpers.ts`)
   - `fulfillMockVRFWithWin()` - Controlled win
   - `fulfillMockVRFWithLoss()` - Controlled loss
   - `getLatestMockSequenceNumber()` - Sequence tracking

4. **Time Helpers** (`time-helpers.ts`)
   - `increaseTime()` - Time advancement
   - `setTime()` - Absolute time setting
   - `getLatestBlockTimestamp()` - Current time

---

## Performance Optimization

**PRINCIPLE**: Tests should execute quickly without sacrificing correctness.

### Performance Metrics

- **Target**: Full suite under 60 seconds
- **Current**: ~35 seconds for 271 tests
- **Average**: ~130ms per test

### Optimization Techniques

1. **Use confirmations: 1** (not 2) for local tests
2. **Minimize token minting** - Reuse owner wallet
3. **Batch operations** - Use Promise.all() for parallel reads
4. **Avoid unnecessary waits** - No setTimeout delays

```typescript
// ✅ CORRECT: Parallel reads
const [lotteryInfo, counter, balance] = await Promise.all([contracts.lottery.read.getLotteryInfo([id]), contracts.lottery.read.lotteryCounter(), contracts.token.read.balanceOf([address])]);

// ❌ WRONG: Sequential reads
const lotteryInfo = await contracts.lottery.read.getLotteryInfo([id]);
const counter = await contracts.lottery.read.lotteryCounter();
const balance = await contracts.token.read.balanceOf([address]);
```

---

## Quality Checklist

**Before committing tests, verify:**

### Per-Test Checklist

- [ ] Test name starts with "should" and describes expected outcome
- [ ] Explicit timeout set: `this.timeout(60000)`
- [ ] AAA pattern followed (no assertions in ARRANGE)
- [ ] Uses `setupLocalEnvironment()` inside test (not in before hook)
- [ ] Wallet separation: owner ≠ provider ≠ player
- [ ] Token setup uses `setupTokensForWallet()` helper
- [ ] Transaction confirmations use `confirmations: 1`
- [ ] Financial assertions are exact (no tolerance)
- [ ] All affected parties validated (provider, player, treasury)
- [ ] Events validated with exact counts and parameters
- [ ] Custom errors use `viem.assertions.revertWithCustomError`
- [ ] No console.log statements
- [ ] No code comments
- [ ] **FALSE NEGATIVE CHECK**: If testing error conditions, verify success test exists

### Per-File Checklist

- [ ] File named: `{domain}-{feature}-{aspect}.test.ts`
- [ ] All tests in file belong to same domain
- [ ] Imports use `.js` extension for ESM compatibility
- [ ] No shared state between tests
- [ ] No before/beforeEach hooks with heavy setup
- [ ] All tests pass independently
- [ ] No false negatives (tests fail when contracts broken)
- [ ] **Every public function has at least one success test**

### Test Suite Checklist

- [ ] All local tests passing
- [ ] Execution time under 60 seconds
- [ ] Zero false negative risk
- [ ] 100% AAA pattern compliance
- [ ] Complete domain coverage
- [ ] Integration tests cover critical paths
- [ ] Race condition tests validate concurrency
- [ ] Security tests validate access control
- [ ] **No describe blocks with only failure tests (must have success test)**

---

## Common Pitfalls and Solutions

### Pitfall 1: Stale Timestamp Calculations

**Problem**: Calculating timestamp before operations that mine blocks

```typescript
// ❌ WRONG
const currentTime = await time.latest();
const targetTime = currentTime + 3600;
await buyTicketsNative(...); // Mines blocks
await setTime(targetTime, publicClient); // targetTime is stale!
```

**Solution**: Calculate timestamp immediately before use

```typescript
// ✅ CORRECT
await buyTicketsNative(...);
const currentBlock = await publicClient.getBlock({ blockTag: "latest" });
const targetTime = Number(currentBlock.timestamp) + 3600;
await setTime(targetTime, publicClient);
```

### Pitfall 2: Missing Balance Snapshots

**Problem**: Capturing balances after operation instead of before

```typescript
// ❌ WRONG
await buyTicketsNative(...);
const playerBalanceBefore = await contracts.token.read.balanceOf([player.account.address]);
// Balance already changed!
```

**Solution**: Capture balances before operation

```typescript
// ✅ CORRECT
const playerBalanceBefore = await contracts.token.read.balanceOf([player.account.address]);
const providerBalanceBefore = await contracts.token.read.balanceOf([provider.account.address]);
const treasuryBalanceBefore = await contracts.token.read.balanceOf([treasuryAddress]);

await buyTicketsNative(...);

const playerBalanceAfter = await contracts.token.read.balanceOf([player.account.address]);
const providerBalanceAfter = await contracts.token.read.balanceOf([provider.account.address]);
const treasuryBalanceAfter = await contracts.token.read.balanceOf([treasuryAddress]);
```

Capture **all** affected parties up front—every bug we found here involved forgetting either the provider or treasury snapshot.

### Pitfall 3: Assuming Sequence Numbers

**Problem**: Hardcoding VRF sequence numbers

```typescript
// ❌ WRONG
await buyTicketsNative(..., { skipVRFFulfillment: true });
const seq = 1n; // Assumes first request
```

**Solution**: Read from events

```typescript
// ✅ CORRECT
await buyTicketsNative(..., { skipVRFFulfillment: true });
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
```

### Pitfall 4: Generic Error Assertions

**Problem**: Using generic revert checks

```typescript
// ❌ WRONG
await expect(
  contracts.lottery.write.createLottery([...])
).to.be.reverted;
```

**Solution**: Use specific custom errors

```typescript
// ✅ CORRECT
await viem.assertions.revertWithCustomError(
  contracts.lottery.write.createLottery([...]),
  contracts.lottery,
  "InvalidPrizeAmount"
);
```

---

## Summary of Key Principles

1. **AAA Pattern**: 100% mandatory - no assertions in ARRANGE
2. **Confirmations**: Always use `confirmations: 1` for local tests
3. **Wallet Separation**: owner ≠ provider ≠ player
4. **Financial Precision**: Exact wei-level calculations, zero tolerance
5. **Complete Validation**: Verify ALL affected parties and state
6. **Event-Driven Tracking**: Use events for IDs and sequence numbers
7. **Time Safety**: Forward-only time manipulation with fresh calculations
8. **VRF Diversity**: Test multiple random values for same outcome
9. **Error Specificity**: Custom errors with exact validation
10. **False Negative Prevention**: Tests must fail when contracts broken
11. **Success Test Requirement**: Every public function MUST have at least one success test
12. **Test Isolation**: Each test completely independent
13. **Native-First**: Use framework APIs directly, minimal abstraction
14. **Performance**: Fast execution without sacrificing correctness
15. **Descriptive Naming**: Clear, action-oriented test names

---

**Remember**: A passing test suite with false negatives is worse than a failing test suite with real issues. Your job is to ensure every passing test represents genuine system correctness.

**When tests pass, we deploy with confidence. When tests fail, we fix real bugs. No exceptions. No false confidence. Zero tolerance for false negatives.**

**Critical Rule from CHA-66**: Never write a describe block with only failure tests. If you can't write a success test, the function might be dead code.

---

## 📚 Critical Patterns & Anti-Patterns

### Pattern 1: Confirmation Strategy (CRITICAL)

**RULE**: Always use `confirmations: 1` for local Hardhat tests.

**Why This Matters**:

Local Hardhat network uses auto-mining (mines blocks on demand):

- `confirmations: 2` waits for second block → never mines without new transaction → timeout
- `confirmations: 1` waits for transaction's own block → instant completion
- **Performance**: 120x faster execution

```typescript
// ✅ CORRECT: Single confirmation for local tests
const receipt = await publicClient.waitForTransactionReceipt({
  hash: tx,
  confirmations: 1,
});

// ❌ WRONG: Multiple confirmations cause timeouts
const receipt = await publicClient.waitForTransactionReceipt({
  hash: tx,
  confirmations: 2, // Waits forever on local Hardhat
});

// ❌ FORBIDDEN: setTimeout for blockchain state
await new Promise((resolve) => setTimeout(resolve, 2000)); // Never use this
```

**Rationale**: Viem's native confirmation system handles blockchain state consistency. No additional delays needed.

---

### Pattern 2: AAA Pattern Enforcement (MANDATORY)

**RULE**: 100% of tests MUST follow strict AAA (Arrange-Act-Assert) separation.

**Correct Structure**:

```typescript
it("should create lottery with exact state", async function () {
  this.timeout(60000);

  // ARRANGE: Setup only - ZERO assertions
  const { contracts, publicClient, viem } = await setupLocalEnvironment();
  const walletClients = await viem.getWalletClients();
  const owner = walletClients[0];
  const provider = walletClients[1];

  await setupTokensForWallet(contracts, publicClient, owner, provider, 20_000_000n);

  // ACT: Single operation under test
  const { lotteryId } = await createLotteryNative(contracts, publicClient, provider, {
    prizeAmount: 10_000_000n,
    ticketPrice: 100_000n,
    pickRange: 10,
    duration: 3600,
  });

  // ASSERT: ALL validations here only
  const lotteryInfo = await contracts.lottery.read.getLotteryInfo([lotteryId]);
  expect(lotteryInfo.prizeAmount).to.equal(10_000_000n, "Prize amount must be exact");
  expect(lotteryInfo.ticketPrice).to.equal(100_000n, "Ticket price must be exact");
  expect(lotteryInfo.status).to.equal(LOTTERY_STATUS.ACTIVE, "Status must be ACTIVE");
});
```

**Common Violations to Avoid**:

```typescript
// ❌ WRONG: Assertions in ARRANGE phase
const tx = await contracts.lottery.write.createLottery([...]);
expect(tx).to.exist; // ❌ Testing setup, not operation

// ❌ WRONG: Multiple operations in ACT phase
const { lotteryId } = await createLotteryNative(...);
await buyTicketsNative(...); // ❌ Split into separate tests

// ❌ WRONG: Partial validation in ASSERT
expect(lotteryInfo.status).to.equal(LOTTERY_STATUS.ACTIVE);
// ❌ Missing: prizeAmount, ticketPrice, provider validation
```

---

### Pattern 3: False Negative Prevention (ZERO TOLERANCE)

**RULE**: Tests MUST fail when contracts are broken. Validate with break testing.

**Critical Anti-Patterns**:

1. **Tolerance Trap** (FORBIDDEN):

   ```typescript
   // ❌ FORBIDDEN: 10% tolerance hides 50% calculation bugs
   expect(prizeAmount).to.be.closeTo(inputAmount, inputAmount / 10n);

   // ✅ CORRECT: Exact validation
   const expectedAfterFee = (inputAmount * 95n) / 100n;
   expect(prizeAmount).to.equal(expectedAfterFee, "Must be exactly 95% of input");
   ```

2. **Range Deception** (FORBIDDEN):

   ```typescript
   // ❌ FORBIDDEN: Accepts any status ≥ 0
   expect(status).to.be.greaterThanOrEqual(0);

   // ✅ CORRECT: Exact state validation
   expect(status).to.equal(LOTTERY_STATUS.ACTIVE, "Must be exactly ACTIVE status");
   ```

3. **Existence Validator** (FORBIDDEN):

   ```typescript
   // ❌ FORBIDDEN: Tests existence, not correctness
   expect(events.length).toBeGreaterThan(0);

   // ✅ CORRECT: Exact count validation
   expect(events).to.have.lengthOf(1, "Must emit exactly 1 event");
   expect(events[0].args.lotteryId).to.equal(lotteryId);
   ```

**Break Testing Validation**:

1. Intentionally break contract (e.g., change fee from 5% to 50%)
2. Run test - it MUST fail
3. Restore contract - test MUST pass
4. Document validation - test is now trusted

**Target**: 100% bug detection rate across all tests

---

### Pattern 4: Token Minting (CRITICAL RULE)

**RULE**: Only contract deployer (owner) can mint tokens. All others receive via transfer.

**Why This Matters**: ERC20 tokens with Ownable pattern restrict minting to owner only.

```typescript
// ✅ CORRECT: Owner mints, then transfers
await setupTokensForWallet(contracts, publicClient, owner, provider, 20_000_000n);
// Internally: owner.mint() → owner.transfer(provider)

// ❌ WRONG: Provider trying to mint
await contracts.token.write.mint([provider.account.address, 20_000_000n], {
  account: provider.account,
});
// ERROR: Ownable: caller is not the owner

// ❌ WRONG: Player trying to mint
await contracts.token.write.mint([player.account.address, 1_000_000n], {
  account: player.account,
});
// ERROR: Ownable: caller is not the owner
```

**setupTokensForWallet Implementation**:

```typescript
export async function setupTokensForWallet(contracts: any, publicClient: any, ownerWallet: any, targetWallet: any, amount: bigint): Promise<void> {
  // Step 1: Owner mints to themselves
  const mintTx = await contracts.token.write.mint([ownerWallet.account.address, amount], {
    account: ownerWallet.account,
  });
  await publicClient.waitForTransactionReceipt({ hash: mintTx, confirmations: 1 });

  // Step 2: Owner transfers to target
  const transferTx = await contracts.token.write.transfer([targetWallet.account.address, amount], {
    account: ownerWallet.account,
  });
  await publicClient.waitForTransactionReceipt({ hash: transferTx, confirmations: 1 });
}
```

**Benefits**:

1. Mimics production token distribution
2. Prevents "not owner" errors
3. Maintains clean wallet separation
4. Enables accurate financial tracking

---

## 🎓 Lessons Learned (Do Not Repeat)

### Anti-Pattern 1: setTimeout for Blockchain State

**Never Do This**:

```typescript
// ❌ FORBIDDEN
await publicClient.waitForTransactionReceipt({ hash: tx });
await new Promise((resolve) => setTimeout(resolve, 2000)); // Arbitrary delay
```

**Why It's Wrong**:

- Non-deterministic (timing-dependent)
- Doesn't guarantee blockchain state consistency
- Slower than native confirmations
- Hides real timing issues

**Always Do This**:

```typescript
// ✅ CORRECT
await publicClient.waitForTransactionReceipt({ hash: tx, confirmations: 1 });
```

---

### Anti-Pattern 2: Assertions in ARRANGE Phase

**Never Do This**:

```typescript
// ❌ FORBIDDEN
it("should perform operation", async function () {
  const { contracts } = await setupLocalEnvironment();
  const tx = await contracts.lottery.write.createLottery([...]);
  expect(tx).to.exist; // ❌ Assertion in ARRANGE
  // ...
});
```

**Why It's Wrong**:

- Violates AAA pattern
- Tests setup instead of operation
- Reduces maintainability
- Creates false confidence

**Always Do This**:

```typescript
// ✅ CORRECT
it("should perform operation", async function () {
  // ARRANGE: Setup only
  const { contracts } = await setupLocalEnvironment();

  // ACT: Operation under test
  const { lotteryId } = await createLotteryNative(contracts, publicClient, provider, {...});

  // ASSERT: Validate outcomes
  const lotteryInfo = await contracts.lottery.read.getLotteryInfo([lotteryId]);
  expect(lotteryInfo.status).to.equal(LOTTERY_STATUS.ACTIVE);
});
```

---

### Anti-Pattern 3: Tolerance-Based Financial Assertions

**Never Do This**:

```typescript
// ❌ FORBIDDEN
expect(balance).to.be.closeTo(expected, 1000); // 1000 wei tolerance
expect(amount).to.be.approximately(expected, 0.01); // 1% tolerance
```

**Why It's Wrong**:

- Hides calculation bugs
- False negatives (tests pass when they shouldn't)
- Unacceptable for financial operations
- Violates zero-tolerance principle

**Always Do This**:

```typescript
// ✅ CORRECT
expect(balance).to.equal(expected, "Balance must be exact to the wei");
expect(amount).to.equal(calculatedAmount, "Amount must match exact calculation");
```

---

### Anti-Pattern 4: Range-Based State Validation

**Never Do This**:

```typescript
// ❌ FORBIDDEN
expect(status).to.be.greaterThanOrEqual(0); // Accepts any status ≥ 0
expect(lotteryId).to.be.greaterThan(0); // Accepts any ID > 0
```

**Why It's Wrong**:

- Too vague (accepts wrong values)
- False negatives (wrong state passes)
- Doesn't validate exact behavior
- Hides state machine bugs

**Always Do This**:

```typescript
// ✅ CORRECT
expect(status).to.equal(LOTTERY_STATUS.ACTIVE, "Must be exactly ACTIVE");
expect(lotteryId).to.equal(expectedId, "Must be exact lottery ID");
```

---

## Test Iteration Best Practices (Added 2025-01-15)

### Fast Feedback Loop

**PRINCIPLE**: Run only the failing test during development, not the entire suite.

```bash
# ❌ SLOW: Running all 496 tests (60+ seconds)
pnpm test:local test/local/e2e/*.test.ts

# ✅ FAST: Running only the failing test (1-2 seconds)
pnpm test:local test/local/e2e/economic-model-validation.test.ts --grep "should validate provider profitability"
```

**Benefits:**

- 60x faster feedback loop
- Immediate error visibility
- Faster debugging iterations
- Less context switching

### Debugging Workflow

```bash
# 1. Identify failing test
pnpm test:local test/local/e2e/*.test.ts 2>&1 | grep ")" | grep -E "^\s+[0-9]+\)"

# 2. Run only that test
pnpm test:local test/local/e2e/economic-model-validation.test.ts --grep "specific test name"

# 3. Fix and iterate quickly
# 4. Once passing, run full suite to verify no regressions
pnpm test:local test/local/e2e/*.test.ts
```

### Test Simplification Strategy

**PRINCIPLE**: If a test is complex and failing, simplify it first before debugging.

```typescript
// ❌ COMPLEX: Hard to debug
for (let i = 0; i < 100; i++) {
  await buyTicketsNative(..., [(i % 10) + 1], { skipVRFFulfillment: true });
}
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
for (let i = 0; i < 100; i++) {
  await fulfillMockVRFWithLoss(..., seq - BigInt(100 - i), 100);
}

// ✅ SIMPLE: Easy to debug, proves same concept
await buyTicketsNative(..., [1], { skipVRFFulfillment: true });
await buyTicketsNative(..., [1], { skipVRFFulfillment: true });
await buyTicketsNative(..., [2], { skipVRFFulfillment: true });
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
await fulfillMockVRFWithLoss(..., seq - 2n, 100);
await fulfillMockVRFWithLoss(..., seq - 1n, 100);
await fulfillMockVRFWithWin(..., seq, 2, 2);
```

**Why Simplification Works:**

- Easier to understand what's happening
- Fewer moving parts = fewer failure points
- Still validates the same business logic
- Faster execution
- More maintainable

---

### Anti-Pattern 5: Counter-Based Entity Tracking

**Never Do This**:

```typescript
// ❌ FORBIDDEN
await contracts.lottery.write.createLottery([...]);
const lotteryId = await contracts.lottery.read.lotteryCounter(); // Race condition risk
```

**Why It's Wrong**:

- Race condition risk (concurrent operations)
- Assumes sequential execution
- Breaks with parallel tests
- Not event-driven

**Always Do This**:

```typescript
// ✅ CORRECT
const { lotteryId } = await createLotteryNative(contracts, publicClient, provider, {...});
// Helper reads ID from actual LotteryCreated event
```

---

## ProductionLottery Coverage Map

| Domain                 | Functions                                                                                                                                   | Required Assertions                                                                                                                                                                                                                             |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Provider Flows**     | `createLottery`, `finalizeLottery`, `withdrawUnawardedPrize`                                                                                | Parameter validation (zero values, bounds), prize funding checks, single-use finalization, unauthorized caller rejection, prize restitution on failure, treasury fee conservation, lottery counter increments exactly once per creation         |
| **Player Flows**       | `buyTicketsPacked`, `buyTicketsPackedNoReferral`, `withdrawFailedRefund`                                                                    | Ticket price enforcement, VRF fee payment, number packing limits, duplicate ticket rejection, referral permutations, revert reasons, refund accounting for both pending and failed callbacks                                                    |
| **VRF Integration**    | Observed via `requestV2` → `_entropyCallback` (through MockEntropy helpers)                                                                 | Sequence number tracking, double fulfillment rejection, stale callback rejection, status transition to `COMPLETED`, winner/loser branching, fee refunds when callbacks expire, event payloads (`VRFRequested`, `LotteryCompleted`, etc.)        |
| **Read APIs**          | `getLotteryInfo`, `isLotteryActive`, `getVRFFee`, `failedRefunds`, additional structs exposed through the contract                          | Snapshot accuracy after every state transition, derived fields (pending counts, status flags), fee reads reflecting admin updates, failed refund ledger updates per address, consistency across repeated reads                                  |
| **Cross-Contract Ops** | Treasury + token interactions, referral payouts, admin configurables (`setReferralManager`, `setTreasury`, `setVRFFee`, `pause`, `unpause`) | Access control on setters, persistence of new addresses/fees, effect on downstream flows (ticket purchase must use updated treasury/referral manager, fee updates apply to subsequent purchases), pause state enforcement across public methods |

### Cross-Cutting Invariants

- **Fee Split**: 5% to treasury, 95% to provider on every successful ticket sale (validated in both win/lose flows)
- **Prize Conservation**: Prize escrowed once on creation, released exactly once (winner claim or provider reclaim)
- **Request Lifecycle**: Pending request count increments on purchase, decrements on callback or expiration; no orphaned pending entries
- **Time-bound Logic**: Duration and expiration windows enforced (cannot finalize early, cannot buy after end)
- **Event Surface**: Every state mutation emits the documented event with exact arguments (use event assertions, not manual decoding)

Use this map when auditing test suites—each domain needs atomic tests for success, failure, and edge-case branches.

---

## Coverage Expansion Playbook

These scenarios were previously weak or missing in coverage. Add or maintain targeted tests before increasing contract complexity.

### VRF Fee Dynamics

- Test `setVRFFee` updates mid-lottery: new purchases pay the new fee, existing pending requests still resolve safely
- Validate refunds when fee decreases and ensure overpayment routes back to players treasury ledger
- Reproduce fee race bugs by altering fees between request and fulfillment; assert `failedRefunds` and balances balance out

### Multi-Provider Isolation

- Run flows with at least two providers in the same suite: independent lotteries, overlapping sequence numbers, and simultaneous callbacks
- Ensure treasury and referral accounting stays per-lottery and no provider can finalize another provider’s lottery
- Verify `lotteryCounter` is global but emissions/events allow mapping back to the correct creator

### Concurrent Ticket Purchases

- Simulate 5–10 players buying tickets before callback, then fulfill VRF to confirm all entries retained
- Assert aggregate ticket revenue equals provider + treasury deltas, even when purchases span multiple blocks
- Include tests for duplicate numbers and packed number bounds under concurrency conditions

### Expiration & Recovery Paths

- Cover the full expiration flow: lottery ends with no winner, provider withdraws prize, players reclaim failed refunds
- Break callbacks intentionally (skip fulfillment, advance time) to assert recovery mechanisms (`withdrawFailedRefund`, provider reclaim) operate correctly

### Gas Limit & Admin Variations

- Execute callbacks with custom gas limits to match production defaults
- Toggle admin setters (`pause`, `unpause`, update referral manager) to ensure downstream flows respect the new configuration immediately
- Document gas-sensitive scenarios so we can mirror them on testnet/mainnet later

Treat this playbook as a backlog of defensive tests—each item closes a gap that previously surfaced during audits.

---

## 📊 Quality Metrics Evolution

### Test Quality Scores

| Metric                  | Initial | Phase 1 | Phase 2 | Current | Target |
| ----------------------- | ------- | ------- | ------- | ------- | ------ |
| **Adherence Score**     | 33.4%   | 72%     | 93.33%  | 98.52%  | 98%+   |
| **AAA Compliance**      | 0%      | 100%    | 100%    | 100%    | 100%   |
| **False Negative Risk** | 3.2/10  | 0.5/10  | 0/10    | 0/10    | 0/10   |
| **Test Count**          | 54      | 56      | 271     | 364     | 400+   |
| **Execution Time**      | 60s     | 35s     | 35s     | 45s     | <60s   |
| **Coverage**            | 53.25%  | 62.75%  | 78.50%  | 92.30%  | 95%+   |

### Bug Detection Rate

| Test Category | Bugs Introduced | Bugs Detected | Detection Rate |
| ------------- | --------------- | ------------- | -------------- |
| Financial     | 12              | 12            | 100%           |
| VRF Timing    | 8               | 8             | 100%           |
| State Machine | 15              | 15            | 100%           |
| Security      | 10              | 10            | 100%           |
| **Overall**   | **45**          | **45**        | **100%**       |

**Validation Method**: Intentional contract breaking + test execution

---

## 🔄 Continuous Improvement

### Quality Assurance Checklist

**Before Committing Tests**:

- [ ] Run full test suite: `pnpm test`
- [ ] Verify all tests pass: 100% pass rate required
- [ ] Check execution time: Should be <60s for full suite
- [ ] Validate AAA pattern: Zero violations allowed
- [ ] Confirm exact assertions: No tolerance/range checks
- [ ] Verify break testing: Critical tests validated
- [ ] Review coverage: Should be 95%+ for production

### New Test Requirements

Every new test MUST:

1. **Follow AAA Pattern**: Zero tolerance for violations
2. **Use Exact Assertions**: No tolerance, no ranges, no approximations
3. **Pass Break Testing**: Intentionally break contract to verify test fails
4. **Use Event-Driven Tracking**: No counter-based IDs or sequence numbers
5. **Separate Wallets**: owner ≠ provider ≠ player (always)
6. **Include Descriptive Messages**: Every assertion has clear failure message
7. **Complete in <1s**: Individual test performance target

### Quality Metrics Targets

| Metric                  | Target | Enforcement |
| ----------------------- | ------ | ----------- |
| **AAA Compliance**      | 100%   | Mandatory   |
| **False Negative Risk** | 0/10   | Mandatory   |
| **Adherence Score**     | 98%+   | Mandatory   |
| **Bug Detection Rate**  | 100%   | Mandatory   |
| **Test Pass Rate**      | 100%   | Mandatory   |
| **Execution Time**      | <60s   | Target      |
| **Coverage**            | 95%+   | Target      |

---

## 📖 Reference Documentation

### Project Files

- **This Document**: `apps/hardhat/test/LOCAL_TESTING_PRINCIPLES.md`
- **Test Helpers**: `apps/hardhat/test/helpers/`
- **Test Suites**: `apps/hardhat/test/local/`
- **Constants**: `apps/hardhat/test/constants/`

### External Resources

- **Hardhat**: https://hardhat.org/docs
- **Viem**: https://viem.sh
- **Mocha**: https://mochajs.org
- **Chai**: https://www.chaijs.com
- **Pyth Entropy**: https://docs.pyth.network/entropy

---
