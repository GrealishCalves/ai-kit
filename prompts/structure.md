# Folder Organization Refactoring Prompt

## Separate Constants, Types, Interfaces, Utils & Hooks

### Project Context

This is a **Vite + React + Web3 DApp** using:

- **Wagmi** - Ethereum interactions
- **TanStack Query** - Data fetching and caching
- **Viem** - Type-safe Ethereum utilities
- **Subgraph/GraphQL** - Blockchain data indexing

Current project structure:

```
src/
├── components/
├── config/           # Configuration files
├── constants/        # Constants (may be mixed)
├── features/         # Feature-based modules
├── hooks/           # React hooks
├── interfaces/      # Type definitions (may be mixed)
├── lib/             # Utility functions
├── providers/       # React providers
├── queries/         # TanStack Query definitions
├── routes/          # Page routes
├── types/           # TypeScript types
└── utils/           # Utility functions
```

---

## Refactoring Goal

**Organize code into dedicated folders with clear separation of concerns:**

1. **Constants** → Separate into dedicated files by domain
2. **Types & Interfaces** → Group by feature/domain
3. **Utilities** → Organize by functionality
4. **Hooks** → Separate by concern (blockchain, UI, data)

---

## Folder Structure Rules

### **1. Constants Organization**

```
src/constants/
├── index.ts                    # Re-export all constants
├── contracts.ts                # Contract addresses, ABIs
├── chains.ts                   # Chain IDs, RPC URLs
├── lottery.ts                  # Lottery-specific constants
├── ticket.ts                   # Ticket-specific constants
├── claim.ts                    # Claim-specific constants
├── evidence.ts                 # Evidence-specific constants
├── ui.ts                       # UI constants (colors, sizes, etc.)
└── validation.ts               # Validation rules, limits
```

**Rules:**

- ONE domain per file
- UPPERCASE for true constants: `LOTTERY_ADDRESS`, `MAX_TICKETS`
- Group related constants together
- Export as named exports, re-export from index.ts

**Example:**

```typescript
// src/constants/lottery.ts
export const LOTTERY_ADDRESS = "0x..." as const;
export const MIN_TICKET_PRICE = 1;
export const MAX_TICKET_PRICE = 1000;
export const DRAW_INTERVAL = 86400; // 24 hours

// src/constants/index.ts
export * from "./contracts";
export * from "./chains";
export * from "./lottery";
export * from "./ticket";
```

---

### **2. Types & Interfaces Organization**

```
src/types/
├── index.ts                    # Re-export all types
├── lottery.ts                  # Lottery types
├── ticket.ts                   # Ticket types
├── claim.ts                    # Claim types
├── evidence.ts                 # Evidence types
├── user.ts                     # User/wallet types
├── blockchain.ts               # Blockchain types (transactions, events)
├── api.ts                      # API response types
├── forms.ts                    # Form data types
└── common.ts                   # Shared/common types
```

**Rules:**

- Use `interface` for object shapes
- Use `type` for unions, primitives, utility types
- Prefix with feature name if needed: `LotteryStatus`, `TicketData`
- Keep types close to their domain

**Example:**

```typescript
// src/types/lottery.ts
export interface Lottery {
  id: string;
  creator: `0x${string}`;
  prizePool: bigint;
  endTime: number;
  status: LotteryStatus;
}

export type LotteryStatus = "active" | "ended" | "claimed";

export interface LotteryFormData {
  ticketPrice: string;
  duration: number;
  maxTickets: number;
}

// src/types/index.ts
export * from "./lottery";
export * from "./ticket";
export * from "./claim";
```

---

### **3. Utilities Organization**

```
src/lib/
├── index.ts                    # Re-export all utils
├── blockchain/
│   ├── index.ts
│   ├── formatters.ts           # Format addresses, units, etc.
│   ├── validation.ts           # Validate addresses, chains
│   └── parsers.ts              # Parse blockchain data
├── lottery/
│   ├── index.ts
│   ├── calculations.ts         # Prize calculations, odds
│   ├── validation.ts           # Lottery-specific validation
│   └── formatters.ts           # Format lottery data
├── time/
│   ├── index.ts
│   ├── formatters.ts           # Format timestamps, durations
│   └── calculations.ts         # Time calculations
├── numbers/
│   ├── index.ts
│   ├── formatters.ts           # Format currency, percentages
│   └── validation.ts           # Number validation
└── error-handling.ts           # Error utilities
```

**Rules:**

