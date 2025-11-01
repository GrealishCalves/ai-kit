# Web3 Hook Standardization Guide

_For Wagmi + TanStack Query + Viem Stack_

This guide defines **standardized patterns** for Web3 hooks to ensure consistency across the codebase. Every hook should follow these patterns so developers can predict implementation based on hook type.

**Note:** For complexity reduction and refactoring guidance, see `complexity.md`. This guide focuses solely on **standardization and consistency**.

## 📚 Hook Type Standards

### **1. Read Hooks (Contract Reads)**

**Standard Pattern:** Custom hook wrapper returning native `useReadContract` object

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
```

**Usage in Components:**

```typescript
const { data: balance, isLoading, error } = useUSDCBalance();
```

**Checklist:**

- ✅ Returns native `useReadContract` object
- ✅ Adds meaningful configuration (enabled, staleTime)
- ✅ Consistent naming: `use[Feature][Operation]`
- ✅ No data transformation in hook

---

### **2. Mutation Hooks (Contract Writes)**

**Standard Pattern:** Pure mutation returning native `useMutation` object

```typescript
export function useCreateLotteryMutation(): UseMutationResult<Hash, Error, CreateLotteryParams> {
  const { writeContractAsync } = useWriteContract();

  return useMutation({
    mutationFn: async (params: CreateLotteryParams) => {
      const hash = await writeContractAsync({
        address: CONTRACTS.LOTTERY.address,
        abi: LOTTERY_ABI,
        functionName: "createLottery",
        args: [params.prizeAmount, params.ticketPrice],
      });
      return hash;
    },
  });
}
```

**Usage in Components:**

```typescript
const { mutateAsync, isPending, error } = useCreateLotteryMutation();
```

**Checklist:**

- ✅ Returns native `useMutation` object
- ✅ Pure function (no side effects, no toasts)
- ✅ Consistent naming: `use[Feature]Mutation`
- ✅ No orchestration logic

---

### **3. Query Hooks (Subgraph/API Queries)**

**Standard Pattern:** Custom hook returning native `useQuery` object

```typescript
export function useActiveLotteries(params: UseActiveLotteriesParams = {}) {
  const { first = 10, skip = 0 } = params;
  const queryClient = useQueryClient();

  useWatchContractEvent({
    address: CONTRACTS.LOTTERY.address,
    abi: LOTTERY_ABI,
    eventName: "LotteryCreated",
    onLogs: () => {
      queryClient.invalidateQueries({ queryKey: QUERY_KEYS.lotteriesActive });
    },
  });

  return useQuery(activeLotteriesOptions(first, skip));
}
```

**Usage in Components:**

```typescript
const { data: lotteries, isLoading, error } = useActiveLotteries();
```

**Checklist:**

- ✅ Returns native `useQuery` object
- ✅ May include event listeners for real-time updates
- ✅ Consistent naming: `use[Feature][Operation]`
- ✅ No data transformation in hook

---

### **4. Orchestration Hooks (Coordination)**

**Standard Pattern:** Spread native mutation + add orchestration state

```typescript
export function useBuyTickets() {
  const [isWaitingForResult, setIsWaitingForResult] = useState(false);
  const [lotteryResult, setLotteryResult] = useState<LotteryResult | null>(null);

  const { address } = useAccount();
  const queryClient = useQueryClient();
  const { listenForLotteryResult } = useLotteryResultListener();
  const mutation = useBuyTicketsMutation();

  const buyTicketsAction = async (params: BuyTicketsParams) => {
    const { hash, receipt } = await mutation.mutateAsync({
      lotteryId: params.lotteryId,
      numbersPacked: params.numbersPacked,
      totalCost: params.totalCost,
    });

    listenForLotteryResult({
      lotteryId: params.lottery.id,
      playerAddress: address,
      selectedNumbers: params.selectedNumbers,
      onResult: (result) => {
        setLotteryResult(result);
        queryClient.invalidateQueries({ queryKey: QUERY_KEYS.lotteriesActive });
      },
      onWaitingChange: setIsWaitingForResult,
    });

    return { hash, receipt };
  };

  return {
    ...mutation,
    mutateAsync: buyTicketsAction,
    isWaitingForResult,
    lotteryResult,
    clearResult: () => setLotteryResult(null),
  };
}
```

**Usage in Components:**

```typescript
const { mutateAsync: buyTickets, isPending: isLoading, error, isWaitingForResult, lotteryResult, clearResult } = useBuyTickets();
```

**Checklist:**

- ✅ Spreads native mutation object (`...mutation`)
- ✅ Overrides `mutateAsync` with orchestration logic
- ✅ Adds orchestration-specific state
- ✅ Returns native `error` object (not string)
- ✅ Components handle toasts
- ✅ Consistent naming: `use[Feature]`

**Key Benefits:**

- Components get all native mutation properties (`isPending`, `error`, `isSuccess`, etc.)
- Orchestration state is clearly separated
- Error handling remains native (no transformation)
- Follows native-first principle

---

### **5. Form Hooks (Complex State Management)**

**Standard Pattern:** Custom return object (acceptable for complex forms)

```typescript
export function useCreateLotteryForm() {
  const [prizeAmount, setPrizeAmount] = useState(0);
  const [ticketPrice, setTicketPrice] = useState(0);

  const approveMutation = useApproveUSDCMutation();
  const createMutation = useCreateLotteryMutation();

  const handleApprove = async () => {};

  const handleSubmit = async () => {};

  return {
    prizeAmount,
    setPrizeAmount,
    ticketPrice,
    setTicketPrice,
    isApproving: approveMutation.isPending,
    isCreating: createMutation.isPending,
    handleApprove,
    handleSubmit,
  };
}
```

**Usage in Components:**

```typescript
const { prizeAmount, setPrizeAmount, isApproving, isCreating, handleApprove, handleSubmit } = useCreateLotteryForm();
```

**Checklist:**

- ✅ Custom return object (acceptable for forms)
- ✅ Consistent naming for loading states: `is[Action]ing`
- ✅ Consistent naming for handlers: `handle[Action]`
- ✅ Groups related state and actions

---

## 🔗 **Wagmi Hook Implementation Consistency**

### **Wagmi Configuration Consistency**

- [ ] **Chain Configuration**: Are chains configured consistently?

```javascript
// Consistent chain setup across all config files:
import { mainnet, polygon, arbitrum } from 'wagmi/chains'

