# TypeScript Refactoring Guide

## Type Safety for Web3 Stack (Wagmi + Viem + React)

### Primary Goals

🎯 **ELIMINATE `any` AND `unknown`**

- Remove all `any` and `unknown` types
- Use proper interfaces and types
- Leverage native package types from Wagmi, Viem, React, etc.

🎯 **PROPER TYPE DEFINITIONS**

- Use `interface` for object shapes
- Use `type` for conversions, unions, utilities
- Use `as const` for type narrowing
- Functions with 3+ args use object parameters

---

## Core Principles

### **Principle 1: Interface vs Type**

```typescript
// ✅ USE interface for object shapes
interface LotteryData {
  id: string;
  creator: `0x${string}`;
  prizePool: bigint;
  endTime: number;
}

// ✅ USE type for conversions, unions, primitives
type LotteryStatus = "active" | "ended" | "claimed";
type HexAddress = `0x${string}`;
type TimestampSeconds = number;

// ✅ USE type for utility types
type Optional<T> = T | null;
type ReadonlyLottery = Readonly<LotteryData>;
```

---

### **Principle 2: Use Native Package Types**

```typescript
// ❌ DON'T recreate types that exist in packages
interface CustomAddress {
  value: string;
  isValid: boolean;
}

// ✅ DO use native types from packages
import { Address } from "viem";
import { UseQueryResult } from "@tanstack/react-query";
import { UseReadContractReturnType } from "wagmi";

// Wagmi/Viem native types
type WalletAddress = Address; // `0x${string}`
type TransactionHash = `0x${string}`;
type ChainId = number;

// TanStack Query native types
type QueryResult<T> = UseQueryResult<T, Error>;

// React native types
type ReactNode = React.ReactNode;
type FormEvent = React.FormEvent<HTMLFormElement>;
type ChangeEvent = React.ChangeEvent<HTMLInputElement>;
```

**Common Package Types to Use:**

```typescript
// From 'viem' (v2.x - verified from docs)
import type {
  Address, // `0x${string}` - Ethereum address type
  Hash, // `0x${string}` - Transaction/block hash
  Hex, // `0x${string}` - Hex data
  PublicClient, // Read-only client
  WalletClient, // Write client with account
  TransactionReceipt, // Transaction receipt type
  Log, // Event log type
  Block, // Block type
  Chain, // Chain configuration type
} from "viem";

// From 'wagmi' (v2.x - verified from docs)
import type {
  UseReadContractReturnType, // Return type of useReadContract
  UseWriteContractReturnType, // Return type of useWriteContract
  UseAccountReturnType, // Return type of useAccount
  UseBalanceReturnType, // Return type of useBalance
  UseSwitchChainReturnType, // Return type of useSwitchChain
  UseWaitForTransactionReceiptReturnType, // Return type for tx wait
  Config, // Wagmi configuration type
} from "wagmi";

// From '@tanstack/react-query' (v5.x - verified from docs)
import type {
  UseQueryResult, // Return type of useQuery
  UseMutationResult, // Return type of useMutation
  UseInfiniteQueryResult, // Return type of useInfiniteQuery
  QueryKey, // Type for query keys
  QueryClient, // Query client instance type
} from "@tanstack/react-query";

// From 'react' (verified from official docs)
import type {
  FC, // Function component type
  ReactNode, // Any valid React child
  PropsWithChildren, // Props with children helper
  MouseEvent, // Mouse event (use with element: MouseEvent<HTMLButtonElement>)
  ChangeEvent, // Change event (use with element: ChangeEvent<HTMLInputElement>)
  FormEvent, // Form event (use with element: FormEvent<HTMLFormElement>)
  KeyboardEvent, // Keyboard event
  CSSProperties, // CSS properties for inline styles
} from "react";
```

---

### **Principle 3: Object Parameters for 3+ Arguments**

