---
type: "agent_requested"
description: "Example description"
---

# Testnet Testing Playbook

Purpose: enforce production-grade testing on Base Sepolia using real contracts, real VRF, and verifiable data sources. Treat this playbook as the complete, self-contained reference for testnet validation.

The playbook now serves as a mental framework and quality standard for testnet testing, ensuring all tests are backed by verifiable evidence and designed to catch real bugs.

The playbook is now:

- Principle-driven: Teaches HOW to write tests, not WHAT tests exist
- Evidence-focused: Requires external validation for all assertions
- Adversarial: Emphasizes finding bugs over confirming functionality
- Universal: Applicable to any testnet test scenario
- Production-grade: Enforces standards strong enough for mainnet deployment

---

## Scope & Goals

- Validate live contract behavior on Base Sepolia with production deployments, not mocks.
- Exercise specification-critical flows end to end, including financial reconciliation and cross-service integrations.
- Produce evidence strong enough to greenlight production releases without relying on any other document.

---

## Core Philosophy: Deterministic Logic Stays Local

**Fundamental Principle**: If logic is deterministic and works 100% locally, it will work 100% on testnet. Don't waste testnet resources re-testing deterministic behavior.

**Testnet's Unique Purpose**: Validate **integration risks** with external dependencies, NOT re-test contract logic.

### Integration Risk vs Contract Logic

**Integration Risks (TESTNET REQUIRED)**:

- Real VRF provider behavior (Pyth Entropy liveness, callbacks, fees)
- Real blockchain state (time-based expiration, block timestamps, network timing)
- Real subgraph indexing (Goldsky data persistence, query accuracy)
- Real token transfers (ERC20 behavior on Base Sepolia)
- Real network conditions (latency, gas costs, concurrent transactions)
- Real wallet workflows (EIP-2612 permits, signature validation)

**Contract Logic (LOCAL SUFFICIENT)**:

- Mathematical calculations (fees, prize distribution, tier boundaries)
- State transitions (ACTIVE → ENDED → EXPIRED)
- Validation logic (input checks, access control)
- Error conditions (reverts, custom errors)
- Edge cases (boundary values, overflow/underflow)
- Security scenarios (reentrancy, access control violations)

**Decision Rule**: If the behavior is **deterministic** (same inputs always produce same outputs regardless of network), test it locally. If it depends on **external systems** or **real network state**, test it on testnet.

---

## Testnet Test Decision Framework

**Before proposing a new testnet test, answer these questions in order:**

### Step 1: Is it already tested locally?

**Question**: Does a local test already validate this contract logic?

**Action**: Search `apps/hardhat/test/local/` for existing coverage.

**Decision**:

- ✅ **YES** → Proceed to Step 2
- ❌ **NO** → Write local test first, then proceed to Step 2

**Rationale**: Local tests provide 100% code coverage. Never skip local testing.

### Step 2: Is the logic deterministic?

**Question**: Will this behavior be identical on testnet vs local, given the same inputs?

**Examples of Deterministic Logic**:

- Fee calculations: `(amount * 500) / 10_000` always equals 5%
- Tier boundaries: `prizeAmount <= 100_000_000000` always returns Tier 1
- State transitions: `status = ENDED` always sets status to 2
- Access control: `onlyProvider` always checks `msg.sender == provider`
- Token math: `balance - cost` always produces same result

**Examples of Non-Deterministic Logic**:

- Time-based expiration: `block.timestamp > endTime` depends on real blockchain time
- VRF callbacks: Pyth Entropy response time varies by network conditions
- Subgraph indexing: Data availability depends on Goldsky infrastructure
- Network latency: Transaction confirmation time varies
- Concurrent transactions: Race conditions depend on real block ordering

**Decision**:

- ✅ **Deterministic** → REJECT testnet test (local coverage sufficient)
- ❌ **Non-Deterministic** → Proceed to Step 3

**Rationale**: Deterministic logic works identically everywhere. Testing it on testnet wastes resources.

### Step 3: Does it validate integration with external systems?

**Question**: Does this test validate behavior that depends on external systems or real network state?

**External Systems**:

- Pyth Entropy VRF (real randomness provider)
- Goldsky Subgraph (real indexing service)
- Base Sepolia blockchain (real network timing, gas, blocks)
- Etherscan API (real transaction verification)
- ERC20 token contracts (real token behavior)

**Decision**:

- ✅ **YES** → Proceed to Step 4
- ❌ **NO** → REJECT testnet test (local coverage sufficient)

**Rationale**: Testnet tests exist to validate integration, not contract logic.

### Step 4: Is it practical to test in automated CI?

**Question**: Can this test complete within 15 seconds and run reliably in CI/CD?

**Practical Tests**:

- Short duration lotteries (5-10 minutes)
- Standard VRF callbacks (<10 seconds)
- Normal transaction flows (<8 seconds)
- Subgraph queries (<12 seconds)

**Impractical Tests**:

- VRF timeout scenarios (1+ hour wait)
- Long-duration lotteries (hours/days)
- Manual intervention scenarios
- Rare edge cases requiring specific network conditions

**Decision**:

- ✅ **Practical** → Proceed to Step 5
- ❌ **Impractical** → Use quarterly manual drill instead