const chains = [mainnet, polygon, arbitrum] as const

// CHECK: Same chains imported and used everywhere?
// No mixed chain definitions or inconsistent chain objects?
```

- [ ] **Provider Configuration**: Are providers set up consistently?

```javascript
// Pattern A: Alchemy transport everywhere
const config = createConfig({
  chains: [mainnet, polygon],
  transports: {
    [mainnet.id]: http(`https://eth-mainnet.g.alchemy.com/v2/${ALCHEMY_KEY}`),
    [polygon.id]: http(`https://polygon-mainnet.g.alchemy.com/v2/${ALCHEMY_KEY}`),
  },
});

// Pattern B: Mixed transports
// Some using Alchemy, others using different providers
// CHECK: Consistent transport configuration across all chains?
```

## 🔐 **Privy Authentication Patterns**

### **Authentication Flow Consistency**

- [ ] **Login/Logout Patterns**: Are auth flows handled consistently?

```javascript
// Pattern A: Direct Privy hooks usage
const { login, logout, authenticated, user } = usePrivy();

const handleLogin = () => {
  login();
};

// Pattern B: Custom auth hook wrapper
const useAuth = () => {
  const { login, logout, authenticated, user } = usePrivy();

  return {
    signIn: login,
    signOut: logout,
    isAuthenticated: authenticated,
    currentUser: user,
  };
};

// CHECK: Is authentication handled the SAME way in ALL components?
```

- [ ] **User Data Access**: Is user data accessed consistently?

```javascript
// Consistent pattern for accessing user data:
const { user } = usePrivy();

// Always check authentication first
if (!user) return <LoginPrompt />;

// Then access user properties consistently
const userWallet = user.wallet?.address;
const userEmail = user.email?.address;