```typescript
// ❌ DON'T use multiple parameters (3+)
function createLottery(ticketPrice: bigint, duration: number, maxTickets: number, creator: Address, metadata: string) {
  // ...
}

// ✅ DO use object parameter with interface
interface CreateLotteryParams {
  ticketPrice: bigint;
  duration: number;
  maxTickets: number;
  creator: Address;
  metadata: string;
}

function createLottery(params: CreateLotteryParams) {
  const { ticketPrice, duration, maxTickets, creator, metadata } = params;
  // ...
}

// ✅ Usage is clearer
createLottery({
  ticketPrice: parseUnits("10", 6),
  duration: 86400,
  maxTickets: 100,
  creator: "0x...",
  metadata: "My Lottery",
});
```

---

### **Principle 4: Use `as const` for Type Narrowing**

```typescript
// ❌ DON'T use loose types
const LOTTERY_STATUS = {
  ACTIVE: "active",
  ENDED: "ended",
  CLAIMED: "claimed",
};
type Status = string; // Too loose

// ✅ DO use 'as const' for exact types
const LOTTERY_STATUS = {
  ACTIVE: "active",
  ENDED: "ended",
  CLAIMED: "claimed",
} as const;

type LotteryStatus = (typeof LOTTERY_STATUS)[keyof typeof LOTTERY_STATUS];
// Type: "active" | "ended" | "claimed"

// ✅ Use 'as const' for readonly arrays
const SUPPORTED_CHAINS = [1, 137, 8453] as const;
type SupportedChain = (typeof SUPPORTED_CHAINS)[number];
// Type: 1 | 137 | 8453

// ✅ Use 'as const' for config objects
const LOTTERY_CONFIG = {
  MIN_DURATION: 3600,
  MAX_DURATION: 604800,
  MIN_TICKET_PRICE: 1,
  MAX_TICKETS: 1000,
} as const;
```

---

### **Principle 5: Eliminate `any` and `unknown`**

```typescript
// ❌ DON'T use any
function handleData(data: any) {
  return data.value;
}

const result: any = someFunction();

// ✅ DO define proper types
interface DataResponse {
  value: string;
  timestamp: number;
}

function handleData(data: DataResponse) {
  return data.value;
}

const result: DataResponse = someFunction();

// ❌ DON'T use unknown without narrowing
function processError(error: unknown) {
  console.log(error.message); // Error!
}

// ✅ DO narrow unknown types
function processError(error: unknown) {
  if (error instanceof Error) {
    console.log(error.message);
  } else {
    console.log("Unknown error");
  }
}

// ✅ Better: Create error type
interface AppError {
  message: string;
  code?: string;
  details?: Record<string, unknown>;
}

function processError(error: AppError) {
  console.log(error.message);
}
```

---

## Refactoring Patterns

### **Pattern 1: Component Props**

```typescript
// ❌ BEFORE: Loose types with any
const LotteryCard = ({ lottery, onSelect }: any) => {
  return <div onClick={() => onSelect(lottery.id)}>{lottery.name}</div>;
};

// ✅ AFTER: Proper interface
interface LotteryCardProps {
  lottery: {
    id: string;
    name: string;
    prizePool: bigint;
    endTime: number;
  };
  onSelect: (id: string) => void;
}

const LotteryCard: FC<LotteryCardProps> = ({ lottery, onSelect }) => {
  return <div onClick={() => onSelect(lottery.id)}>{lottery.name}</div>;
};

// ✅ BETTER: Use defined Lottery type
import type { Lottery } from '@/types/lottery';

interface LotteryCardProps {
  lottery: Lottery;
  onSelect: (id: string) => void;
}
```

---

### **Pattern 2: Hook Return Types**

```typescript
// ❌ BEFORE: No return type, uses any
const useLottery = (id: string) => {
  const { data }: any = useReadContract({...});
  return { lottery: data, loading: false };
};

// ✅ AFTER: Explicit return type with native types
import type { UseReadContractReturnType } from 'wagmi';

interface UseLotteryReturn {
  lottery: Lottery | undefined;
  isLoading: boolean;
  error: Error | null;
}

const useLottery = (id: string): UseLotteryReturn => {
  const { data, isLoading, error } = useReadContract({
    address: LOTTERY_ADDRESS,
    abi: lotteryAbi,
    functionName: 'getLottery',
    args: [id],
  });

  return {
    lottery: data as Lottery | undefined,
    isLoading,
    error,
  };
};
```

