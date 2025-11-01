# Zero Complexity TSX Refactoring

## Pages & Components - Native-First Approach

### Primary Goals

🎯 **REDUCE COMPLEXITY**

- Remove all custom abstractions and wrapper components
- Use native React patterns directly
- Eliminate mixed patterns and inconsistencies
- Keep functionality identical while simplifying code
- Clean, readable JSX with minimal logic

### Before vs After Pattern

#### ❌ AVOID: Over-Abstracted Components

```tsx
// DON'T - Complex wrapper with too much logic
interface ComplexCardProps {
  data: any;
  loading?: boolean;
  error?: Error;
  onAction?: () => void;
  customRenderer?: (data: any) => ReactNode;
  showHeader?: boolean;
  headerConfig?: HeaderConfig;
  footerConfig?: FooterConfig;
  theme?: "light" | "dark";
  animationConfig?: AnimationConfig;
}

const ComplexCard: FC<ComplexCardProps> = ({ data, loading, error, onAction, customRenderer, showHeader = true, headerConfig, footerConfig, theme = "light", animationConfig }) => {
  const [internalState, setInternalState] = useState();
  const processedData = useMemo(() => transformData(data), [data]);

  useEffect(() => {
    // Complex side effects
  }, [processedData]);

  if (loading) return <CustomLoader config={animationConfig} />;
  if (error) return <CustomError error={error} theme={theme} />;

  return (
    <AnimatedWrapper config={animationConfig}>
      {showHeader && <CustomHeader {...headerConfig} />}
      {customRenderer ? customRenderer(processedData) : <DefaultRenderer data={processedData} />}
      {footerConfig && <CustomFooter {...footerConfig} />}
    </AnimatedWrapper>
  );
};
```

#### ✅ DO: Simple, Direct Components

```tsx
// DO - Simple component with clear purpose
interface TokenBalanceProps {
  address: `0x${string}`;
  tokenAddress: `0x${string}`;
}

const TokenBalance: FC<TokenBalanceProps> = ({ address, tokenAddress }) => {
  const { data: balance, isLoading, error } = useTokenBalance(address, tokenAddress);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return <div>Balance: {balance ? formatUnits(balance, 18) : "0"} tokens</div>;
};
```

### Native-First Component Patterns

#### **1. Simple Display Component**

```tsx
import { FC } from "react";
import { useTokenBalance, useTokenSymbol } from "@/hooks";
import { formatUnits } from "viem";

interface TokenDisplayProps {
  address: `0x${string}`;
  tokenAddress: `0x${string}`;
}

export const TokenDisplay: FC<TokenDisplayProps> = ({ address, tokenAddress }) => {
  const { data: balance, isLoading: balanceLoading } = useTokenBalance(address, tokenAddress);
  const { data: symbol, isLoading: symbolLoading } = useTokenSymbol(tokenAddress);

  if (balanceLoading || symbolLoading) {
    return <div>Loading...</div>;
  }

  return (
    <div>
      <p>Token: {symbol}</p>
      <p>Balance: {balance ? formatUnits(balance, 18) : "0"}</p>
    </div>
  );
};
```

#### **2. Simple Form Component**

```tsx
import { FC, useState } from "react";
import { useTokenTransfer } from "@/hooks";
import { parseUnits } from "viem";

interface TransferFormProps {
  tokenAddress: `0x${string}`;
}

export const TransferForm: FC<TransferFormProps> = ({ tokenAddress }) => {
  const [to, setTo] = useState("");
  const [amount, setAmount] = useState("");
  const transfer = useTokenTransfer();

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();

    transfer.mutate({
      to: to as `0x${string}`,
      amount,
      tokenAddress,
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={to} onChange={(e) => setTo(e.target.value)} placeholder="Recipient address" />
      <input type="text" value={amount} onChange={(e) => setAmount(e.target.value)} placeholder="Amount" />
      <button type="submit" disabled={transfer.isPending}>
        {transfer.isPending ? "Sending..." : "Send"}
      </button>
      {transfer.error && <div>Error: {transfer.error.message}</div>}
      {transfer.isSuccess && <div>Transfer successful!</div>}
    </form>
  );
};
```

#### **3. Simple Page Component**

```tsx
import { FC } from "react";
import { useAccount } from "wagmi";
import { TokenDisplay } from "@/components/TokenDisplay";
import { TransferForm } from "@/components/TransferForm";

const TOKEN_ADDRESS = "0x..." as const;

export const TokenPage: FC = () => {
  const { address, isConnected } = useAccount();

  if (!isConnected) {
    return <div>Please connect your wallet</div>;
  }

  return (
    <div>
      <h1>Token Dashboard</h1>
      <TokenDisplay address={address!} tokenAddress={TOKEN_ADDRESS} />
      <TransferForm tokenAddress={TOKEN_ADDRESS} />
    </div>
  );
};
```

