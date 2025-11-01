# Zero Complexity Web3 Hooks

## Native-First Pattern Guide

> **Purpose:** This guide defines the patterns and principles for creating simple, maintainable Web3 hooks using Wagmi and TanStack Query. Use this as your reference when creating new hooks or refactoring existing ones.

---

## Core Principles

### 1. **Native-First: No Custom Abstractions**

**Always return native Wagmi/TanStack Query objects directly.**

- ✅ Return `useReadContract()` as-is
- ✅ Return `useMutation()` as-is
- ✅ Return `useQuery()` as-is
- ❌ Never wrap or transform return objects
- ❌ Never rename properties (data, isLoading, error)

**Why:** Native returns are well-documented, type-safe, and familiar to all developers.

### 2. **Single Responsibility: One Thing Per Hook**

**Each hook does exactly ONE thing.**

- **Read Hooks:** Only read blockchain data
- **Mutation Hooks:** Only execute blockchain writes
- **Orchestration Hooks:** Only coordinate multiple operations
- **Utility Hooks:** Only provide specialized utilities

**Why:** Single-purpose hooks are easier to test, reuse, and understand.

### 3. **Three-Layer Architecture**

**Separate concerns into three clear layers:**

```
Layer 1: Mutation Hooks (Pure)
├─ Just the blockchain operation
├─ No state, no effects, no toasts
└─ Returns native useMutation object

Layer 2: Orchestration Hooks (Coordination)
├─ Coordinates multiple operations
├─ Handles receipts, polling, state
├─ Shows toast notifications
└─ Used by forms and pages

Layer 3: Components (Presentation)
├─ Handles formatting and display
├─ Manages user interactions
└─ Uses hooks from layers 1 & 2
```

**Why:** Clear separation makes code predictable and maintainable.

### 4. **No Hidden Complexity**

**Avoid these patterns in mutation hooks:**

- ❌ `useState` or `useRef` for transaction state
- ❌ Multiple `useEffect` chains
- ❌ Toast notifications
- ❌ Receipt waiting or polling
- ❌ Data transformation or formatting
- ❌ Custom error handling

**Why:** These concerns belong in orchestration hooks or components.

---

## Quick Reference: Hook Patterns

### Pattern 1: Read Hook (Native Return)

```typescript
// ✅ CORRECT: Returns native useReadContract object
export function useUSDCBalance() {
  const { address } = useAccount();

  return useReadContract({
    address: CONTRACTS.USDC.address,
    abi: USDC_ABI,
    functionName: "balanceOf",
    args: address ? [address] : undefined,
    query: {
      enabled: !!address,
      staleTime: 30_000,
    },
  });
}

// Component usage
function MyComponent() {
  const { data: balance = 0n, isLoading } = useUSDCBalance();
  const formatted = formatUSDC(balance); // ✅ Format in component
  return <div>{formatted}</div>;
}
```

### Pattern 2: Mutation Hook (Pure)

```typescript
// ✅ CORRECT: Pure mutation, no side effects
export function useApproveUSDCMutation() {
  const { address } = useAccount();
  const { writeContractAsync } = useWriteContract();

  return useMutation({
    mutationFn: async ({ spender, amount }) => {
      if (!address) throw new Error("Wallet not connected");

      const hash = await writeContractAsync({
        address: CONTRACTS.USDC.address,
        abi: USDC_ABI,
        functionName: "approve",
        args: [spender, amount],
      });

      return { hash };
    },
  });
}

// Component usage
function MyComponent() {
  const approve = useApproveUSDCMutation();

  const handleApprove = async () => {
    try {
      await approve.mutateAsync({ spender, amount });
      toast.success("Approved!"); // ✅ Toast in component
    } catch (error) {
      toast.error("Failed"); // ✅ Error handling in component
    }
  };

  return <button onClick={handleApprove}>Approve</button>;
}
```

### Pattern 3: Orchestration Hook (Coordination)