// CHECK: Same null-checking and property access everywhere?
```

### **Wallet Connection Patterns**

- [ ] **Wallet State Management**: Are wallet connections handled consistently?

```javascript
// Pattern A: Direct useWallets hook
const { wallets } = useWallets();
const connectedWallet = wallets.find((wallet) => wallet.connectorType === "injected");

// Pattern B: Custom wallet hook
const useConnectedWallet = () => {
  const { wallets } = useWallets();
  return wallets.find((wallet) => wallet.connectorType === "injected");
};

// CHECK: Same wallet selection logic across all components?
```

## 📊 **Goldsky Subgraph Query Consistency**

### **GraphQL Query Patterns**

- [ ] **Query Structure**: Are subgraph queries structured consistently?

```javascript
// Pattern A: Direct GraphQL queries with consistent structure
const GET_USER_TOKENS = gql`
  query GetUserTokens($userAddress: String!) {
    user(id: $userAddress) {
      tokens {
        id
        balance
        token {
          name
          symbol
          decimals
        }
      }
    }
  }
`;

// Pattern B: Custom hook with query
const useUserTokens = (userAddress) => {
  return useQuery(GET_USER_TOKENS, {
    variables: { userAddress: userAddress?.toLowerCase() },
    skip: !userAddress,
  });
};

// CHECK: Same query structure and error handling patterns everywhere?
```

- [ ] **Data Transformation**: Is subgraph data processed consistently?

```javascript
// Consistent data transformation pattern:
const transformTokenData = (rawTokens) => {
  return rawTokens.map((token) => ({
    id: token.id,
    balance: parseFloat(formatUnits(token.balance, token.token.decimals)),
    symbol: token.token.symbol,
    name: token.token.name,
  }));
};

// CHECK: Same transformation patterns for similar data types?
```

### **Subgraph Error Handling**

- [ ] **Query Error Patterns**: Are GraphQL errors handled consistently?

```javascript
// Consistent error handling pattern:
const { data, loading, error } = useQuery(QUERY);

if (loading) return <LoadingSpinner />;
if (error) {
  console.error("Subgraph query failed:", error);
  return <ErrorMessage message="Failed to load data" />;
}

// CHECK: Same error handling across ALL subgraph queries?
```

## ⛓️ **Blockchain Data Formatting Consistency**

### **Token Amount Formatting**

- [ ] **BigNumber Handling**: Are token amounts formatted consistently?

```javascript
// Pattern A: Consistent use of viem utilities
import { formatUnits, parseUnits } from "viem";

const formatTokenAmount = (amount, decimals) => {
  return formatUnits(amount, decimals);
};

const parseTokenAmount = (amount, decimals) => {
  return parseUnits(amount, decimals);
};

// Pattern B: Custom BigNumber utilities
// CHECK: Is ONE approach used consistently everywhere?
```

- [ ] **Address Formatting**: Are addresses handled consistently?

```javascript
// Consistent address formatting:
import { getAddress, isAddress } from "viem";

const formatAddress = (address) => {
  if (!isAddress(address)) return null;
  return getAddress(address); // Checksummed address
};

const truncateAddress = (address) => {
  if (!address) return "";
  return `${address.slice(0, 6)}...${address.slice(-4)}`;
};

// CHECK: Same address validation and formatting everywhere?
```

### **Chain-Specific Data**

- [ ] **Multi-Chain Handling**: Is multi-chain data handled consistently?

```javascript
// Consistent multi-chain pattern:
const useMultiChainBalance = (userAddress) => {
  const mainnetBalance = useBalance({
    address: userAddress,
    chainId: mainnet.id,
  });

  const polygonBalance = useBalance({
    address: userAddress,
    chainId: polygon.id,
  });

  return {
    [mainnet.id]: mainnetBalance,
    [polygon.id]: polygonBalance,
  };
};

// CHECK: Same multi-chain pattern across all features?
```

## 🚫 **Web3-Specific Unnecessary Abstractions**

### **Over-Abstracted Wagmi Wrappers**

- [ ] **Simple Wagmi Hook Wrappers**: Are there unnecessary wagmi hook wrappers?

```javascript
// UNNECESSARY: Simple wrapper that adds no value
const useBalance = (address) => {
  return useBalance({ address });
};