- Pure functions only (no side effects)
- ONE responsibility per file
- Group by domain/functionality
- Export named functions

**Example:**

```typescript
// src/lib/blockchain/formatters.ts
export const formatAddress = (address: string) => `${address.slice(0, 6)}...${address.slice(-4)}`;

export const formatTokenAmount = (amount: bigint, decimals = 18) => Number(formatUnits(amount, decimals)).toFixed(2);

// src/lib/lottery/calculations.ts
export const calculateOdds = (totalTickets: number, userTickets: number) => (totalTickets > 0 ? (userTickets / totalTickets) * 100 : 0);

export const calculatePrizeShare = (prizePool: bigint, winners: number) => prizePool / BigInt(winners);

// src/lib/index.ts
export * from "./blockchain";
export * from "./lottery";
export * from "./time";
```

---

### **4. Hooks Organization**

```
src/hooks/
├── index.ts                    # Re-export all hooks
├── blockchain/
│   ├── index.ts
│   ├── useBalance.ts           # Token balance hooks
│   ├── useTransaction.ts       # Transaction hooks
│   └── useBlockchain.ts        # Chain, block hooks
├── lottery/
│   ├── index.ts
│   ├── useLottery.ts           # Read lottery data
│   ├── useLotteryMutations.ts  # Write lottery operations
│   └── useLotteryQueries.ts    # Subgraph queries
├── ticket/
│   ├── index.ts
│   ├── useTicket.ts            # Read ticket data
│   ├── useTicketMutations.ts   # Buy tickets, etc.
│   └── useTicketQueries.ts     # Subgraph queries
├── claim/
│   ├── index.ts
│   ├── useClaim.ts             # Claim operations
│   └── useClaimQueries.ts      # Claim queries
├── ui/
│   ├── index.ts
│   ├── useCountdown.ts         # Countdown timer
│   ├── useModal.ts             # Modal state
│   └── useToast.ts             # Toast notifications
└── wallet/
    ├── index.ts
    ├── useWallet.ts            # Wallet connection
    └── useWalletBalance.ts     # Wallet balances
```

**Rules:**

- Separate read (queries) from write (mutations)
- ONE hook per file
- Prefix with domain: `useLotteryData`, `useTicketPurchase`
- Group by feature/domain

**Example:**

```typescript
// src/hooks/lottery/useLottery.ts
export const useLottery = (lotteryId: string) => {
  return useReadContract({
    address: LOTTERY_ADDRESS,
    abi: lotteryAbi,
    functionName: "getLottery",
    args: [lotteryId],
  });
};

// src/hooks/lottery/useLotteryMutations.ts
export const useCreateLottery = () => {
  const { writeContractAsync } = useWriteContract();

  return useMutation({
    mutationFn: async (params: CreateLotteryParams) => {
      return writeContractAsync({
        address: LOTTERY_ADDRESS,
        abi: lotteryAbi,
        functionName: "createLottery",
        args: [params.ticketPrice, params.duration],
      });
    },
  });
};

// src/hooks/index.ts
export * from "./blockchain";
export * from "./lottery";
export * from "./ticket";
export * from "./wallet";
```

---

## Refactoring Prompt Template

---

**FOLDER ORGANIZATION REFACTORING**

**Goal:** Separate constants, types, utilities, and hooks into dedicated, organized folders

**Current File/Code:**

```typescript
[PASTE YOUR CURRENT FILE HERE]
```

**Context:**

- File location: `[CURRENT FILE PATH]`
- What it contains: `[constants/types/utils/hooks - list what's mixed]`

**Refactoring Tasks:**

### **Step 1: Identify & Extract**

Analyze the current file and identify:

- [ ] Constants that should move to `src/constants/`
- [ ] Types/Interfaces that should move to `src/types/`
- [ ] Utility functions that should move to `src/lib/`
- [ ] Hooks that should move to `src/hooks/`

### **Step 2: Organize by Domain**

Group extracted items by domain:

- Lottery-related → `lottery.ts`
- Ticket-related → `ticket.ts`
- Claim-related → `claim.ts`
- Blockchain-related → `blockchain.ts`
- UI-related → `ui.ts`

### **Step 3: Create New Files**

For each domain, create appropriate files:

```typescript
// Example for lottery domain

// src/constants/lottery.ts
export const LOTTERY_ADDRESS = '0x...';
export const MIN_DURATION = 3600;

// src/types/lottery.ts
export interface Lottery {
  id: string;
  creator: `0x${string}`;
}

// src/lib/lottery/calculations.ts
export const calculateOdds = (total: number, user: number) => ...;

// src/hooks/lottery/useLottery.ts
export const useLottery = (id: string) => ...;
```