```typescript
// ✅ CORRECT: Orchestrates multiple operations
export function useCreateLotteryForm() {
  const [txHash, setTxHash] = useState<Hash>();
  const [lotteryId, setLotteryId] = useState<string>();

  const createMutation = useCreateLotteryMutation(); // ✅ Use pure mutation

  // ✅ Orchestration: Wait for receipt
  const { data: receipt } = useWaitForTransactionReceipt({ hash: txHash });

  // ✅ Orchestration: Poll for indexing
  const { data: indexed } = useQuery({
    queryKey: ["lottery", lotteryId],
    enabled: !!lotteryId,
    refetchInterval: 2000,
  });

  // ✅ Orchestration: Handle receipt
  useEffect(() => {
    if (receipt) {
      const id = extractIdFromLogs(receipt.logs);
      setLotteryId(id);
    }
  }, [receipt]);

  // ✅ Orchestration: Handle success
  useEffect(() => {
    if (indexed && lotteryId) {
      toast.success(`Lottery #${lotteryId} created!`);
      setTxHash(undefined);
      setLotteryId(undefined);
    }
  }, [indexed, lotteryId]);

  const handleSubmit = async (params) => {
    const hash = await createMutation.mutateAsync(params);
    setTxHash(hash);
    toast.success("Transaction submitted");
  };

  return {
    handleSubmit,
    isCreating: createMutation.isPending,
    isWaitingForReceipt: !!txHash && !receipt,
    isWaitingForIndexing: !!lotteryId && !indexed,
  };
}
```

---

## Anti-Patterns: What NOT to Do

### ❌ DON'T: Custom Return Objects

```typescript
// ❌ WRONG: Custom return object
export function useBalance() {
  const { data, isLoading, error } = useReadContract({...});

  return {
    balance: data,           // ❌ Renamed
    loading: isLoading,      // ❌ Renamed
    hasError: !!error,       // ❌ Transformed
    errorMsg: error?.message // ❌ Transformed
  };
}

// ✅ CORRECT: Native return
export function useBalance() {
  return useReadContract({...}); // ✅ Returns { data, isLoading, error, ... }
}
```

### ❌ DON'T: Data Transformation in Hooks

```typescript
// ❌ WRONG: Formatting in hook
export function useBalance() {
  const { data } = useReadContract({...});
  return {
    formatted: formatUSDC(data), // ❌ Transformation in hook
  };
}

// ✅ CORRECT: Formatting in component
export function useBalance() {
  return useReadContract({...}); // ✅ Return raw data
}

function Component() {
  const { data } = useBalance();
  const formatted = formatUSDC(data); // ✅ Format in component
}
```

### ❌ DON'T: Side Effects in Mutation Hooks

```typescript
// ❌ WRONG: Toast in mutation
export function useMyMutation() {
  return useMutation({
    mutationFn: async () => {...},
    onSuccess: () => toast.success("Done!"), // ❌ Toast in mutation
  });
}

// ✅ CORRECT: Toast in component/orchestration
export function useMyMutation() {
  return useMutation({
    mutationFn: async () => {...}, // ✅ Pure mutation
  });
}

function Component() {
  const mutation = useMyMutation();

  const handleClick = async () => {
    await mutation.mutateAsync();
    toast.success("Done!"); // ✅ Toast in component
  };
}
```

### ❌ DON'T: State in Mutation Hooks

```typescript
// ❌ WRONG: State in mutation
export function useMyMutation() {
  const [txHash, setTxHash] = useState(); // ❌ State in mutation

  return useMutation({
    mutationFn: async () => {
      const hash = await writeContract();
      setTxHash(hash); // ❌ Side effect
    },
  });
}

// ✅ CORRECT: State in orchestration
export function useMyForm() {
  const [txHash, setTxHash] = useState(); // ✅ State in orchestration
  const mutation = useMyMutation();

  const handleSubmit = async () => {
    const hash = await mutation.mutateAsync();
    setTxHash(hash); // ✅ Handle in orchestration
  };
}
```

---

## Key Learnings from Our Refactoring

### Learning 1: Orchestration Belongs in Form Hooks

**Discovery:** Form hooks are the perfect place for complex orchestration logic.

**Pattern:**

- ✅ Mutation hooks stay pure (just the blockchain operation)
- ✅ Form hooks handle: receipts, polling, toasts, state management
- ✅ Components stay focused on presentation

**Example:**

```typescript
// ✅ Pure mutation
export function useCreateLotteryMutation() {
  return useMutation({
    mutationFn: async (params) => {
      return await writeContractAsync({...});
    },
  });
}