// BETTER: Use wagmi hook directly
const { data: balance } = useBalance({ address: userAddress });

// JUSTIFIED: Wrapper that adds meaningful functionality
const useFormattedBalance = (address, decimals = 18) => {
  const { data: balance } = useBalance({ address });
  return balance ? formatUnits(balance.value, decimals) : "0";
};
```

- [ ] **Redundant Contract Abstractions**: Are contract interactions over-abstracted?

```javascript
// UNNECESSARY: Wrapper that just passes through wagmi
const useContractRead = (address, abi, functionName, args) => {
  return useReadContract({ address, abi, functionName, args });
};

// BETTER: Use useReadContract directly
const { data } = useReadContract({
  address: TOKEN_ADDRESS,
  abi: ERC20_ABI,
  functionName: "balanceOf",
  args: [userAddress],
});

// JUSTIFIED: Contract-specific hook with business logic
const useTokenBalance = (tokenAddress, userAddress) => {
  const { data: balance } = useReadContract({
    address: tokenAddress,
    abi: ERC20_ABI,
    functionName: "balanceOf",
    args: [userAddress],
  });

  const { data: decimals } = useReadContract({
    address: tokenAddress,
    abi: ERC20_ABI,
    functionName: "decimals",
  });

  return {
    raw: balance,
    formatted: balance && decimals ? formatUnits(balance, decimals) : "0",
    decimals,
  };
};
```

### **Over-Engineered Privy Wrappers**

- [ ] **Simple Auth Wrappers**: Are there unnecessary Privy wrappers?

```javascript
// UNNECESSARY: Simple wrapper
const useAuthentication = () => {
  const { authenticated } = usePrivy();
  return authenticated;
};

// BETTER: Use usePrivy directly
const { authenticated } = usePrivy();

// JUSTIFIED: Complex auth logic
const useAuthWithRedirect = () => {
  const { authenticated, login } = usePrivy();
  const router = useRouter();

  useEffect(() => {
    if (!authenticated && router.pathname !== "/login") {
      router.push("/login");
    }
  }, [authenticated, router]);

  return { authenticated, login };
};
```

## 🔍 **Native Web3 Solutions First**

### **Prefer Viem Utilities Over Custom Implementations**

- [ ] **Address Validation**: Use viem instead of custom validation

```javascript
// AVOID: Custom address validation
const isValidAddress = (address) => {
  return /^0x[a-fA-F0-9]{40}$/.test(address);
};

// PREFER: Viem isAddress utility
import { isAddress } from "viem";
const isValid = isAddress(userAddress);
```

- [ ] **Unit Conversion**: Use viem instead of custom BigNumber handling

```javascript
// AVOID: Custom unit conversion
const toWei = (value) => {
  return BigNumber.from(value).mul(BigNumber.from(10).pow(18));
};

// PREFER: Viem parseUnits
import { parseUnits } from "viem";
const weiValue = parseUnits(value, 18);
```

### **Leverage Wagmi Built-ins**

- [ ] **Network Switching**: Use wagmi's built-in network switching

```javascript
// PREFER: Wagmi's useSwitchChain
const { switchChain } = useSwitchChain();

const handleSwitchToPolygon = () => {
  switchChain({ chainId: polygon.id });
};

// AVOID: Custom network switching logic
```

- [ ] **Transaction Status**: Use wagmi's transaction utilities

```javascript
// PREFER: useWaitForTransactionReceipt
const { isLoading, isSuccess, isError } = useWaitForTransactionReceipt({
  hash: transactionHash,
});