---

### **Pattern 3: Function Parameters (3+ Args)**

```typescript
// ❌ BEFORE: Many parameters
function buyTickets(lotteryId: string, quantity: number, numbers: number[], userAddress: Address, gasLimit: bigint) {
  // ...
}

// ✅ AFTER: Object parameter
interface BuyTicketsParams {
  lotteryId: string;
  quantity: number;
  numbers: number[];
  userAddress: Address;
  gasLimit: bigint;
}

function buyTickets(params: BuyTicketsParams) {
  const { lotteryId, quantity, numbers, userAddress, gasLimit } = params;
  // ...
}
```

---

### **Pattern 4: Event Handlers**

```typescript
// ❌ BEFORE: any for events
const handleSubmit = (e: any) => {
  e.preventDefault();
  // ...
};

const handleChange = (e: any) => {
  setValue(e.target.value);
};

const handleClick = (e: any) => {
  console.log("clicked");
};

// ✅ AFTER: Native React types (verified from React docs)
import type { FormEvent, ChangeEvent, MouseEvent } from "react";

// Form submission
const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  // ...
};

// Input change
const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};

// Textarea change
const handleTextareaChange = (e: ChangeEvent<HTMLTextAreaElement>) => {
  setText(e.target.value);
};

// Select change
const handleSelectChange = (e: ChangeEvent<HTMLSelectElement>) => {
  setOption(e.target.value);
};

// Button click
const handleClick = (e: MouseEvent<HTMLButtonElement>) => {
  console.log("clicked");
};

// Div click
const handleDivClick = (e: MouseEvent<HTMLDivElement>) => {
  console.log("div clicked");
};
```

---

### **Pattern 5: Contract Interactions**

```typescript
// ❌ BEFORE: Loose types
const transferTokens = async (to: string, amount: any) => {
  const tx = await contract.transfer(to, amount);
  return tx;
};

// ✅ AFTER: Proper types with native Viem types
import type { Address, Hash } from "viem";

interface TransferParams {
  to: Address;
  amount: bigint;
}

const transferTokens = async (params: TransferParams): Promise<Hash> => {
  const { to, amount } = params;
  const hash = await writeContractAsync({
    address: TOKEN_ADDRESS,
    abi: erc20Abi,
    functionName: "transfer",
    args: [to, amount],
  });
  return hash;
};
```

---

### **Pattern 6: GraphQL/Subgraph Queries**

```typescript
// ❌ BEFORE: any for query results
const fetchLotteries = async (): Promise<any> => {
  const response = await fetch(SUBGRAPH_URL, {...});
  const data = await response.json();
  return data.lotteries;
};

// ✅ AFTER: Defined interface
interface SubgraphLottery {
  id: string;
  creator: Address;
  ticketPrice: string; // BigInt as string in GraphQL
  endTime: string;     // Timestamp as string
  totalTickets: string;
}

interface LotteriesResponse {
  lotteries: SubgraphLottery[];
}

const fetchLotteries = async (): Promise<SubgraphLottery[]> => {
  const response = await fetch(SUBGRAPH_URL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ query: LOTTERIES_QUERY }),
  });
  const data: LotteriesResponse = await response.json();
  return data.lotteries;
};
```

---

### **Pattern 7: Constants with Type Narrowing**

```typescript
// ❌ BEFORE: Loose constants
const CONTRACT_ADDRESSES = {
  LOTTERY: "0x...",
  USDC: "0x...",
};

// ✅ AFTER: Type-safe constants
const CONTRACT_ADDRESSES = {
  LOTTERY: "0x...",
  USDC: "0x...",
} as const satisfies Record<string, Address>;

type ContractName = keyof typeof CONTRACT_ADDRESSES;
// Type: "LOTTERY" | "USDC"

// ✅ Use satisfies for validation
const CHAIN_CONFIG = {
  mainnet: { id: 1, name: "Ethereum" },
  polygon: { id: 137, name: "Polygon" },
} as const satisfies Record<string, { id: number; name: string }>;
```