// ✅ Orchestration in form hook
export function useCreateLotteryForm() {
  const [txHash, setTxHash] = useState();
  const mutation = useCreateLotteryMutation();

  const handleSubmit = async (params) => {
    const hash = await mutation.mutateAsync(params);
    setTxHash(hash);
    toast.success("Submitted!");
  };

  return { handleSubmit, isCreating: mutation.isPending };
}
```

### Learning 2: Incremental Refactoring Is Safe

**Discovery:** Refactoring one hook at a time with testing prevents regressions.

**Process:**

1. Identify one complex hook
2. Refactor following native-first patterns
3. Run all tests (especially E2E)
4. Commit if tests pass
5. Move to next hook

**Result:** Zero regressions across 5 refactoring phases.

### Learning 3: Native Returns Are More Flexible

**Discovery:** Returning native objects gives components more control.

**Before:**

```typescript
// ❌ Hook decides formatting
export function useBalance() {
  const { data } = useReadContract({...});
  return { formatted: formatUSDC(data) }; // ❌ Locked into one format
}
```

**After:**

```typescript
// ✅ Component decides formatting
export function useBalance() {
  return useReadContract({...}); // ✅ Returns raw data
}

// Component A: Full precision
const { data } = useBalance();
const display = formatUSDC(data, 6); // 6 decimals

// Component B: Rounded
const { data } = useBalance();
const display = formatUSDC(data, 2); // 2 decimals
```

### Learning 4: Extract Reusable Reads

**Discovery:** Common reads should be separate hooks for reusability.

**Pattern:**

```typescript
// ✅ Reusable read hook
export function useLotteryVrfFee() {
  return useReadContract({
    address: CONTRACTS.LOTTERY.address,
    abi: LOTTERY_ABI,
    functionName: "getEntropyFee",
    query: { staleTime: 60_000 },
  });
}

// ✅ Used in mutation
export function useBuyTicketsMutation() {
  const { data: vrfFee } = useLotteryVrfFee();
  return useMutation({...});
}

// ✅ Also used in component
function TicketPrice() {
  const { data: vrfFee } = useLotteryVrfFee();
  return <div>VRF Fee: {vrfFee}</div>;
}
```

### Learning 5: Testing Catches Everything

**Discovery:** Comprehensive E2E tests catch all regressions.

**Our Results:**

- 7/7 smoke tests passing after every phase
- Zero regressions detected
- 100% functionality preserved

**Lesson:** Always run full test suite after refactoring.

---

## Real-World Examples from Our Refactoring

### Example 1: Balance Hook - Remove Data Transformation

**Problem:** Hook was formatting data, limiting component flexibility.

#### ❌ BEFORE (56 lines)

```typescript
export function useUSDCBalance() {
  const { address } = useAccount();
  const balanceQuery = useReadContract({...});
  const balance = balanceQuery.data ?? 0n;

  return {
    balance,
    formattedBalance: formatUSDC(balance), // ❌ Transformation in hook
    ...balanceQuery,
  };
}
```

#### ✅ AFTER (37 lines - 34% reduction)

```typescript
export function useUSDCBalance() {
  const { address } = useAccount();

  return useReadContract({
    address: CONTRACTS.USDC.address,
    abi: USDC_ABI,
    functionName: "balanceOf",
    args: address ? [address] : undefined,
    query: {
      enabled: !!address,
      staleTime: 30_000,
    },
  });
}

