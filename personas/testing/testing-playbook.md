---
type: "guide"
description: "Testing Playbook - Patterns & How to Add New Tests"
---

# Testing Playbook (Local Hardhat + Viem)

This playbook distills our mandatory patterns for adding production‑grade tests. It complements `ai-kit/personas/testing/local_testing_persona.md` (principles) with concise how‑tos and snippets.

## Golden Rules

- AAA structure only: Arrange → Act → Assert.
- Use chain time, never `Date.now()`.
- Use block‑scoped `publicClient.getLogs` with explicit ABI for events.
- Assert exact wei values (no approximations).
- Verify ALL affected parties (player, provider, treasury, ReferralManager, referrer).
- One behavior per test; no multi‑action “kitchen sinks”.

## Snippets

### 1) Setup Contracts & Wallets

```ts
const { contracts, publicClient, viem } = await setupLocalEnvironment();
const [owner, provider, player, referrer] = (await viem.getWalletClients()).slice(0, 4);
await setupTokensForWallet(contracts, publicClient, owner, provider, 20_000_000n);
await setupTokensForWallet(contracts, publicClient, owner, player, 1_000_000n);
```

### 2) Create Lottery & Buy Tickets

```ts
const { lotteryId } = await createLotteryNative(contracts, publicClient, provider, {
  prizeAmount: 10_000_000n, ticketPrice: 100_000n, pickRange: 10, duration: 3600,
});

const vrfFee = await contracts.lottery.read.getEntropyFee();
await contracts.token.write.approve([contracts.lottery.address, 100_000n], { account: player.account });
const emptyPermit = { value: 0n, deadline: 0n, v: 0, r: ZERO32, s: ZERO32 } as const;
const buyTx = await contracts.lottery.write.buyTicketsPacked([lotteryId, "0x000005", referrer.account.address, deadline, signature, emptyPermit], { account: player.account, value: vrfFee });
await publicClient.waitForTransactionReceipt({ hash: buyTx, confirmations: 1 });
```

### 3) Fulfill VRF Deterministically

```ts
const seq = await getLatestMockSequenceNumber(contracts.mockEntropy);
const fulfillTx = await fulfillMockVRFWithLoss(contracts.mockEntropy, publicClient, player, seq, 10);
const fulfillRcpt = await publicClient.waitForTransactionReceipt({ hash: fulfillTx, confirmations: 1 });
```

### 4) Event Assertions (Block-Scoped)

```ts
const logs = await publicClient.getLogs({
  address: contracts.referralManager.address,
  fromBlock: fulfillRcpt.blockNumber,
  toBlock: fulfillRcpt.blockNumber,
  event: {
    type: "event",
    name: "ReferralCommissionRecorded",
    inputs: [
      { type: "address", name: "referrer", indexed: true },
      { type: "address", name: "referee", indexed: true },
      { type: "address", name: "token", indexed: true },
      { type: "uint256", name: "commission" },
      { type: "uint256", name: "lotteryId" },
    ],
  },
});
expect(logs.length).to.equal(1);
```

### 5) Wei-Exact Financials

```ts
const platformFee = (ticketPrice * 500n) / 10_000n; // 5%
const expectedCommission = (platformFee * 100n) / 10_000n; // 1%
expect(await contracts.referralManager.read.referralEarnings([referrer.account.address, token])).to.equal(expectedCommission);
```

## Adding New Tests

1) Choose scope: unit (fast behavior focus) vs E2E (cross‑contract flows).
2) Start from a minimal working copy of the closest existing test file.
3) Enforce AAA & single‑behavior focus.
4) Always add: balances of all parties, exact events with args, and state reads.
5) Prefer `simulateContract` for non‑view readbacks of mutating methods.
6) Use helpers: `createLotteryNative`, `setupTokensForWallet`, `getLatestMockSequenceNumber`, fulfillment helpers.

## Adversarial Patterns

- Invalid/malleable signatures → expect zero attribution & no events.
- Out‑of‑order fulfillments → first fulfillment wins lock.
- Rate change between buy & fulfill → assert committed rate at fulfill.
- Refund path (hasWinner=true) → no referral effects.
- ERC‑1271 non‑compliant → no referral effects (even after manager swap).
- Malicious manager → reverting manager should not break buys; forced referrer shows admin risk.

## Constants

```ts
export const ZERO32 = "0x" + "0".repeat(64) as const;
```

Keep tests small, deterministic, and evidence‑backed. If a behavior is ambiguous, document and escalate in spec.