// AVOID: Custom transaction polling
```

## 📋 **Web3-Specific Analysis Framework**

### **Questions for Each Pattern:**

1. **Wagmi Consistency**:
   - Are the same wagmi hooks used for similar functionality?
   - Is error handling consistent across all contract interactions?

2. **Privy Integration**:
   - Is authentication checked the same way everywhere?
   - Are wallet connections handled uniformly?

3. **Subgraph Queries**:
   - Do all GraphQL queries follow the same structure?
   - Is data transformation consistent for similar entities?

4. **Web3 Utilities**:
   - Are addresses, amounts, and transactions formatted uniformly?
   - Is multi-chain logic implemented consistently?

### **Web3-Specific Red Flags:**

- **Mixed Contract Patterns**: Same contract called differently across components
- **Inconsistent Error Handling**: Different error patterns for failed transactions
- **Address Format Mixing**: Some checksummed, others lowercase
- **Amount Format Inconsistency**: Mixed BigNumber vs string vs number handling
- **Auth State Confusion**: Different ways of checking authentication status
- **Chain Data Mixing**: Inconsistent multi-chain data handling

### **Expected Web3 Deliverable:**

```markdown
## Web3 Implementation Consistency Report

### Wagmi Pattern Violations: [X]

### Privy Integration Issues: [Y]

### Subgraph Query Inconsistencies: [Z]

### Unnecessary Web3 Abstractions: [W]

### Recommended Web3 Patterns:

1. **Contract Reads**: [Chosen wagmi pattern]
2. **Contract Writes**: [Chosen transaction pattern]
3. **Authentication Flow**: [Chosen Privy pattern]
4. **Subgraph Queries**: [Chosen GraphQL pattern]
5. **Address Formatting**: [Chosen viem pattern]
6. **Amount Formatting**: [Chosen BigNumber pattern]

### Web3 Abstractions to Remove:

1. [Hook/Component] - Replace with [Native wagmi/viem solution]
2. [Utility] - Replace with [Built-in Web3 functionality]

### Files Requiring Web3 Updates:

- [File]: [Specific wagmi/privy/subgraph changes]
- [File]: [Specific Web3 consistency fixes]
```

Focus on ensuring that any Web3 developer can predict how blockchain interactions, authentication, and data fetching will be implemented based on established patterns in the codebase.

## 📝 **TypeScript/Type Definition Consistency**

### **Interface vs Type Consistency**

- [ ] **Type Definition Style**: Are types defined consistently?

```typescript
// Pattern A: Interface for object shapes
interface UserProps {
  name: string;
  email: string;
}

// Pattern B: Type aliases
type UserProps = {
  name: string;
  email: string;
};

// Pattern C: Mixed usage
// Some files use interface, others use type

// CHECK: Is ONE approach used consistently throughout?
```

- [ ] **Props Type Patterns**: Are component props typed consistently?

```typescript
// Pattern A: Inline props type
const Component = ({ name, age }: { name: string; age: number }) => {};

// Pattern B: Separate interface
interface ComponentProps {
  name: string;
  age: number;
}
const Component = ({ name, age }: ComponentProps) => {};

// Pattern C: Type alias
type ComponentProps = {
  name: string;
  age: number;
};

// CHECK: Same pattern for ALL component props?
```

- [ ] **Optional vs Required Props**: Are optional properties handled consistently?

```typescript
// Consistent pattern for optional props:
interface Props {
  required: string;
  optional?: string;
  withDefault?: string;
}

// With consistent default handling:
const Component = ({ required, optional, withDefault = "default value" }: Props) => {};
```

## 🛠 **Utility Function Implementation Consistency**

### **Function Definition Patterns**

- [ ] **Utility Function Style**: Are utility functions defined consistently?

```javascript
// Pattern A: Named function exports
export function formatDate(date) {
  return date.toLocaleDateString();
}

// Pattern B: Arrow function exports
export const formatDate = (date) => {
  return date.toLocaleDateString();
};

// Pattern C: Default exports
const formatDate = (date) => date.toLocaleDateString();
export default formatDate;

// CHECK: Same pattern for ALL utility functions?
```

- [ ] **Error Handling in Utils**: Do utilities handle errors consistently?

```javascript
// Consistent error handling pattern:
export const safeOperation = (input) => {
  try {
    // Validate input consistently
    if (!input) {
      throw new Error("Input is required");
    }

    // Perform operation
    return result;
  } catch (error) {
    // Handle errors consistently
    console.error("SafeOperation failed:", error);
    return null; // or throw, but be consistent
  }
};
```

## ⚠️ **Unnecessary Abstractions & Wrappers**

### **Over-Abstracted Components**

- [ ] **Wrapper Components**: Are there unnecessary wrapper components?

```javascript
// UNNECESSARY: Simple wrapper that adds no value
const CustomButton = ({ children, ...props }) => {
  return <button {...props}>{children}</button>;
};