### Refactoring Rules for Components

#### **RULE 1: No Wrapper Components**

```tsx
// ❌ DON'T create generic wrapper components
interface WrapperProps {
  children: ReactNode;
  loading?: boolean;
  error?: Error;
  showHeader?: boolean;
}

const GenericWrapper: FC<WrapperProps> = ({ children, loading, error, showHeader }) => {
  if (loading) return <Loader />;
  if (error) return <ErrorDisplay error={error} />;
  return (
    <div>
      {showHeader && <Header />}
      {children}
    </div>
  );
};

// ✅ DO handle states directly in components
const TokenBalance: FC<TokenBalanceProps> = ({ address, tokenAddress }) => {
  const { data, isLoading, error } = useTokenBalance(address, tokenAddress);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  return <div>Balance: {formatUnits(data, 18)}</div>;
};
```

#### **RULE 2: One Responsibility Per Component**

```tsx
// ❌ DON'T combine multiple features
const TokenDashboard: FC = () => {
  const balance = useTokenBalance();
  const transfer = useTokenTransfer();
  const approve = useTokenApprove();
  const history = useTransactionHistory();

  // 200+ lines of mixed logic
  return <div>{/* Everything in one component */}</div>;
};

// ✅ DO split into focused components
const TokenDashboard: FC = () => {
  return (
    <div>
      <TokenBalance />
      <TransferSection />
      <TransactionHistory />
    </div>
  );
};

const TokenBalance: FC = () => {
  const { data } = useTokenBalance();
  return <div>Balance: {data}</div>;
};

const TransferSection: FC = () => {
  const transfer = useTokenTransfer();
  return <TransferForm onSubmit={transfer.mutate} />;
};
```

#### **RULE 3: No Logic in JSX**

```tsx
// ❌ DON'T put logic in JSX
const TokenDisplay: FC = () => {
  const { data } = useTokenBalance();

  return <div>{data && data.length > 0 ? formatUnits(data, 18) !== "0" ? <span>{formatUnits(data, 18)}</span> : <span>No balance</span> : <span>Loading...</span>}</div>;
};

// ✅ DO extract logic before return
const TokenDisplay: FC = () => {
  const { data, isLoading } = useTokenBalance();

  if (isLoading) return <div>Loading...</div>;

  const formattedBalance = data ? formatUnits(data, 18) : "0";
  const hasBalance = formattedBalance !== "0";

  return <div>{hasBalance ? <span>{formattedBalance}</span> : <span>No balance</span>}</div>;
};
```

#### **RULE 4: Simple Props Interface**

```tsx
// ❌ DON'T create complex prop interfaces
interface ComplexProps {
  data?: any;
  config?: {
    showHeader?: boolean;
    theme?: "light" | "dark";
    animation?: AnimationConfig;
  };
  callbacks?: {
    onSuccess?: () => void;
    onError?: (err: Error) => void;
    onLoad?: () => void;
  };
  customRenderers?: {
    header?: (data: any) => ReactNode;
    body?: (data: any) => ReactNode;
    footer?: (data: any) => ReactNode;
  };
}

// ✅ DO use simple, typed props
interface TokenBalanceProps {
  address: `0x${string}`;
  tokenAddress: `0x${string}`;
}
```

### Component Structure Template

```tsx
import { FC } from "react";
import { useYourHook } from "@/hooks";

// 1. Define simple props interface
interface YourComponentProps {
  requiredProp: string;
  optionalProp?: number;
}

// 2. Component with clear name
export const YourComponent: FC<YourComponentProps> = ({ requiredProp, optionalProp }) => {
  // 3. Hooks at the top
  const { data, isLoading, error } = useYourHook(requiredProp);

  // 4. Local state if needed
  const [localState, setLocalState] = useState("");

  // 5. Event handlers
  const handleAction = () => {
    // Simple, focused logic
  };

  // 6. Early returns for loading/error states
  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  // 7. Compute derived values
  const displayValue = data ? formatData(data) : "N/A";

  // 8. Return simple JSX
  return (
    <div>
      <p>{displayValue}</p>
      <button onClick={handleAction}>Action</button>
    </div>
  );
};
```

### Page Structure Template

```tsx
import { FC } from "react";
import { useAccount } from "wagmi";
import { ComponentA } from "@/components/ComponentA";
import { ComponentB } from "@/components/ComponentB";

export const YourPage: FC = () => {
  // 1. Wallet connection check
  const { address, isConnected } = useAccount();

  // 2. Early return if not connected
  if (!isConnected) {
    return (
      <div>
        <h1>Please Connect Wallet</h1>
      </div>
    );
  }

  // 3. Simple page layout with components
  return (
    <div>
      <h1>Page Title</h1>
      <ComponentA address={address!} />
      <ComponentB address={address!} />
    </div>
  );
};
```

### Refactoring Prompt Template