**Rationale**: CI/CD tests must be fast and reliable. Impractical tests belong in operational runbooks.

### Step 5: Does it provide unique coverage?

**Question**: Does this test validate a scenario NOT already covered by existing testnet tests?

**Action**: Review existing testnet scenarios (A-F) for overlap.

**Decision**:

- ✅ **Unique coverage** → APPROVE testnet test
- ❌ **Redundant** → REJECT testnet test

**Rationale**: Avoid redundant testing. Each testnet test should validate a unique integration scenario.

---

## Decision Framework Summary

```
┌─────────────────────────────────────┐
│ Proposed Testnet Test               │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│ Step 1: Already tested locally?     │
│ Search apps/hardhat/test/local/     │
└──────────────┬──────────────────────┘
               │
               ├─ NO ──► Write local test first
               │
               ▼ YES
┌─────────────────────────────────────┐
│ Step 2: Is logic deterministic?     │
│ Same inputs = same outputs?         │
└──────────────┬──────────────────────┘
               │
               ├─ YES ──► REJECT (local sufficient)
               │
               ▼ NO
┌─────────────────────────────────────┐
│ Step 3: Validates integration?      │
│ Depends on external systems?        │
└──────────────┬──────────────────────┘
               │
               ├─ NO ──► REJECT (local sufficient)
               │
               ▼ YES
┌─────────────────────────────────────┐
│ Step 4: Practical for CI?           │
│ Completes in <15 seconds?           │
└──────────────┬──────────────────────┘
               │
               ├─ NO ──► Use manual drill
               │
               ▼ YES
┌─────────────────────────────────────┐
│ Step 5: Unique coverage?            │
│ Not redundant with existing tests?  │
└──────────────┬──────────────────────┘
               │
               ├─ NO ──► REJECT (redundant)
               │
               ▼ YES
┌─────────────────────────────────────┐
│ ✅ APPROVE TESTNET TEST              │
└─────────────────────────────────────┘
```

---

## Adversarial Testing Mindset

**Fundamental Principle**: We are NOT testing to prove the contract works. We are testing to FIND WHERE IT BREAKS.

**Mindset Shift**:

- ❌ "Let's verify this works correctly"
- ✅ "Let's prove this contract has bugs, or exhaust all attempts to break it"

**Testing Attitude**:

- Assume every function has a vulnerability
- Assume every calculation can be exploited
- Assume every state transition has a loophole
- Assume every user will try to cheat
- Assume every edge case will be hit in production

**Auditor Responsibility**:

- You are the last line of defense before user funds are at risk on mainnet.
- Demand evidence: every expectation must map to verifiable external sources.
- Question everything—the spec, the implementation, and your own understanding. Ambiguity is a blocker, not a footnote.

---

## Guiding Principles

1. **Deterministic logic stays local.** If logic works 100% locally, it works 100% on testnet—don't waste testnet resources re-testing deterministic behavior.
2. **Testnet validates integration only.** Use testnet to validate external dependencies (VRF, subgraph, blockchain state), not contract logic.
3. **Assume the contract is broken.** Tests exist to uncover failures, never to rubber-stamp success.
4. **Zero tolerance for false negatives.** A passing test must prove the system behaves correctly under adversarial scrutiny.
5. **Evidence-based assertions only.** Every assertion MUST be validated against real, external evidence sources—never rely solely on contract read calls.
6. **Real network or nothing.** Interact only with deployed addresses, Pyth Entropy VRF, and the Goldsky subgraph.
7. **Triple validation.** Confirm outcomes via blockchain explorer APIs, indexed subgraph data, AND contract state reads.
8. **Persistent state awareness.** Base Sepolia is not reset between runs—design idempotent setups.
9. **Role separation.** Providers, players, treasury, and admin wallets must remain distinct in every scenario.
10. **Financial precision.** Account for every wei; token conservation is mandatory.
11. **Eventual consistency.** Fresh transactions are not instantly indexable—plan for lag when reading state or subgraph data.
12. **No explicit delays.** Replace ad-hoc sleeps with data-driven polling helpers that exit as soon as evidence appears.
13. **Prefer EIP-2612 permits over approve/transferFrom.** Use `createLotteryWithPermit` and `buyTicketsWithPermit` for gasless token approvals; only use traditional approvals when explicitly testing backward compatibility.
14. **Preserve native errors.** Allow contract and Viem errors to surface untouched; they contain critical diagnostics.
15. **Minimal logging.** Log only when it materially improves debugging; silence is the default.
16. **Clear assertions.** Use descriptive messages in `expect()` so failures expose intent immediately.
17. **Trust the framework.** Mocha, Viem, and Hardhat already emit detailed traces—avoid wrapping their errors or outputs.
18. **Fast-fail timeouts.** Every test must complete within 15 seconds; use aggressive global timeouts to prevent hanging.
19. **Small test values.** Use minimal token amounts (10 USDC prize, 0.1 USDC ticket) to prevent wallet depletion across test suites.
20. **Polling over delays.** Use 2-second polling intervals for subgraph indexing and state verification; never use fixed sleep delays.
21. **No individual timeouts.** Remove all `this.timeout()` calls; rely on global Mocha configuration for consistency.
22. **Use helper functions over inline code.** Prefer `generatePermitSignature()` and `packNumbers()` helpers over inline permit objects and manual packing to ensure consistency and reduce duplication.
23. **Operational mitigations for impractical tests.** Use quarterly manual drills and monitoring runbooks for scenarios impractical to automate (VRF timeouts, long-duration lotteries).