// BETTER: Use native button directly
<button onClick={handleClick}>Click me</button>;

// JUSTIFIED: Wrapper that adds consistent styling/behavior
const CustomButton = ({ variant, children, ...props }) => {
  const className = `btn btn-${variant}`;
  return (
    <button className={className} {...props}>
      {children}
    </button>
  );
};
```

- [ ] **Redundant Higher-Order Components**: Are HOCs adding unnecessary complexity?

```javascript
// UNNECESSARY: HOC that only passes props
const withProps = (Component) => (props) => <Component {...props} />

// BETTER: Use component directly
<UserProfile {...props} />

// JUSTIFIED: HOC that adds meaningful functionality
const withAuth = (Component) => (props) => {
  const { isAuthenticated } = useAuth()
  return isAuthenticated ? <Component {...props} /> : <LoginForm />
}
```

### **Over-Engineered Custom Hooks**

- [ ] **Simple State Wrappers**: Are there hooks that just wrap useState unnecessarily?

```javascript
// UNNECESSARY: Hook that just wraps useState
const useToggle = (initial = false) => {
  const [state, setState] = useState(initial);
  const toggle = () => setState(!state);
  return [state, toggle];
};

// BETTER: Use useState directly for simple cases
const [isOpen, setIsOpen] = useState(false);
const toggle = () => setIsOpen(!isOpen);

// JUSTIFIED: Hook with complex logic
const useToggle = (initial = false) => {
  const [state, setState] = useState(initial);
  const toggle = useCallback(() => setState((prev) => !prev), []);
  const setTrue = useCallback(() => setState(true), []);
  const setFalse = useCallback(() => setState(false), []);

  return { state, toggle, setTrue, setFalse };
};
```

### **Unnecessary Utility Abstractions**

- [ ] **Simple Native Wrappers**: Are there utilities wrapping native functions unnecessarily?

```javascript
// UNNECESSARY: Wrapper around native methods
const isEmpty = (arr) => arr.length === 0;
const isNotEmpty = (arr) => arr.length > 0;

// BETTER: Use native methods directly
if (items.length === 0) {
}
if (items.length > 0) {
}

// JUSTIFIED: Complex validation utility
const isValidEmail = (email) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email) && email.length <= 254;
};
```

## 🔍 **Native-First Solutions**

### **Prefer Native Browser APIs**

- [ ] **Date Handling**: Are custom date utilities necessary?

```javascript
// AVOID: Custom date utility for simple formatting
const formatDate = (date) => {
  return `${date.getMonth() + 1}/${date.getDate()}/${date.getFullYear()}`;
};

// PREFER: Native Intl.DateTimeFormat
const formatDate = (date) => {
  return new Intl.DateTimeFormat("en-US").format(date);
};

// OR: Native toLocaleDateString
const formatDate = (date) => {
  return date.toLocaleDateString("en-US");
};
```

- [ ] **Array Operations**: Are custom array utilities replacing native methods?

```javascript
// AVOID: Custom implementation
const findUserById = (users, id) => {
  for (let user of users) {
    if (user.id === id) return user;
  }
  return null;
};

// PREFER: Native find method
const findUserById = (users, id) => users.find((user) => user.id === id);

// AVOID: Custom filter
const getActiveUsers = (users) => {
  const active = [];
  for (let user of users) {
    if (user.active) active.push(user);
  }
  return active;
};

// PREFER: Native filter
const getActiveUsers = (users) => users.filter((user) => user.active);
```

### **DOM Manipulation**

- [ ] **Ref Usage**: Are refs used consistently vs direct DOM access?

```javascript
// CONSISTENT: Use refs for all DOM access
const inputRef = useRef(null);

const focusInput = () => {
  inputRef.current?.focus();
};