### **Step 4: Update Imports**

Update all import statements in affected files:

```typescript
// Before
import { LOTTERY_ADDRESS, Lottery, calculateOdds, useLottery } from "./mixed-file";

// After
import { LOTTERY_ADDRESS } from "@/constants/lottery";
import type { Lottery } from "@/types/lottery";
import { calculateOdds } from "@/lib/lottery/calculations";
import { useLottery } from "@/hooks/lottery/useLottery";
```

### **Step 5: Create Index Files**

Create index.ts files for each folder:

```typescript
// src/constants/index.ts
export * from "./lottery";
export * from "./ticket";
export * from "./claim";

// src/types/index.ts
export * from "./lottery";
export * from "./ticket";

// src/lib/index.ts
export * from "./blockchain";
export * from "./lottery";

// src/hooks/index.ts
export * from "./lottery";
export * from "./ticket";
```

**Requirements:**

1. **Separation Rules:**
   - Constants → `src/constants/[domain].ts`
   - Types/Interfaces → `src/types/[domain].ts`
   - Utils → `src/lib/[domain]/[function].ts`
   - Hooks → `src/hooks/[domain]/[hookName].ts`

2. **Naming Conventions:**
   - Constants: `SCREAMING_SNAKE_CASE`
   - Types/Interfaces: `PascalCase`
   - Utils: `camelCase`
   - Hooks: `useCamelCase`

3. **File Structure:**
   - ONE domain per file for constants/types
   - ONE function per file for complex utils
   - ONE hook per file always
   - Index files re-export everything

4. **Import Organization:**
   - Use path aliases: `@/constants`, `@/types`, `@/lib`, `@/hooks`
   - Group imports by source
   - Type imports use `import type { ... }`

**Deliverables:**

1. List of new files to create with their paths
2. Content for each new file
3. Updated import statements for all affected files
4. Index.ts files for each folder
5. Migration checklist to ensure nothing breaks

**Preserve:**

- All functionality remains identical
- No behavior changes
- All exports are accessible
- TypeScript types remain correct

---

## Migration Checklist

After refactoring, verify:

- [ ] All constants moved to appropriate `src/constants/[domain].ts` files
- [ ] All types/interfaces moved to appropriate `src/types/[domain].ts` files
- [ ] All utilities moved to appropriate `src/lib/[domain]/` folders
- [ ] All hooks moved to appropriate `src/hooks/[domain]/` folders
- [ ] Index files created for each folder
- [ ] All imports updated across the codebase
- [ ] No circular dependencies introduced
- [ ] TypeScript compiles without errors
- [ ] All tests still pass
- [ ] Application runs without errors

---

## Example: Complete Migration

### Before: Mixed File

```typescript
// src/features/lottery/utils.ts (MIXED - BAD)
export const LOTTERY_ADDRESS = '0x...';
export const MIN_DURATION = 3600;

export interface Lottery {
  id: string;
  creator: string;
}

export const calculateOdds = (total: number, user: number) => {
  return (user / total) * 100;
};

export const useLottery = (id: string) => {
  return useReadContract({...});
};
```

### After: Organized Files

```typescript
// src/constants/lottery.ts
export const LOTTERY_ADDRESS = "0x..." as const;
export const MIN_DURATION = 3600;

// src/types/lottery.ts
export interface Lottery {
  id: string;
  creator: `0x${string}`;
}

// src/lib/lottery/calculations.ts
export const calculateOdds = (total: number, user: number) => (user / total) * 100;

// src/hooks/lottery/useLottery.ts
import { LOTTERY_ADDRESS } from "@/constants/lottery";
import type { Lottery } from "@/types/lottery";

export const useLottery = (id: string) => {
  return useReadContract({
    address: LOTTERY_ADDRESS,
    // ...
  });
};

// src/constants/index.ts
export * from "./lottery";

// src/types/index.ts
export * from "./lottery";

// src/lib/index.ts
export * from "./lottery";

// src/hooks/index.ts
export * from "./lottery";
```

---

## Key Benefits

✅ **Clear Separation** - Easy to find constants, types, utils, hooks
✅ **Better Organization** - Logical grouping by domain
✅ **Easier Maintenance** - Know exactly where to add new code
✅ **No Duplication** - Centralized definitions
✅ **Better Imports** - Clean, organized import statements
✅ **Type Safety** - Better TypeScript support with organized types