---

## Operational Mitigations for Impractical Tests

**Principle**: Not all critical scenarios can be tested in automated CI. Use operational mitigations for impractical tests.

### When to Use Operational Mitigations

**Use operational mitigations when a scenario is:**

- ✅ Critical for production safety
- ✅ Already tested locally (contract logic validated)
- ❌ Impractical for automated CI (>15 seconds, requires manual intervention)
- ❌ Depends on rare network conditions or long wait times

**Examples**:

- VRF timeout scenarios (1+ hour wait)
- Long-duration lottery expiration (hours/days)
- Manual intervention flows (admin actions, emergency procedures)
- Rare edge cases requiring specific network conditions

### Operational Mitigation Strategies

#### 1. Monitoring + Alerting

**Purpose**: Detect issues in production before they impact users.

**Implementation**:

- Set up alerts for VRF callbacks >10 minutes (Pyth Entropy SLA: <5 minutes)
- Monitor lottery expiration events
- Track platform fee collection accuracy
- Alert on unexpected state transitions

**Example**:

```typescript
// Alert if VRF callback takes >10 minutes
if (vrfCallbackTime > 10 * 60 * 1000) {
  alert("VRF callback delayed - investigate Pyth Entropy status");
}
```

#### 2. Manual Runbooks

**Purpose**: Provide step-by-step procedures for handling edge cases.

**Implementation**:

- Document manual intervention procedures
- Include exact contract function calls
- Provide example transactions on testnet
- Test runbooks quarterly

**Example Runbook**: VRF Timeout Manual Refund

```markdown
1. Identify stuck VRF request: `getPendingRequestCount(lotteryId)`
2. Verify timeout: `block.timestamp > requestTime + 1 hour`
3. Call `expirePendingRequest(sequenceNumber)` as player
4. Verify refund: check player balance increased by ticket cost
5. Document incident in operations log
```

#### 3. Quarterly Manual Drills

**Purpose**: Validate operational procedures work correctly on real testnet.

**Schedule**: Every 3 months (quarterly)

**Drill 1: VRF Timeout Scenario**

- **Environment**: Base Sepolia testnet
- **Duration**: 1+ hour
- **Procedure**:
  1. Create lottery with short duration
  2. Buy tickets
  3. Wait for VRF timeout (1+ hour)
  4. Manually call `expirePendingRequest(sequenceNumber)`
  5. Validate refund processed correctly
  6. Update runbook with any learnings

**Drill 2: Lottery Expiration Scenario**

- **Environment**: Base Sepolia testnet
- **Duration**: 10-15 minutes
- **Procedure**:
  1. Create lottery with 5-minute duration
  2. Wait for expiration (no ticket purchases)
  3. Call `finalizeLottery(lotteryId)`
  4. Call `withdrawUnawardedPrize(lotteryId)`
  5. Validate prize returned to provider
  6. Update runbook with any learnings

**Drill 3: Emergency Pause Scenario**

- **Environment**: Base Sepolia testnet
- **Duration**: 5 minutes
- **Procedure**:
  1. Simulate emergency condition
  2. Execute pause procedure
  3. Verify all user funds are safe
  4. Execute unpause procedure
  5. Verify normal operations resume
  6. Update runbook with any learnings

### Documentation Requirements

**For each operational mitigation, document:**

- ✅ Scenario description
- ✅ Why it's impractical for automated CI
- ✅ Local test coverage (contract logic validation)
- ✅ Monitoring/alerting setup
- ✅ Manual runbook procedure
- ✅ Quarterly drill schedule
- ✅ Last drill date and results

**Example Documentation**:

```markdown
## VRF Timeout Handling

**Scenario**: VRF callback doesn't arrive within 1 hour

**Why Not Automated**: Requires 1+ hour wait, impractical for CI/CD

**Local Coverage**: `vrf-timing.test.ts` validates timeout logic with time manipulation

**Monitoring**: Alert if VRF callback >10 minutes (Pyth Entropy SLA: <5 minutes)

**Runbook**: See "VRF Timeout Manual Refund" procedure

**Drill Schedule**: Quarterly (every 3 months)

**Last Drill**: 2025-10-15 - PASSED - All refunds processed correctly
```

---

## Evidence-Based Assertions Principle

**Core Requirement**: ALL assertions in testnet tests MUST be validated against real, external evidence sources. Never rely solely on contract read calls without cross-validation.

### Evidence Source Selection

**Use Blockchain Explorer APIs (Etherscan/Basescan) when:**

- Verifying transaction receipts and confirmations
- Validating event emissions and event parameters
- Checking transaction status and revert reasons
- Confirming gas usage and block inclusion
- Inspecting raw transaction data and logs

**Use Subgraph GraphQL Queries when:**

- Validating aggregated financial data (total revenue, tickets sold)
- Checking historical state across multiple transactions
- Verifying indexed entity relationships (lottery → tickets → players)
- Confirming time-series data and trends
- Validating cross-contract state consistency