// Component usage
function WalletDisplay() {
  const { data: balance = 0n } = useUSDCBalance();
  const formatted = formatUSDC(balance); // ✅ Format in component
  return <div>{formatted}</div>;
}
```

**Results:**

- ✅ 34% less code
- ✅ Native return object
- ✅ Components control formatting
- ✅ More flexible and reusable

---

### Example 2: Create Lottery - Split Mutation from Orchestration

**Problem:** One hook doing 7 different things (mutation, receipt, polling, toasts, state, timeout, error handling).

#### ❌ BEFORE (159 lines - Complexity 10/10)

```typescript
export function useCreateLottery() {
  const [txHash, setTxHash] = useState(); // ❌ State
  const [lotteryId, setLotteryId] = useState(); // ❌ State

  // ❌ Receipt waiting
  const { data: receipt } = useWaitForTransactionReceipt({ hash: txHash });

  // ❌ Extract ID from logs
  useEffect(() => {
    if (receipt) {
      const id = extractId(receipt.logs);
      setLotteryId(id);
      toast.loading("Indexing..."); // ❌ Toast
    }
  }, [receipt]);

  // ❌ Polling for indexing
  const { data: indexed } = useQuery({
    queryKey: ["lottery", lotteryId],
    enabled: !!lotteryId,
    refetchInterval: 2000,
  });

  // ❌ Success handling
  useEffect(() => {
    if (indexed) {
      toast.success("Created!"); // ❌ Toast
      setTxHash(undefined);
      setLotteryId(undefined);
    }
  }, [indexed]);

  // ❌ Timeout handling
  useEffect(() => {
    if (!lotteryId) return;
    const timeout = setTimeout(() => {
      toast.error("Timeout!"); // ❌ Toast
    }, 30_000);
    return () => clearTimeout(timeout);
  }, [lotteryId]);

  // ❌ Mutation with side effects
  const mutation = useMutation({
    mutationFn: async (params) => {
      const hash = await writeContractAsync({...});
      setTxHash(hash); // ❌ Side effect
      return hash;
    },
    onSuccess: () => toast.success("Submitted!"), // ❌ Toast
  });

  return {
    ...mutation,
    isWaitingForReceipt: !!txHash && !receipt,
    isWaitingForIndexing: !!lotteryId && !indexed,
  };
}
```

**Problems:**

- ❌ 159 lines, 7 responsibilities
- ❌ 2 useState, 5 useEffect
- ❌ Toasts in 4 places
- ❌ Receipt + polling + timeout in mutation
- ❌ Complexity: 10/10

#### ✅ AFTER: Simple mutation hook (39 lines)

```typescript
export function useCreateLotteryMutation(): UseMutationResult<Hash, Error, CreateLotteryParams> {
  const { writeContractAsync } = useWriteContract();

  return useMutation({
    mutationFn: async (params: CreateLotteryParams) => {
      const { prizeAmount, ticketPrice, pickRange, durationMins } = params;

      const convertedParams = {
        prizeAmountBigInt: toUSDC(prizeAmount),
        ticketPriceBigInt: toUSDC(ticketPrice),
        pickRangeBigInt: BigInt(pickRange),
        durationBigInt: minutesToSeconds(durationMins),
      };

      const hash = await writeContractAsync({
        address: CONTRACTS.LOTTERY.address,
        abi: LOTTERY_ABI,
        functionName: "createLottery",
        args: toCreateLotteryArgs({
          prizeToken: CONTRACTS.USDC.address,
          prizeAmount: convertedParams.prizeAmountBigInt,
          ticketPrice: convertedParams.ticketPriceBigInt,
          pickRange: convertedParams.pickRangeBigInt,
          duration: convertedParams.durationBigInt,
        }),
      });

      return hash;
    },
  });
}
```

**Orchestration moved to form hook:**

```typescript
export function useCreateLotteryForm() {
  // Form state
  const [prizeAmount, setPrizeAmount] = useState(0);
  const [txHash, setTxHash] = useState<`0x${string}` | undefined>();
  const [createdLotteryId, setCreatedLotteryId] = useState<string | null>(null);

  // Use simple mutation
  const createMutation = useCreateLotteryMutation();

  // Orchestration: receipt waiting
  const { isLoading: isConfirming, data: receipt } = useWaitForTransactionReceipt({
    hash: txHash,
  });

  // Orchestration: indexing polling
  const { data: indexedLottery } = useQuery({
    ...lotteryByIdOptions(createdLotteryId || ""),
    enabled: !!createdLotteryId,
    refetchInterval: 2000,
  });

  // Orchestration: extract ID from receipt
  useEffect(() => {
    if (receipt) {
      const id = extractLotteryCreatedId(receipt.logs, LOTTERY_ABI);
      setCreatedLotteryId(id);
    }
  }, [receipt]);

  // Orchestration: success handling
  useEffect(() => {
    if (indexedLottery && createdLotteryId) {
      toast.success(`Lottery #${createdLotteryId} created`);
      setTxHash(undefined);
      setCreatedLotteryId(null);
    }
  }, [indexedLottery, createdLotteryId]);

  const handleSubmit = async () => {
    const hash = await createMutation.mutateAsync({ prizeAmount, ... });
    setTxHash(hash);
    toast.success("Transaction submitted");
  };

  return {
    prizeAmount,
    setPrizeAmount,
    handleSubmit,
    isCreating: createMutation.isPending,
    isConfirming,
    isWaitingForIndexing: !!createdLotteryId && !indexedLottery,
  };
}
```

**Benefits:**

- ✅ 75% less code in mutation hook (159 → 39 lines)
- ✅ Mutation hook is pure and reusable
- ✅ Returns native useMutation object
- ✅ No useState/useRef in mutation
- ✅ No useEffect in mutation
- ✅ No toast notifications in mutation
- ✅ Orchestration properly separated
- ✅ Complexity score: 2/10

---

### Example 3: Buy Tickets - Remove Toast from Hook

**Problem:** Hook was showing toasts, making it less reusable.

#### ❌ BEFORE (88 lines)

```typescript
export function useBuyTickets() {
  const mutation = useBuyTicketsMutation();

  const buyTickets = async (params) => {
    try {
      await mutation.mutateAsync(params);
      toast.success("Purchase confirmed!"); // ❌ Toast in hook
    } catch (error) {
      toast.error("Purchase failed"); // ❌ Toast in hook
    }
  };

  return { buyTickets, isLoading: mutation.isPending };
}
```

#### ✅ AFTER (74 lines - 16% reduction)

```typescript
export function useBuyTickets() {
  const mutation = useBuyTicketsMutation();

  const buyTickets = async (params) => {
    await mutation.mutateAsync(params); // ✅ Just execute, no toast
  };

  return { buyTickets, isLoading: mutation.isPending };
}