---

## Refactoring Prompt Template

---

**TYPESCRIPT TYPE SAFETY REFACTORING**

**Goal:** Eliminate `any`/`unknown` and use proper TypeScript types with native package types

**Current Code:**

```typescript
[PASTE YOUR CODE HERE]
```

**Refactoring Checklist:**

### **Step 1: Find All `any` and `unknown`**

- [ ] Search for all instances of `any` in the code
- [ ] Search for all instances of `unknown` in the code
- [ ] List each occurrence with line number

### **Step 2: Identify Native Package Types**

For each `any`/`unknown`, check if a native type exists:

- [ ] **Viem types**: `Address`, `Hash`, `Hex`, `TransactionReceipt`, `Log`, `Block`, `Chain`, `PublicClient`, `WalletClient`
- [ ] **Wagmi types**: `UseReadContractReturnType`, `UseWriteContractReturnType`, `UseAccountReturnType`, `UseBalanceReturnType`, `UseSwitchChainReturnType`, `UseWaitForTransactionReceiptReturnType`, `Config`
- [ ] **TanStack Query**: `UseQueryResult`, `UseMutationResult`, `UseInfiniteQueryResult`, `QueryKey`, `QueryClient`
- [ ] **React types**: `FC`, `ReactNode`, `PropsWithChildren`, `FormEvent<T>`, `ChangeEvent<T>`, `MouseEvent<T>`, `KeyboardEvent<T>`, `CSSProperties`
- [ ] **TanStack Router**: Check official docs for route and navigation types
- [ ] **Privy**: Check Privy documentation for authentication and wallet types

### **Step 3: Define Custom Interfaces**

For domain-specific data, create interfaces:

- [ ] Create interface for each object shape (Lottery, Ticket, Claim, etc.)
- [ ] Use `interface` for object definitions
- [ ] Use `type` for unions, conversions, utilities
- [ ] Place in appropriate file under `src/types/[domain].ts`

### **Step 4: Convert Functions with 3+ Parameters**

- [ ] Find functions with 3 or more parameters
- [ ] Create interface for parameter object
- [ ] Name interface: `[FunctionName]Params`
- [ ] Place interface in same file or in `src/types/`

### **Step 5: Add `as const` Where Appropriate**

- [ ] Find constant objects and arrays
- [ ] Add `as const` for type narrowing
- [ ] Use `satisfies` for validation when needed
- [ ] Create union types from constants

### **Step 6: Update Component Props**

- [ ] Define interfaces for all component props
- [ ] Use native React types for events and elements
- [ ] Add `FC<PropType>` for function components

### **Step 7: Type Hook Return Values**

- [ ] Define return type interfaces for custom hooks
- [ ] Use native Wagmi/TanStack Query types where possible
- [ ] Be explicit about return types

**Deliverables:**

1. **List of replacements:**

   ```typescript
   Line 15: `any` → `Address` (from viem)
   Line 23: `unknown` → `TransactionReceipt` (from viem)
   Line 45: Function params → `CreateLotteryParams` interface
   ```

2. **New type definitions:**

   ```typescript
   // src/types/lottery.ts
   interface Lottery { ... }
   type LotteryStatus = ...;
   ```

3. **Updated function signatures:**

   ```typescript
   // Before
   function create(a: any, b: any, c: any) {}

   // After
   interface CreateParams {
     a: string;
     b: number;
     c: Address;
   }
   function create(params: CreateParams) {}
   ```

4. **Import statements:**
   ```typescript
   import type { Address, Hash } from "viem";
   import type { FC, FormEvent } from "react";
   import type { UseQueryResult } from "@tanstack/react-query";
   ```

**Rules:**

1. **Never use `any`** - Always find or create proper type
2. **Prefer native types** - Check packages first before creating custom
3. **Interface for objects** - Use `interface` for object shapes
4. **Type for everything else** - Use `type` for unions, conversions
5. **Object params for 3+ args** - Create interface for parameter object
6. **Use `as const`** - For constants that need type narrowing
7. **Keep types simple** - Avoid complex extends, generics unless necessary