**Use Contract Read Calls when:**

- Reading current on-chain state as ONE of multiple evidence sources
- Validating state transitions immediately after transactions
- Checking real-time values that haven't been indexed yet
- ALWAYS cross-validate with at least one external source

### False Negative Prevention

**Break-First Thought Experiment**: For each critical test, describe how breaking the contract should cause failure and whether the test would catch it.

**Example Questions**:

- If I remove the prize transfer line, would this test fail?
- If I change the platform fee to 10%, would this test detect it?
- If I allow duplicate winners, would this test catch it?
- If VRF callback is called twice, would this test fail?

**Mock Realism Review**: Evaluate whether any test dependencies could hide bugs:

- Does the test use real Pyth Entropy VRF or a mock?
- Does the test use real USDC behavior or simplified token logic?
- Does the test account for network delays and eventual consistency?
- Does the test exercise all observable side effects?

**Event & Side-Effect Coverage**: Confirm the test exercises and validates ALL observable side effects:

- Token transfers (from, to, amount)
- Event emissions (all parameters)
- State changes (status, winner, balances)
- External calls (VRF requests, subgraph updates)

### Assertion Evidence Audit

Every assertion must have an associated contract reference, specification requirement, or invariant:

```typescript
// ❌ WEAK: No evidence source
expect(balance).to.equal(expectedBalance);

// ✅ STRONG: Multiple evidence sources
const balanceOnChain = await contracts.token.read.balanceOf([player.account.address]);
const txReceipt = await getTransactionReceipt(txHash); // Etherscan API
const { lottery } = await querySubgraph(GET_LOTTERY_FINANCIAL, { lotteryId }); // Subgraph

expect(balanceOnChain, "on-chain balance mismatch").to.equal(expectedBalance);
expect(txReceipt.status, "transaction should succeed").to.equal("1");
expect(BigInt(lottery.ticketsSold), "subgraph tickets sold mismatch").to.equal(1n);
```

### Financial Math Trace

For monetary flows, trace the calculation end-to-end and record any unexplained numbers:

```typescript
// Document the calculation
const ticketPrice = 100_000n;
const platformFeeRate = 500n; // 5% = 500 basis points
const platformFee = (ticketPrice * platformFeeRate) / 10_000n;
const providerRevenue = ticketPrice - platformFee;

// Verify with multiple sources
const treasuryDelta = treasuryAfter - treasuryBefore;
const providerDelta = providerAfter - providerBefore;

expect(treasuryDelta, "treasury should receive exact platform fee").to.equal(platformFee);
expect(providerDelta, "provider should receive exact net revenue").to.equal(providerRevenue);

// Cross-validate with subgraph
const { lottery } = await querySubgraph(GET_LOTTERY_FINANCIAL, { lotteryId: lotteryId.toString() });
expect(BigInt(lottery.platformFee), "subgraph platform fee mismatch").to.equal(platformFee);
expect(BigInt(lottery.netRevenue), "subgraph net revenue mismatch").to.equal(providerRevenue);
```

### Edge-Case Acknowledgement

Tests must discuss or exercise edge conditions; missing considerations must be documented:

- Boundary values (0, 1, max)
- Rounding and precision loss
- Overflow and underflow scenarios
- Race conditions and timing issues
- Failed external calls and reverts

---

## Test Naming Convention

All testnet E2E tests follow a strict naming pattern for consistency and clarity:

```
Testnet E2E :: Scenario {Letter} - {Description}
```

**Current Test Scenarios (A-F):**

- `Testnet E2E :: Scenario A - Complete Win Flow` - Basic happy path with single player winning
- `Testnet E2E :: Scenario B - Complete Loss Flow` - Basic loss path with single player losing
- `Testnet E2E :: Scenario C - Multi-Ticket Win Flow` - Multiple players with race conditions
- `Testnet E2E :: Scenario D - Concurrent Purchases Before VRF` - Race condition testing
- `Testnet E2E :: Scenario E - Closed Lottery Protection` - Edge case protection
- `Testnet E2E :: Scenario F - No Winner Flow` - Probabilistic outcome handling

**Pattern breakdown:**

- `Testnet E2E` - Identifies this as an end-to-end testnet test
- `Scenario {Letter}` - Sequential scenario identifier (A-F, expandable to G-Z)
- `{Description}` - Clear, concise description of what the scenario tests

**File Naming:**

```
test/testnet/scenarios/scenario-{letter}-{kebab-case-description}.e2e.test.ts
```

**Examples:**

- `scenario-a-complete-win.e2e.test.ts`
- `scenario-d-concurrent-purchases.e2e.test.ts`
- `scenario-f-no-winner-flow.e2e.test.ts`

This convention ensures:

- Easy filtering with `--grep "Scenario A"` or `--grep "Scenario"`
- Clear identification of test purpose from the describe block
- Consistent alphabetical ordering in file explorers
- All tests in single `scenarios/` directory for easy navigation

---

## Standard Test Structure

Follow a consistent Arrange → Act → Assert pattern gated to the testnet environment.