// Component handles toasts
function QuickPlayModal() {
  const { buyTickets } = useBuyTickets();

  const handlePurchase = async () => {
    try {
      await buyTickets(params);
      toast.success("Purchase confirmed!"); // ✅ Toast in component
    } catch (error) {
      toast.error("Purchase failed"); // ✅ Toast in component
    }
  };
}
```

**Results:**

- ✅ 16% less code (88 → 74 lines)
- ✅ Hook is more reusable
- ✅ Components control user feedback
- ✅ Follows three-layer architecture

---

## Decision Tree: Where Does Logic Belong?

```
Is it reading blockchain data?
├─ YES → Use useReadContract directly
│         Return native object
│         No transformation
│
└─ NO → Is it writing to blockchain?
    ├─ YES → Is it JUST the mutation?
    │   ├─ YES → Use useMutation + useWriteContract
    │   │         Return native object
    │   │         No side effects
    │   │
    │   └─ NO → Does it need orchestration?
    │       └─ YES → Create orchestration hook (form/page level)
    │                Handle: receipts, toasts, state, polling
    │
    └─ NO → Is it formatting/transformation?
        └─ YES → Do it in the component
                 Not in the hook
```

---

## Summary: The Three-Layer Architecture

### Layer 1: Mutation Hooks (Pure Business Logic)

- **Purpose:** Execute blockchain operations
- **Contains:** useMutation + useWriteContract
- **Returns:** Native mutation object
- **No:** State, useEffect, toasts, side effects

### Layer 2: Orchestration Hooks (Coordination)

- **Purpose:** Coordinate multiple operations
- **Contains:** State, useEffect, receipt waiting, polling
- **Returns:** Combined state and handlers
- **Examples:** Form hooks, page-level hooks

### Layer 3: Components (Presentation)

- **Purpose:** Display UI and handle user interactions
- **Contains:** Formatting, rendering, event handlers
- **Uses:** Both mutation and orchestration hooks

---

## Refactoring Checklist

### Step 1: Identify Complexity

Does the hook have any of these?

- [ ] useState/useRef for transaction state
- [ ] Multiple useEffect hooks
- [ ] Toast notifications
- [ ] Data transformation/formatting
- [ ] Custom return object (not native)
- [ ] Multiple responsibilities (reads + writes + orchestration)

### Step 2: Refactor

If YES to any above:

1. **Extract pure mutation** → New hook with just useMutation
2. **Move orchestration** → Form/page hook (receipts, polling, toasts)
3. **Move formatting** → Components
4. **Return native objects** → No custom wrappers
5. **Test thoroughly** → Run E2E tests after each change

### Step 3: Verify

- [ ] Mutation hooks are pure (no state/effects/toasts)
- [ ] Orchestration is in form/page hooks
- [ ] All hooks return native objects
- [ ] All tests passing
- [ ] Code is simpler and shorter

---

## Acceptable Exceptions

While we strive for zero complexity, some patterns are acceptable exceptions when they provide clear value without adding unnecessary abstraction.

### Event Listener Hooks with Toast Notifications

Event listener hooks that watch for blockchain events **may** contain toast notifications when:

1. **Event-Driven (Not Action-Driven):** The toasts respond to blockchain events, not user actions
2. **Utility Hook (Not Mutation):** The hook is a specialized utility, not a mutation hook
3. **Tightly Coupled:** The toasts are directly related to the event being watched
4. **Complexity Trade-off:** Moving toasts would add more complexity than it removes

**Example: `use-lottery-result-listener.ts`**

```typescript
export function useLotteryResultListener(options = {}) {
  const publicClient = usePublicClient();

  const listenForLotteryResult = useCallback(
    (params) => {
      const unwatch = publicClient.watchContractEvent({
        eventName: "LotteryResult",
        onLogs: (logs) => {
          const result = parseResult(logs[0]);

          if (result.won) {
            toast.success(`🎉 You Won! Prize: ${result.prize}`);
          } else {
            toast.info(`Better luck next time!`);
          }

          params.onResult(result);
          unwatch();
        },
      });
    },
    [publicClient],
  );

  return { listenForLotteryResult };
}
```

**Why This Is Acceptable:**

- ✅ The hook is a specialized event listener utility
- ✅ Toasts are event-driven (blockchain event → toast)
- ✅ The toasts are tightly coupled to the lottery result
- ✅ Moving toasts to components would require complex callback chains
- ✅ The pattern is clear and maintainable
- ✅ It's well-abstracted from components

**When NOT Acceptable:**

- ❌ Mutation hooks showing toasts (use components instead)
- ❌ Read hooks showing toasts (no side effects in reads)
- ❌ Toasts for user actions (belongs in components)
- ❌ Toasts that could easily be in components

**Documentation Requirement:**

If you create an exception, document it clearly:

```typescript
/**
 * Event listener utility hook for lottery results.
 *
 * NOTE: This hook contains toast notifications, which is an exception to our
 * "no toasts in hooks" rule. This is acceptable because:
 * 1. It's an event-driven utility, not a mutation hook
 * 2. The toasts are tightly coupled to blockchain events
 * 3. Moving them would add complexity without benefit
 * 4. The hook is well-abstracted from components
 */