return <input ref={inputRef} />;

// AVOID: Mixed patterns of DOM access
// Sometimes refs, sometimes querySelector
```

### **Event Handling**

- [ ] **Event Patterns**: Are events handled with native patterns?

```javascript
// PREFER: Native event handling
const handleSubmit = (e) => {
  e.preventDefault();
  // handle form submission
};

// AVOID: Unnecessary event wrapper utilities
const preventDefaultAndSubmit = (callback) => (e) => {
  e.preventDefault();
  callback(e);
};
```

## 📊 **Analysis Framework**

### **Questions to Ask for Each Pattern:**

1. **Consistency Check**:
   - Is this pattern used in ALL similar situations?
   - Are there files doing the same thing differently?

2. **Necessity Check**:
   - Does this abstraction solve a real problem?
   - Could native JavaScript/React handle this better?
   - Is this wrapper adding meaningful value?

3. **Simplicity Check**:
   - Is this the simplest solution that works?
   - Could a developer understand this without documentation?
   - Does this reduce or increase cognitive load?

### **Red Flags to Report:**

- **Mixed Patterns**: Same functionality implemented differently across files
- **Unnecessary Layers**: Wrappers that don't add meaningful functionality
- **Native Alternatives**: Custom code that duplicates native browser/React features
- **Inconsistent Error Handling**: Different error patterns across similar functions
- **Over-Engineering**: Complex solutions for simple problems

### **Expected Deliverable:**

```markdown
## Implementation Consistency Report

### Pattern Violations Found: [X]

### Unnecessary Abstractions: [Y]

### Native Alternative Opportunities: [Z]

### Recommended Patterns to Standardize:

1. **Component Definition**: [Chosen pattern with examples]
2. **Props Handling**: [Chosen pattern with examples]
3. **Hook Structure**: [Chosen pattern with examples]
4. **Error Handling**: [Chosen pattern with examples]

### Abstractions to Remove:

1. [Component/Hook/Utility] - Replace with [Native solution]
2. [Component/Hook/Utility] - Replace with [Native solution]

### Files Requiring Updates:

- [File path]: [Specific changes needed]
- [File path]: [Specific changes needed]
```

Focus on creating a codebase where any developer can predict how something will be implemented based on how they've seen it done elsewhere in the project.

---

## 📋 Quick Reference: Hook Standardization

### Hook Type Decision Tree

```
Is it reading blockchain data?
├─ YES → Use Read Hook Pattern (returns native useReadContract)
└─ NO → Continue...

Is it writing to blockchain?
├─ YES → Use Mutation Hook Pattern (returns native useMutation)
└─ NO → Continue...

Is it querying subgraph/API?
├─ YES → Use Query Hook Pattern (returns native useQuery)
└─ NO → Continue...

Does it coordinate multiple operations?
├─ YES → Use Orchestration Hook Pattern (spread mutation + add state)
└─ NO → Continue...

Is it managing complex form state?
└─ YES → Use Form Hook Pattern (custom return object)
```

### Standardized Return Patterns

| Hook Type              | Return Pattern           | Example                                                 |
| ---------------------- | ------------------------ | ------------------------------------------------------- |
| **Read Hook**          | Native `useReadContract` | `return useReadContract({...})`                         |
| **Mutation Hook**      | Native `useMutation`     | `return useMutation({...})`                             |
| **Query Hook**         | Native `useQuery`        | `return useQuery({...})`                                |
| **Orchestration Hook** | Spread mutation + state  | `return { ...mutation, mutateAsync: action, ...state }` |
| **Form Hook**          | Custom object            | `return { field1, setField1, handleSubmit, ... }`       |

### Naming Conventions

| Hook Type              | Naming Pattern            | Example                                             |
| ---------------------- | ------------------------- | --------------------------------------------------- |
| **Read Hook**          | `use[Feature][Operation]` | `useUSDCBalance`, `useLotteryVrfFee`                |
| **Mutation Hook**      | `use[Feature]Mutation`    | `useCreateLotteryMutation`, `useBuyTicketsMutation` |
| **Query Hook**         | `use[Feature][Operation]` | `useActiveLotteries`, `useLotteryFinancialEvidence` |
| **Orchestration Hook** | `use[Feature]`            | `useBuyTickets`, `useCreateLottery`                 |
| **Form Hook**          | `use[Feature]Form`        | `useCreateLotteryForm`, `useEditProfileForm`        |

### Component Usage Patterns

```typescript
const { data, isLoading, error } = useUSDCBalance();