```typescript
describe("Testnet E2E :: Scenario A - Player Wins", function () {
  this.timeout(180000);

  let contracts: any;
  let publicClient: any;
  let viem: any;
  let provider: any;
  let player: any;

  before(async function () {
    const isTestnet = process.env.HARDHAT_NETWORK === "baseSepolia" || process.env.TEST_ENVIRONMENT === "testnet";
    if (!isTestnet) this.skip();

    ({ contracts, publicClient, viem } = await setupTestnetEnvironment());
    const walletClients = await viem.getWalletClients();
    provider = walletClients[0];
    player = walletClients[1] ?? walletClients[0];

    await ensureMultipleWalletBalances(contracts.token, publicClient, provider, [
      { address: provider.account.address, requiredBalance: 300_000_000n, label: "provider" },
      { address: player.account.address, requiredBalance: 2_000_000n, label: "player" },
    ]);
  });

  it("should distribute prize money and reconcile all balances", async function () {
    const prizeAmount = 10_000_000n;
    const ticketPrice = 100_000n;

    const { lotteryId } = await createLotteryNative(contracts, publicClient, provider, {
      prizeAmount,
      ticketPrice,
      pickRange: 10,
      duration: 3600,
    });

    const playerBalanceBefore = await contracts.token.read.balanceOf([player.account.address]);

    const buyTxHash = await buyTicketsNative(contracts, publicClient, player, lotteryId, [5]);
    await publicClient.waitForTransactionReceipt({ hash: buyTxHash, confirmations: 1, timeout: 15_000 });

    await waitForVRFCompletion(lotteryId);

    const lotteryInfo = await contracts.lottery.read.getLotteryInfo([lotteryId]);
    expect(lotteryInfo.status, "lottery must end after VRF resolution").to.equal(LOTTERY_STATUS.ENDED);
    expect(await contracts.lottery.read.getPendingRequestCount([lotteryId]), "pending VRF requests must clear").to.equal(0n);
    const isWinner = lotteryInfo.hasWinner;

    const playerBalanceAfter = await contracts.token.read.balanceOf([player.account.address]);
    const expectedPlayerDelta = isWinner ? prizeAmount - ticketPrice : -ticketPrice;
    expect(playerBalanceAfter - playerBalanceBefore, "player balance delta mismatch").to.equal(expectedPlayerDelta);

    await waitForSubgraphSync(publicClient, 6, 15_000, 2_000);
    const { lottery } = await querySubgraph(GET_LOTTERY_FINANCIAL, { lotteryId: lotteryId.toString() });
    expect(lottery, "lottery should be indexed in subgraph").to.not.be.undefined;
    expect(BigInt(lottery!.grossRevenue), "subgraph gross revenue mismatch").to.equal(ticketPrice);
    expect(BigInt(lottery!.ticketsSold), "subgraph tickets sold mismatch").to.equal(1n);
  });
});
```

Expectations for every suite:

- The `before` hook skips automatically outside Base Sepolia.
- Wallet funding is conditional; no redundant minting.
- Assertions live inside the test and cover every affected party and external system.

---

## Execution Playbook

1. **Environment gate** – Skip immediately if the network is not Base Sepolia. Never run these tests on local forks.
2. **Setup once** – Call `setupTestnetEnvironment()` to source deployed addresses, ABI bindings, and Goldsky config.
3. **Fund deterministically** – Use `ensureMultipleWalletBalances` so wallets are topped up only when needed.
4. **Create via permit helpers** – Use `createLotteryWithPermit` for gasless lottery creation; pass `chainId` from test environment.
5. **Buy tickets via permits** – Use `buyTicketsWithPermit` or `generatePermitSignature()` for gasless ticket purchases; never use inline `emptyPermit` objects.
6. **Submit transactions** – Await receipts with `confirmations: 1` and explicit timeouts; avoid arbitrary sleeps.
7. **Await VRF** – Use polling helpers that check pending requests or subgraph updates without explicit delays.
8. **Validate triple evidence** – Read contract state, verify events, and query the subgraph before asserting success.
9. **Reconcile finances** – Confirm provider, player, treasury, and platform balances sum to zero net change.

---

## EIP-2612 Permit Pattern

**Principle**: Always prefer EIP-2612 permits over traditional approve/transferFrom for testnet tests to match production client behavior and enable gasless transactions.

### Lottery Creation with Permits

```typescript
import { createLotteryWithPermit } from "../../helpers/lottery-helpers.js";

const { lotteryId, hash } = await createLotteryWithPermit(
  contracts,
  publicClient,
  provider,
  {
    prizeAmount: 10_000_000n,
    ticketPrice: 100_000n,
    pickRange: 10,
    duration: 3600,
  },
  chainId // Always pass chainId from testEnv
);
```

**Key Points**:

- `createLotteryWithPermit` handles permit signature generation internally
- No manual approval transactions needed
- Saves gas and reduces transaction count
- Matches production client implementation

### Ticket Purchase with Permits

**Option 1: Using Helper Function (Preferred)**

```typescript
import { buyTicketsWithPermit } from "../../helpers/lottery-helpers.js";

const buyTx = await buyTicketsWithPermit(
  contracts,
  publicClient,
  player,
  lotteryId,
  [1, 2, 3], // ticket numbers
  undefined, // options (optional)
  chainId
);
```