export function useLotteryResultListener(...)
```

---

## Testing Your Refactoring

After refactoring, verify:

- ✅ All tests pass (especially E2E tests)
- ✅ Functionality is identical
- ✅ Code is simpler and shorter
- ✅ Hooks follow native patterns
- ✅ No custom abstractions remain
- ✅ Components work without changes (or minimal changes)
- ✅ Any exceptions are documented with clear justification

---

## Quick Reference Card

### When Creating a New Hook

**Ask yourself:**

1. **What does it do?**
   - Read data → Use `useReadContract`, return as-is
   - Write data → Use `useMutation` + `useWriteContract`, return as-is
   - Coordinate operations → Form/page hook with state + effects

2. **Does it need state/effects/toasts?**
   - NO → It's a mutation/read hook (pure)
   - YES → It's an orchestration hook (form/page level)

3. **Should it transform data?**
   - NO → Return raw data, let components format

4. **Should it combine operations?**
   - NO → One hook = one responsibility

### Red Flags

If you see these, refactor:

- ❌ Custom return object (not native)
- ❌ Data transformation in hook
- ❌ Toast notifications in mutation
- ❌ useState/useRef in mutation
- ❌ Multiple useEffect in mutation
- ❌ Combining reads + writes + orchestration

### Green Lights

These are good:

- ✅ Returns native Wagmi/TanStack object
- ✅ One clear responsibility
- ✅ No side effects in mutations
- ✅ Orchestration in form hooks
- ✅ Formatting in components
- ✅ Easy to test independently

### The Golden Rule

> **If you're not sure where logic belongs, ask: "Is this the blockchain operation itself, or is it handling the result?"**
>
> - **The operation** → Mutation hook
> - **Handling the result** → Orchestration hook or component

---

## Summary

This guide defines our patterns for Web3 hooks:

1. **Native-First:** Always return native objects
2. **Single Responsibility:** One hook = one thing
3. **Three Layers:** Mutation → Orchestration → Component
4. **No Hidden Complexity:** No state/effects/toasts in mutations
5. **Test Everything:** E2E tests catch all regressions

**Result:** Simple, maintainable, reusable hooks that follow industry best practices.

---

**Last Updated:** After 5-phase refactoring project (Phases 1-5 complete)
**Status:** Production-ready patterns, zero breaking changes, 100% test coverage