const { mutateAsync, isPending, error } = useCreateLotteryMutation();

const { data: lotteries, isLoading, error } = useActiveLotteries();

const { mutateAsync: buyTickets, isPending: isLoading, error, isWaitingForResult, lotteryResult } = useBuyTickets();

const { prizeAmount, setPrizeAmount, isApproving, handleApprove, handleSubmit } = useCreateLotteryForm();
```

### Error Handling Standards

| Hook Type              | Error Return          | Component Handling                      |
| ---------------------- | --------------------- | --------------------------------------- |
| **Read Hook**          | Native `Error` object | `if (error) toast.error(error.message)` |
| **Mutation Hook**      | Native `Error` object | `if (error) toast.error(error.message)` |
| **Query Hook**         | Native `Error` object | `if (error) toast.error(error.message)` |
| **Orchestration Hook** | Native `Error` object | `if (error) toast.error(error.message)` |
| **Form Hook**          | Native `Error` object | `if (error) toast.error(error.message)` |

**Key Rule:** Never transform errors to strings in hooks. Always return native `Error` objects.

### Loading State Standards

| Hook Type              | Loading Property | Usage                               |
| ---------------------- | ---------------- | ----------------------------------- |
| **Read Hook**          | `isLoading`      | From native `useReadContract`       |
| **Mutation Hook**      | `isPending`      | From native `useMutation`           |
| **Query Hook**         | `isLoading`      | From native `useQuery`              |
| **Orchestration Hook** | `isPending`      | From spread mutation                |
| **Form Hook**          | `is[Action]ing`  | Custom: `isApproving`, `isCreating` |

---

## ✅ Standardization Checklist

### For Read Hooks

- [ ] Returns native `useReadContract` object
- [ ] Adds meaningful configuration (enabled, staleTime)
- [ ] Naming: `use[Feature][Operation]`
- [ ] No data transformation in hook
- [ ] Components destructure properties

### For Mutation Hooks

- [ ] Returns native `useMutation` object
- [ ] Pure function (no side effects)
- [ ] Naming: `use[Feature]Mutation`
- [ ] No toasts or orchestration
- [ ] Components handle user feedback

### For Query Hooks

- [ ] Returns native `useQuery` object
- [ ] May include event listeners
- [ ] Naming: `use[Feature][Operation]`
- [ ] No data transformation in hook
- [ ] Components destructure properties

### For Orchestration Hooks

- [ ] Spreads native mutation object
- [ ] Overrides `mutateAsync` with orchestration logic
- [ ] Adds orchestration-specific state
- [ ] Returns native `error` object (not string)
- [ ] Naming: `use[Feature]`
- [ ] Components handle toasts

### For Form Hooks

- [ ] Custom return object (acceptable)
- [ ] Consistent naming: `is[Action]ing`, `handle[Action]`
- [ ] Naming: `use[Feature]Form`
- [ ] Groups related state and actions
- [ ] Components handle toasts

---

## 🎯 Key Principles

1. **Native-First**: Always return native Wagmi/TanStack Query objects when possible
2. **Predictability**: Developers should know what a hook returns based on its type
3. **Consistency**: Same hook type = same return pattern everywhere
4. **Error Handling**: Never transform errors to strings
5. **Loading States**: Use native loading properties (`isPending`, `isLoading`)
6. **Separation of Concerns**: Hooks coordinate, components present

---

## 📚 Related Documentation

- **complexity.md** - For complexity reduction and refactoring guidance
- **hooks.md** - For simple Web3 hooks examples (may be outdated)
- **type.md** - For TypeScript patterns and type definitions

---

**Remember:** This guide is about **standardization**, not **simplification**. For reducing complexity, see `complexity.md`.