**Option 2: Using Permit Signature Directly**

```typescript
import { generatePermitSignature } from "../../helpers/permit-helpers.js";
import { packNumbers } from "../../helpers/lottery-helpers.js";

const numbers = [1, 2, 3];
const totalCost = BigInt(numbers.length) * ticketPrice;
const packedNumbers = packNumbers(numbers);

const permitSignature = await generatePermitSignature(
  contracts.token,
  player,
  contracts.lottery.address,
  totalCost,
  undefined, // deadline (optional)
  chainId
);

const buyTx = await contracts.lottery.write.buyTicketsPackedNoReferral([lotteryId, packedNumbers, permitSignature], {
  account: player.account,
  value: vrfFee,
});
```

### Anti-Patterns to Avoid

**❌ WRONG: Inline Empty Permit Objects**

```typescript
// DON'T DO THIS
const emptyPermit = {
  value: 0n,
  deadline: 0n,
  v: 0,
  r: "0x0000000000000000000000000000000000000000000000000000000000000000" as const,
  s: "0x0000000000000000000000000000000000000000000000000000000000000000" as const,
};

const buyTx = await contracts.lottery.write.buyTicketsPackedNoReferral([lotteryId, packedNumbers, emptyPermit], { account: player.account, value: vrfFee });
```

**❌ WRONG: Manual Approve/TransferFrom Pattern**

```typescript
// DON'T DO THIS
const approveTx = await contracts.token.write.approve([contracts.lottery.address, totalCost], { account: player.account });
await publicClient.waitForTransactionReceipt({ hash: approveTx });

const buyTx = await contracts.lottery.write.buyTicketsPackedNoReferral([lotteryId, packedNumbers, emptyPermit], { account: player.account, value: vrfFee });
```

**❌ WRONG: Manual Number Packing**

```typescript
// DON'T DO THIS
const packedNumbers = `0x${numbers.map((n) => n.toString(16).padStart(6, "0")).join("")}` as `0x${string}`;
```

**✅ CORRECT: Use Helper Functions**

```typescript
// DO THIS
import { generatePermitSignature } from "../../helpers/permit-helpers.js";
import { packNumbers } from "../../helpers/lottery-helpers.js";

const packedNumbers = packNumbers(numbers);
const permitSignature = await generatePermitSignature(contracts.token, player, contracts.lottery.address, totalCost, undefined, chainId);
```

### Permit Signature Requirements

**Required Parameters**:

- `token`: Token contract instance (contracts.token)
- `walletClient`: Wallet client with account (player, provider)
- `spender`: Contract address receiving approval (contracts.lottery.address)
- `value`: Amount to approve (totalCost)
- `deadline`: Optional expiry timestamp (undefined = max deadline)
- `chainId`: Network chain ID (84532 for Base Sepolia)

**Signature Components**:

- `value`: Amount approved
- `deadline`: Expiry timestamp
- `v`, `r`, `s`: ECDSA signature components

### ChainId Management

**Always extract chainId from test environment**:

```typescript
const { contracts, publicClient, chainId } = testEnv;

// Pass chainId to all permit functions
await createLotteryWithPermit(contracts, publicClient, provider, {...}, chainId);
await buyTicketsWithPermit(contracts, publicClient, player, lotteryId, numbers, undefined, chainId);
```

**Why chainId matters**:

- EIP-2612 signatures are chain-specific for security
- Prevents replay attacks across networks
- Base Sepolia chainId = 84532
- Local Hardhat chainId = 31337

### Stress Testing Permits

**Validate permit reliability with consecutive runs**:

```bash
bash apps/hardhat/run-testnet-stress-test.sh
```

**Expected outcome**:

- 100% success rate across 5+ consecutive runs
- No flakiness or signature failures
- Consistent gas usage patterns
- All financial invariants validated

---

## Validation Toolkit

### Transaction Confirmation

```typescript
await publicClient.waitForTransactionReceipt({
  hash: txHash,
  confirmations: 1,
  timeout: 20_000,
});
```

### Contract State

Example win-path assertions (adjust expectations when testing loss or expiration flows):

```typescript
const info = await contracts.lottery.read.getLotteryInfo([lotteryId]);
expect(info.status, "lottery must be ended after VRF").to.equal(LOTTERY_STATUS.ENDED);
expect(info.hasWinner, "VRF callback should mark winner").to.equal(true);
expect(info.winner?.toLowerCase(), "winner address mismatch").to.equal(player.account.address.toLowerCase());
```

### Event Verification

Example win-path event validation:

```typescript
const logs = await waitForEventLog(publicClient, contracts.lottery.address, LOTTERY_RESULT_EVENT, receipt.blockNumber, "lottery result", { lotteryId });

const result = logs.at(-1)?.args;
expect(result?.won, "result event should mark win").to.equal(true);
expect(result?.netPrizeAwarded, "result event prize amount mismatch").to.equal(prizeAmount);
```

### Subgraph Cross-Check

Ensure indexed data matches on-chain totals:

```typescript
await waitForSubgraphSync(publicClient, 6, 15_000, 2_000);

const { lottery } = await querySubgraph(GET_LOTTERY_FINANCIAL, { lotteryId: lotteryId.toString() });
expect(lottery, "lottery should exist in subgraph").to.not.be.undefined;
expect(BigInt(lottery!.grossRevenue), "subgraph gross revenue mismatch").to.equal(totalTicketCost);
```

### VRF Completion Without Explicit Delays

```typescript
const waitForVRFCompletion = async (lotteryId: bigint, maxAttempts = 10) => {
  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    const pending = await contracts.lottery.read.getPendingRequestCount([lotteryId]);
    if (pending === 0n) return;
    await waitForSubgraphSync(publicClient, 1, 5_000, 1_000);
  }
  throw new Error(`VRF request still pending for lottery ${lotteryId}`);
};
```

This loop honors eventual consistency and stops as soon as observable evidence confirms completion.

---

## Financial Reconciliation Pattern

```typescript
const treasuryBefore = await contracts.token.read.balanceOf([treasuryAddress]);
const providerBefore = await contracts.token.read.balanceOf([provider.account.address]);
const playerBefore = await contracts.token.read.balanceOf([player.account.address]);

// ... scenario ...

const treasuryAfter = await contracts.token.read.balanceOf([treasuryAddress]);
const providerAfter = await contracts.token.read.balanceOf([provider.account.address]);
const playerAfter = await contracts.token.read.balanceOf([player.account.address]);

const platformFee = (ticketCost * 500n) / 10_000n;
const netRevenue = ticketCost - platformFee;

expect(treasuryAfter - treasuryBefore, "platform fee mismatch").to.equal(platformFee);
expect(providerAfter - providerBefore, "provider net change mismatch").to.equal(netRevenue - prizeAmount);
expect(playerAfter - playerBefore, "player prize payout mismatch").to.equal(prizeAmount - ticketCost);

const totalDelta = treasuryAfter - treasuryBefore + (providerAfter - providerBefore) + (playerAfter - playerBefore);
expect(totalDelta, "token conservation violated").to.equal(0n);
```

Always verify every participant whenever value transfers occur.

---

## Error & Logging Discipline

- Allow native errors to propagate; never wrap them in new `Error` instances.
- Log only when it adds actionable context. When logging, rethrow the original error unchanged.

```typescript
try {
  await publicClient.waitForTransactionReceipt({ hash, confirmations: 1, timeout: 15_000 });
} catch (error) {
  testLogger.error("Transaction confirmation failed", { hash, error });
  throw error; // preserve native Viem error details
}
```

- Avoid info-level chatter. Failures should be the only noisy path.
- Do not swallow stack traces or override revert reasons—contract errors are the richest diagnostics you have.

---

## Assertion Standards

- Every `expect` includes a descriptive message (`expect(value, "why this matters").to.equal(expected)`).
- Assertions cover contract state, emitted events, subgraph data, wallet balances, AND external evidence sources.
- Financial checks use exact equality—no tolerance-based helpers.
- Every assertion must be traceable to a specification requirement or contract invariant.

### Assertion Strength Guidelines

**Weak Assertions (Avoid)**:

```typescript
// ❌ Too vague - doesn't verify exact amount
expect(balance).to.be.greaterThan(0n);

// ❌ Doesn't verify correct state
expect(lottery.status).to.not.equal(LOTTERY_STATUS.ACTIVE);

// ❌ Any revert passes - doesn't verify correct error
await expect(buyTicket()).to.be.reverted;
```

**Strong Assertions (Prefer)**:

```typescript
// ✅ Exact value with evidence
expect(balance, "player balance should equal prize minus ticket cost").to.equal(prizeAmount - ticketPrice);

// ✅ Exact expected state
expect(lottery.status, "lottery must be ended after VRF").to.equal(LOTTERY_STATUS.ENDED);

// ✅ Specific error with selector
await expect(buyTicket()).to.be.revertedWithCustomError(lottery, "AlreadyHasWinner");
```

---

## Specification Alignment

**Before writing any test, answer these questions:**

- What is the SPECIFICATION for this functionality?
- What is the BUSINESS REQUIREMENT this contract should fulfill?
- What should happen according to the DESIGN DOCUMENT?
- What is the EXPECTED BEHAVIOR from the user perspective?
- What economic outcome is required?

**After reviewing requirements, validate:**

- Does the implementation match the specification?
- Are there gaps between spec and code?
- Is the contract doing what it should do, or merely what it currently does?

**When specification is unclear:**

1. Identify the ambiguity and document all plausible interpretations
2. Note which interpretation the contract currently implements
3. Analyze security, economic, and UX implications for each interpretation
4. Recommend a resolution with reasoning and risk assessment
5. Get clarification from product/design; request spec updates
6. Document the decision and adjust tests accordingly

---

## Prohibited Patterns