**Don't Create Complex Types:**

- Avoid deep generics
- Avoid complex conditional types
- Avoid complex mapped types
- Keep types readable and understandable

**Expected Result:**

- Zero `any` in codebase
- Minimal `unknown` (only for error handling with proper narrowing)
- Proper use of native package types
- Clean, readable type definitions
- Full TypeScript type safety

---

## Common Web3 Type Definitions

```typescript
// src/types/blockchain.ts
import type { Address, Hash, TransactionReceipt } from "viem";

export type HexAddress = Address;
export type TransactionHash = Hash;
export type ChainId = number;
export type Wei = bigint;
export type Gwei = bigint;

export interface TransactionResult {
  hash: TransactionHash;
  receipt: TransactionReceipt | null;
  isConfirmed: boolean;
}

// src/types/lottery.ts
import type { Address } from "viem";

export interface Lottery {
  id: string;
  creator: Address;
  ticketPrice: bigint;
  prizePool: bigint;
  endTime: number;
  maxTickets: number;
  totalTickets: number;
  status: LotteryStatus;
}

export type LotteryStatus = "active" | "ended" | "claimed";

export interface CreateLotteryParams {
  ticketPrice: bigint;
  duration: number;
  maxTickets: number;
  metadata: string;
}

// src/types/ticket.ts
export interface Ticket {
  id: string;
  lotteryId: string;
  owner: Address;
  numbers: number[];
  purchaseTime: number;
}

export interface BuyTicketsParams {
  lotteryId: string;
  quantity: number;
  numbers: number[][];
}

// src/types/wallet.ts
import type { Address } from "viem";

export interface WalletState {
  address: Address | undefined;
  isConnected: boolean;
  isConnecting: boolean;
  chainId: number | undefined;
}
```

---

## Package-Specific Type Imports

```typescript
// Viem (v2.x - verified from official docs)
import type {
  Address, // `0x${string}` - Ethereum addresses
  Hash, // `0x${string}` - Transaction/block hashes
  Hex, // `0x${string}` - Hex encoded data
  TransactionReceipt, // Transaction receipt after confirmation
  Log, // Event logs from contracts
  Block, // Block information
  Chain, // Chain configuration
  PublicClient, // Read-only blockchain client
  WalletClient, // Write client with wallet/account
} from "viem";

// Wagmi (v2.x - verified from official docs)
import type {
  UseReadContractReturnType, // useReadContract hook return
  UseWriteContractReturnType, // useWriteContract hook return
  UseAccountReturnType, // useAccount hook return
  UseBalanceReturnType, // useBalance hook return
  UseSwitchChainReturnType, // useSwitchChain hook return
  UseWaitForTransactionReceiptReturnType, // useWaitForTransactionReceipt return
  Config, // Wagmi configuration type
} from "wagmi";

// TanStack Query (v5.x - verified from official docs)
import type {
  UseQueryResult, // useQuery return type
  UseMutationResult, // useMutation return type
  UseInfiniteQueryResult, // useInfiniteQuery return type
  QueryKey, // Query key type (string | readonly unknown[])
  QueryClient, // QueryClient instance type
} from "@tanstack/react-query";

// React (verified from official React docs)
import type {
  FC, // Function component: FC<Props>
  ReactNode, // Any renderable React content
  PropsWithChildren, // Helper for props with children
  FormEvent, // Form events: FormEvent<HTMLFormElement>
  ChangeEvent, // Change events: ChangeEvent<HTMLInputElement>
  MouseEvent, // Mouse events: MouseEvent<HTMLButtonElement>
  KeyboardEvent, // Keyboard events: KeyboardEvent<HTMLInputElement>
  CSSProperties, // Inline style object type
  HTMLAttributes, // Standard HTML attributes
} from "react";

// TanStack Router
import type {
  LinkProps, // Link component props
  RouteComponent, // Route component type
  Router, // Router instance type
} from "@tanstack/react-router";
```