---

**TSX COMPONENT REFACTORING**

**Primary Goal:** Remove ALL custom abstractions and use native React patterns

**Current Code:**

```tsx
[PASTE YOUR COMPONENT/PAGE TSX CODE HERE]
```

**Refactoring Requirements:**

1. **REMOVE all wrapper components** - handle states directly
2. **ELIMINATE mixed patterns** - one consistent approach
3. **NO complex prop interfaces** - simple, typed props only
4. **ONE responsibility per component** - split large components
5. **ZERO logic in JSX** - compute values before return
6. **KEEP functionality identical** - same behavior, simpler code

**Forbidden Patterns:**

- Generic wrapper components (LoadingWrapper, ErrorBoundary wrappers, etc.)
- Complex prop configurations with nested objects
- Logic inside JSX (ternaries with computations)
- Components with multiple responsibilities
- Custom state management abstractions
- Prop drilling more than 2 levels

**Required Patterns:**

- Direct hook usage in components
- Simple prop interfaces with clear types
- Early returns for loading/error states
- Extracted logic before JSX return
- Focused components with single purpose
- Native React patterns only

**Expected Result:**

- Simpler components with identical functionality
- Native React patterns throughout
- Easy to read and understand JSX
- Clear separation of concerns
- No custom abstractions

---

### File Structure - Flat and Simple

```
src/
├── pages/
│   ├── TokenPage.tsx         # ONE feature per page
│   ├── SwapPage.tsx          # ONE feature per page
│   └── DashboardPage.tsx     # ONE feature per page
├── components/
│   ├── TokenBalance.tsx      # ONE responsibility
│   ├── TokenTransfer.tsx     # ONE responsibility
│   ├── WalletConnect.tsx     # ONE responsibility
│   └── index.ts             # Simple exports
└── hooks/
    ├── useTokenBalance.ts
    ├── useTokenTransfer.ts
    └── index.ts
```

### Migration Checklist

#### **Phase 1: Identify Complexity**

- [ ] Find all wrapper components
- [ ] Locate components with multiple responsibilities
- [ ] Identify complex prop interfaces
- [ ] Find logic inside JSX
- [ ] Locate mixed patterns

#### **Phase 2: Simplify Components**

- [ ] Remove wrapper components
- [ ] Split multi-responsibility components
- [ ] Simplify prop interfaces
- [ ] Extract JSX logic to variables
- [ ] Use consistent patterns

#### **Phase 3: Verify Functionality**

- [ ] All features work identically
- [ ] All props are properly typed
- [ ] No runtime errors
- [ ] Clean, readable code

### Real-World Example: Before & After

#### Before: Complex Component

```tsx
const TokenDashboard: FC<ComplexDashboardProps> = ({ config, customHandlers, themeConfig }) => {
  const [state, dispatch] = useReducer(complexReducer, initialState);
  const { data } = useComplexHook(config);

  useEffect(() => {
    // Complex side effects
  }, [data, config, state]);

  return (
    <ThemeWrapper theme={themeConfig}>
      <LoadingBoundary loading={state.loading}>
        <ErrorBoundary error={state.error}>
          {data?.map((item) =>
            customHandlers?.renderItem ? (
              customHandlers.renderItem(item)
            ) : (
              <DefaultItemRenderer
                item={item}
                config={config.itemConfig}
                onAction={(action) => {
                  dispatch({ type: action.type, payload: action.payload });
                  customHandlers?.onAction?.(action);
                }}
              />
            ),
          )}
        </ErrorBoundary>
      </LoadingBoundary>
    </ThemeWrapper>
  );
};
```

#### After: Simple Component

```tsx
const TokenDashboard: FC = () => {
  const { address } = useAccount();

  if (!address) {
    return <div>Connect wallet</div>;
  }

  return (
    <div>
      <h1>Token Dashboard</h1>
      <TokenBalance address={address} />
      <TokenTransfer address={address} />
      <TransactionList address={address} />
    </div>
  );
};

const TokenBalance: FC<{ address: `0x${string}` }> = ({ address }) => {
  const { data, isLoading } = useTokenBalance(address, TOKEN_ADDRESS);

  if (isLoading) return <div>Loading...</div>;

  return <div>Balance: {data ? formatUnits(data, 18) : "0"}</div>;
};
```

### Success Metrics

**Before Refactoring:**

- Complex wrapper components
- Mixed patterns across pages
- Logic scattered in JSX
- Hard to understand component hierarchy

**After Refactoring:**

- Simple, focused components
- Consistent patterns throughout
- Clear, readable JSX
- Easy to modify and maintain

**Key Benefits:**

- **Less Code** - Remove wrapper components
- **More Readable** - Clear component structure
- **Easier Testing** - Simple, focused components
- **Better Performance** - No unnecessary abstractions
- **Future Proof** - Standard React patterns