- ❌ Mocking or simulating contracts, VRF providers, or subgraph responses.
- ❌ Using traditional approve/transferFrom pattern when EIP-2612 permits are available.
- ❌ Inline permit objects (`emptyPermit` with zero values) instead of using `generatePermitSignature()` helper.
- ❌ Manual number packing (`0x${numbers.map(...).join("")}`) instead of using `packNumbers()` helper.
- ❌ `sleep`, `setTimeout`, or other explicit delays that are not driven by observable state.
- ❌ Wrapping or replacing errors thrown by Viem or the EVM.
- ❌ Logging progress for routine operations.
- ❌ Partial validation that ignores any affected actor.
- ❌ Assertions without external evidence validation.
- ❌ Testing what the contract DOES instead of what it SHOULD do.
- ❌ Accepting passing tests without proving they would fail if contract breaks.
- ❌ Individual `this.timeout()` calls in test files—use global Mocha configuration.
- ❌ Custom poll option overrides (`maxAttempts`, `maxDelayMs`)—use helper defaults.
- ❌ Large token amounts (>100 USDC) that cause wallet depletion across test suites.
- ❌ Timeouts longer than 15 seconds per test—enforce fast-fail behavior.

---

## Timeout & Performance Standards

### Global Timeout Configuration

**Mocha Configuration** (`hardhat.config.ts`):

```typescript
test: {
  mocha: {
    timeout: 15000,  // 15 seconds max per test
    bail: true,      // Stop on first failure
  },
},
```

**Rationale:**

- Prevents hanging tests from blocking CI/CD pipelines
- Forces efficient test design
- Provides fast feedback on failures
- Ensures tests complete within reasonable time

### Polling Strategy

**Subgraph Polling** (2-second intervals, 12-second max):

```typescript
// querySubgraph() - polls for up to 12 seconds with 2-second intervals
const data = await querySubgraph(GET_LOTTERY_FINANCIAL, { lotteryId });

// waitForSubgraphSync() - polls for up to 12 seconds with 2-second intervals
await waitForSubgraphSync(publicClient);
```

**State Verification Polling** (aggressive defaults):

```typescript
// DEFAULT_POLL_OPTIONS: 8 attempts, 100-1000ms delays
// VRF_POLL_DEFAULTS: 12 attempts, 300-1500ms delays
// EVENT_POLL_DEFAULTS: 10 attempts, 200-1200ms delays
```

**Transaction Receipt Timeouts:**

- Standard receipts: 8 seconds
- Lottery creation: 10 seconds
- Wallet funding: 8 seconds

**Rationale:**

- 2-second intervals balance responsiveness with network load
- Automatic retries handle indexing delays gracefully
- Exponential backoff prevents API rate limiting
- Timeouts prevent infinite waiting

### Test Value Standards

**Standard Token Amounts:**

```typescript
const STANDARD_VALUES = {
  prizeAmount: 10_000_000n, // 10 USDC (with 6 decimals)
  ticketPrice: 100_000n, // 0.1 USDC
  providerBalance: 50_000_000n, // 50 USDC
  playerBalance: 5_000_000n, // 5 USDC
};
```

**Rationale:**

- Small values prevent wallet depletion across test suites
- Consistent values make financial flows predictable
- Reduces test execution time
- Prevents balance-related test failures

### Performance Requirements

- **Per-test maximum**: 15 seconds
- **Full suite target**: <3 minutes for 35 tests
- **Subgraph sync**: <12 seconds
- **VRF callback**: <10 seconds
- **Transaction confirmation**: <8 seconds

**Enforcement:**

- Global Mocha timeout: 15 seconds
- Bail on first failure
- No individual timeout overrides
- Aggressive polling defaults

---

## Acceptance Checklist

- [ ] Test auto-skips when not running on Base Sepolia.
- [ ] Wallet balances are verified and topped up only when insufficient.
- [ ] Transactions target deployed addresses and use real VRF callbacks.
- [ ] Uses EIP-2612 permits (`createLotteryWithPermit`, `buyTicketsWithPermit`) instead of approve/transferFrom.
- [ ] No inline `emptyPermit` objects—uses `generatePermitSignature()` helper.
- [ ] No manual number packing—uses `packNumbers()` helper.
- [ ] ChainId is extracted from test environment and passed to all permit functions.
- [ ] Assertions validate contract reads, events, subgraph indices, AND external evidence sources.
- [ ] Every assertion is cross-validated with at least one external source (Etherscan API or subgraph).
- [ ] VRF completion is confirmed through state-based polling—not explicit delays.
- [ ] Financial conservation is proven for treasury, provider, player, and platform.
- [ ] All token flows are traced end-to-end with exact calculations documented.
- [ ] Break-first thought experiment performed for critical assertions.
- [ ] Edge cases and failure modes are explicitly tested.
- [ ] Logging is limited to high-value errors; no redundant info logs.
- [ ] `expect()` statements include descriptive messages and exact expected values.
- [ ] Native errors surface intact; no wrapping or suppression.
- [ ] No redundant approvals, mocks, or ad-hoc shortcuts remain.
- [ ] Test would fail if corresponding contract logic is removed or broken.
- [ ] Test completes within 15 seconds (global timeout enforced).
- [ ] No individual `this.timeout()` calls present.
- [ ] Uses standard token amounts (10 USDC prize, 0.1 USDC ticket).
- [ ] Subgraph queries use 2-second polling intervals with automatic retries.
- [ ] No custom poll option overrides—uses helper defaults.
- [ ] Transaction receipts use 8-10 second timeouts.
- [ ] Stress test passes with 100% success rate across 5+ consecutive runs.

All items must be satisfied before a testnet scenario is considered production-grade.
