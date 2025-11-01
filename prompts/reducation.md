# Universal Code Reduction Guide

## Less Code, More Readable - Zero Behavior Change

### Primary Goal

🎯 **REDUCE CODE VOLUME**

- Remove unnecessary code without changing behavior
- Use modern JavaScript/TypeScript and Web3 patterns efficiently
- Make code more readable and easier to understand
- Preserve all functionality and blockchain interactions exactly as-is

---

## General JavaScript/TypeScript Patterns

### Pattern 1: Remove Unnecessary Variables

#### ❌ Before: Extra Variables

```tsx
const TokenBalance = ({ address, tokenAddress }) => {
  const result = useTokenBalance(address, tokenAddress);
  const balance = result.data;
  const loading = result.isLoading;
  const error = result.error;

  if (loading) {
    return <div>Loading...</div>;
  }

  if (error) {
    return <div>Error</div>;
  }

  return <div>{balance}</div>;
};
```

#### ✅ After: Direct Destructuring

```tsx
const TokenBalance = ({ address, tokenAddress }) => {
  const { data, isLoading, error } = useTokenBalance(address, tokenAddress);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error</div>;

  return <div>{data}</div>;
};
```

**Reduction: 13 lines → 7 lines (46% less code)**

---

### Pattern 2: Remove Verbose Conditionals

#### ❌ Before: Verbose Checks

```tsx
const isValid = address !== null && address !== undefined && address !== "";
const shouldFetch = tokenAddress ? true : false;
const hasData = data && data.length > 0 ? true : false;

if (isValid === true) {
  // do something
}
```

#### ✅ After: Concise Checks

```tsx
const isValid = Boolean(address);
const shouldFetch = Boolean(tokenAddress);
const hasData = data?.length > 0;

if (isValid) {
  // do something
}
```

---

### Pattern 3: Optional Chaining & Nullish Coalescing

#### ❌ Before: Nested Conditionals

```tsx
const displayName = user && user.profile && user.profile.name ? user.profile.name : "Anonymous";
const balance = data && data.balance ? data.balance : "0";

if (config && config.settings && config.settings.enabled) {
  // do something
}
```

#### ✅ After: Modern Operators

```tsx
const displayName = user?.profile?.name ?? "Anonymous";
const balance = data?.balance ?? "0";

if (config?.settings?.enabled) {
  // do something
}
```

---

### Pattern 4: Early Returns

#### ❌ Before: Nested Conditionals

```tsx
const Component = () => {
  const { data, isLoading, error } = useData();

  return (
    <div>
      {isLoading ? (
        <div>Loading...</div>
      ) : error ? (
        <div>Error: {error.message}</div>
      ) : data ? (
        <div>
          <h1>Data</h1>
          <p>{data.value}</p>
        </div>
      ) : (
        <div>No data</div>
      )}
    </div>
  );
};
```

#### ✅ After: Early Returns

```tsx
const Component = () => {
  const { data, isLoading, error } = useData();

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  if (!data) return <div>No data</div>;

  return (
    <div>
      <h1>Data</h1>
      <p>{data.value}</p>
    </div>
  );
};
```

---

### Pattern 5: Template Literals

#### ❌ Before: String Concatenation

```tsx
const message = "Balance: " + balance + " tokens";
const url = baseUrl + "/api/" + endpoint + "?id=" + id;
const className = "btn " + (isActive ? "active" : "") + " " + size;
```

#### ✅ After: Template Literals

```tsx
const message = `Balance: ${balance} tokens`;
const url = `${baseUrl}/api/${endpoint}?id=${id}`;
const className = `btn ${isActive ? "active" : ""} ${size}`;
```

---

### Pattern 6: Array Methods Over Loops

#### ❌ Before: Manual Loops

```tsx
const validItems = [];
for (let i = 0; i < items.length; i++) {
  if (items[i].isValid) {
    validItems.push(items[i]);
  }
}

const ids = [];
for (const item of items) {
  ids.push(item.id);
}
```

#### ✅ After: Array Methods

```tsx
const validItems = items.filter((item) => item.isValid);
const ids = items.map((item) => item.id);
```

**Reduction: 8 lines → 2 lines (75% less code)**

---

### Pattern 7: Remove Unnecessary useEffect

#### ❌ Before: Unnecessary Effect

```tsx
const [formattedBalance, setFormattedBalance] = useState("0");

useEffect(() => {
  if (balance) {
    setFormattedBalance(formatUnits(balance, 18));
  }
}, [balance]);

return <div>{formattedBalance}</div>;
```

#### ✅ After: Direct Computation

```tsx
const formattedBalance = balance ? formatUnits(balance, 18) : "0";

return <div>{formattedBalance}</div>;
```

**Reduction: 7 lines → 3 lines (57% less code)**

---

### Pattern 8: Object Shorthand

#### ❌ Before: Verbose Object Syntax

```tsx
const user = {
  name: name,
  email: email,
  address: address,
};

const config = {
  enabled: enabled,
  timeout: timeout,
};
```

#### ✅ After: Property Shorthand

```tsx
const user = { name, email, address };
const config = { enabled, timeout };
```

---

## Web3-Specific Patterns (Vite + React + Wagmi + TanStack Query + Subgraph)

### Web3 Pattern 1: Direct Wagmi Hook Usage

#### ❌ Before: Extra Variables

```tsx
const TokenBalance = ({ address, tokenAddress }) => {
  const result = useReadContract({
    address: tokenAddress,
    abi: erc20Abi,
    functionName: "balanceOf",
    args: [address],
  });

  const balance = result.data;
  const loading = result.isLoading;
  const error = result.error;

  if (loading) {
    return <div>Loading...</div>;
  }

  if (error) {
    return <div>Error</div>;
  }

  return <div>{balance ? formatUnits(balance, 18) : "0"}</div>;
};
```

#### ✅ After: Direct Destructuring

```tsx
const TokenBalance = ({ address, tokenAddress }) => {
  const { data, isLoading, error } = useReadContract({
    address: tokenAddress,
    abi: erc20Abi,
    functionName: "balanceOf",
    args: [address],
  });

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error</div>;

  return <div>{data ? formatUnits(data, 18) : "0"}</div>;
};
```

**Reduction: 17 lines → 11 lines (35% less code)**

---

### Web3 Pattern 2: Simplify Wallet Connection

#### ❌ Before: Verbose Wallet Logic

```tsx
const WalletButton = () => {
  const accountData = useAccount();
  const address = accountData.address;
  const isConnected = accountData.isConnected;
  const isConnecting = accountData.isConnecting;

  const connectData = useConnect();
  const connectors = connectData.connectors;
  const connect = connectData.connect;

  const disconnectData = useDisconnect();
  const disconnect = disconnectData.disconnect;

  const handleConnect = () => {
    if (connectors && connectors.length > 0) {
      const connector = connectors[0];
      connect({ connector: connector });
    }
  };

  if (isConnecting) {
    return <button disabled>Connecting...</button>;
  }

  if (isConnected && address) {
    return <button onClick={() => disconnect()}>Disconnect {address}</button>;
  }

  return <button onClick={handleConnect}>Connect Wallet</button>;
};
```

#### ✅ After: Concise Wallet Logic

```tsx
const WalletButton = () => {
  const { address, isConnected, isConnecting } = useAccount();
  const { connect, connectors } = useConnect();
  const { disconnect } = useDisconnect();

  if (isConnecting) return <button disabled>Connecting...</button>;
  if (isConnected) return <button onClick={() => disconnect()}>Disconnect {address}</button>;

  return <button onClick={() => connect({ connector: connectors[0] })}>Connect Wallet</button>;
};
```

**Reduction: 30 lines → 12 lines (60% less code)**

---

### Web3 Pattern 3: Use TanStack Mutation for Contract Writes

#### ❌ Before: Manual Transaction State

```tsx
const TransferButton = ({ to, amount, tokenAddress }) => {
  const { writeContractAsync } = useWriteContract();
  const queryClient = useQueryClient();

  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [success, setSuccess] = useState(false);

  const handleTransfer = async () => {
    setLoading(true);
    setError(null);
    setSuccess(false);

    try {
      const hash = await writeContractAsync({
        address: tokenAddress,
        abi: erc20Abi,
        functionName: "transfer",
        args: [to, parseUnits(amount, 18)],
      });

      setLoading(false);
      setSuccess(true);
      queryClient.invalidateQueries({ queryKey: ["readContract"] });
    } catch (err) {
      setLoading(false);
      setError(err);
      setSuccess(false);
    }
  };

  return (
    <div>
      <button onClick={handleTransfer} disabled={loading}>
        {loading ? "Sending..." : "Transfer"}
      </button>
      {error && <div>Error: {error.message}</div>}
      {success && <div>Success!</div>}
    </div>
  );
};
```

#### ✅ After: Use TanStack Mutation

```tsx
const TransferButton = ({ to, amount, tokenAddress }) => {
  const { writeContractAsync } = useWriteContract();
  const queryClient = useQueryClient();

  const { mutate, isPending, error, isSuccess } = useMutation({
    mutationFn: () =>
      writeContractAsync({
        address: tokenAddress,
        abi: erc20Abi,
        functionName: "transfer",
        args: [to, parseUnits(amount, 18)],
      }),
    onSuccess: () => queryClient.invalidateQueries({ queryKey: ["readContract"] }),
  });

  return (
    <div>
      <button onClick={() => mutate()} disabled={isPending}>
        {isPending ? "Sending..." : "Transfer"}
      </button>
      {error && <div>Error: {error.message}</div>}
      {isSuccess && <div>Success!</div>}
    </div>
  );
};
```

**Reduction: 35 lines → 20 lines (43% less code)**

---

### Web3 Pattern 4: Simplify GraphQL Queries (Subgraph)

#### ❌ Before: Manual GraphQL State Management

```tsx
const TokenTransfers = ({ address }) => {
  const [transfers, setTransfers] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchTransfers = async () => {
      setLoading(true);
      setError(null);

      try {
        const query = `
          query GetTransfers($address: String!) {
            transfers(where: { from: $address }) {
              id
              from
              to
              value
              timestamp
            }
          }
        `;

        const response = await fetch(SUBGRAPH_URL, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            query: query,
            variables: { address: address },
          }),
        });

        const result = await response.json();
        const data = result.data;

        setTransfers(data.transfers);
        setLoading(false);
      } catch (err) {
        setError(err);
        setLoading(false);
      }
    };

    if (address) {
      fetchTransfers();
    }
  }, [address]);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error</div>;

  return (
    <div>
      {transfers.map((transfer) => (
        <div key={transfer.id}>{transfer.value}</div>
      ))}
    </div>
  );
};
```

#### ✅ After: Use TanStack Query with GraphQL

```tsx
const TRANSFERS_QUERY = `
  query GetTransfers($address: String!) {
    transfers(where: { from: $address }) {
      id
      from
      to
      value
      timestamp
    }
  }
`;

const fetchTransfers = async (address) => {
  const response = await fetch(SUBGRAPH_URL, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ query: TRANSFERS_QUERY, variables: { address } }),
  });
  return (await response.json()).data.transfers;
};

const TokenTransfers = ({ address }) => {
  const { data, isLoading, error } = useQuery({
    queryKey: ["transfers", address],
    queryFn: () => fetchTransfers(address),
    enabled: Boolean(address),
  });

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error</div>;

  return (
    <div>
      {data?.map((transfer) => (
        <div key={transfer.id}>{transfer.value}</div>
      ))}
    </div>
  );
};
```

**Reduction: 55 lines → 35 lines (36% less code)**

---

### Web3 Pattern 5: Use Multicall for Multiple Contract Reads

#### ❌ Before: Separate Hook Calls

```tsx
const TokenInfo = ({ tokenAddress }) => {
  const nameResult = useReadContract({
    address: tokenAddress,
    abi: erc20Abi,
    functionName: "name",
  });

  const symbolResult = useReadContract({
    address: tokenAddress,
    abi: erc20Abi,
    functionName: "symbol",
  });

  const decimalsResult = useReadContract({
    address: tokenAddress,
    abi: erc20Abi,
    functionName: "decimals",
  });

  const name = nameResult.data;
  const symbol = symbolResult.data;
  const decimals = decimalsResult.data;
  const loading = nameResult.isLoading || symbolResult.isLoading || decimalsResult.isLoading;

  if (loading) return <div>Loading...</div>;

  return (
    <div>
      <p>Name: {name}</p>
      <p>Symbol: {symbol}</p>
      <p>Decimals: {decimals}</p>
    </div>
  );
};
```

#### ✅ After: Use useReadContracts (Multicall)

```tsx
const TokenInfo = ({ tokenAddress }) => {
  const { data, isLoading } = useReadContracts({
    contracts: [
      { address: tokenAddress, abi: erc20Abi, functionName: "name" },
      { address: tokenAddress, abi: erc20Abi, functionName: "symbol" },
      { address: tokenAddress, abi: erc20Abi, functionName: "decimals" },
    ],
  });

  if (isLoading) return <div>Loading...</div>;

  const [name, symbol, decimals] = data?.map((d) => d.result) ?? [];

  return (
    <div>
      <p>Name: {name}</p>
      <p>Symbol: {symbol}</p>
      <p>Decimals: {decimals}</p>
    </div>
  );
};
```

**Reduction: 35 lines → 20 lines (43% less code)**

---

### Web3 Pattern 6: Simplify Address Formatting

#### ❌ Before: Manual Address Truncation

```tsx
const formatAddress = (address) => {
  if (!address) return "";
  const start = address.substring(0, 6);
  const end = address.substring(address.length - 4);
  return start + "..." + end;
};

const AddressDisplay = ({ address }) => {
  const formatted = formatAddress(address);
  return <div>{formatted}</div>;
};
```

#### ✅ After: Template Literal

```tsx
const AddressDisplay = ({ address }) => {
  if (!address) return null;
  return <div>{`${address.slice(0, 6)}...${address.slice(-4)}`}</div>;
};
```

**Reduction: 11 lines → 4 lines (64% less code)**

---

### Web3 Pattern 7: Simplify Transaction Waiting

#### ❌ Before: Manual Transaction Tracking

```tsx
const SendTransaction = () => {
  const { writeContractAsync } = useWriteContract();
  const [hash, setHash] = useState(null);
  const [receipt, setReceipt] = useState(null);
  const [loading, setLoading] = useState(false);

  const waitResult = useWaitForTransactionReceipt({ hash });

  useEffect(() => {
    if (waitResult.data) {
      setReceipt(waitResult.data);
      setLoading(false);
    }
  }, [waitResult.data]);

  const send = async () => {
    setLoading(true);
    const txHash = await writeContractAsync({...});
    setHash(txHash);
  };

  return (
    <div>
      <button onClick={send} disabled={loading}>Send</button>
      {loading && <div>Waiting for confirmation...</div>}
      {receipt && <div>Confirmed!</div>}
    </div>
  );
};
```

#### ✅ After: Combined Logic

```tsx
const SendTransaction = () => {
  const { writeContractAsync } = useWriteContract();
  const [hash, setHash] = useState(null);

  const { data: receipt, isLoading } = useWaitForTransactionReceipt({
    hash,
    query: { enabled: Boolean(hash) },
  });

  const send = async () => {
    const txHash = await writeContractAsync({...});
    setHash(txHash);
  };

  return (
    <div>
      <button onClick={send} disabled={isLoading}>Send</button>
      {isLoading && <div>Waiting for confirmation...</div>}
      {receipt && <div>Confirmed!</div>}
    </div>
  );
};
```

**Reduction: 30 lines → 20 lines (33% less code)**

---

### Web3 Pattern 8: Simplify Chain Checks

#### ❌ Before: Verbose Chain Validation

```tsx
const CheckNetwork = () => {
  const { chain } = useAccount();
  const { chains } = useConfig();
  const { switchChain } = useSwitchChain();

  const isCorrectChain = () => {
    if (!chain) return false;
    const supportedChain = chains.find((c) => c.id === chain.id);
    return supportedChain !== undefined;
  };

  const handleSwitch = () => {
    const mainnetChain = chains.find((c) => c.id === 1);
    if (mainnetChain) {
      switchChain({ chainId: mainnetChain.id });
    }
  };

  if (!chain || !isCorrectChain()) {
    return <button onClick={handleSwitch}>Switch to Mainnet</button>;
  }

  return <div>Connected to {chain.name}</div>;
};
```

#### ✅ After: Direct Chain Check

```tsx
const CheckNetwork = () => {
  const { chain } = useAccount();
  const { switchChain } = useSwitchChain();

  if (chain?.id !== 1) {
    return <button onClick={() => switchChain({ chainId: 1 })}>Switch to Mainnet</button>;
  }

  return <div>Connected to {chain.name}</div>;
};
```

**Reduction: 24 lines → 11 lines (54% less code)**

---

### Web3 Pattern 9: Simplify Balance Formatting

#### ❌ Before: Multiple Formatting Steps

```tsx
const BalanceDisplay = ({ address, tokenAddress }) => {
  const { data } = useReadContract({
    address: tokenAddress,
    abi: erc20Abi,
    functionName: "balanceOf",
    args: [address],
  });

  const rawBalance = data;
  let formattedBalance = "0";

  if (rawBalance) {
    const balanceInEther = formatUnits(rawBalance, 18);
    const balanceAsNumber = parseFloat(balanceInEther);
    const roundedBalance = balanceAsNumber.toFixed(4);
    formattedBalance = roundedBalance;
  }

  return <div>Balance: {formattedBalance}</div>;
};
```

#### ✅ After: Direct Formatting

```tsx
const BalanceDisplay = ({ address, tokenAddress }) => {
  const { data } = useReadContract({
    address: tokenAddress,
    abi: erc20Abi,
    functionName: "balanceOf",
    args: [address],
  });

  const formatted = data ? Number(formatUnits(data, 18)).toFixed(4) : "0";

  return <div>Balance: {formatted}</div>;
};
```

**Reduction: 20 lines → 12 lines (40% less code)**

---

### Web3 Pattern 10: Simplify Subgraph Pagination

#### ❌ Before: Manual Pagination Logic

```tsx
const TokenList = () => {
  const [tokens, setTokens] = useState([]);
  const [skip, setSkip] = useState(0);
  const [hasMore, setHasMore] = useState(true);
  const [loading, setLoading] = useState(false);

  const fetchMore = async () => {
    setLoading(true);
    const query = `
      query GetTokens($skip: Int!) {
        tokens(first: 10, skip: $skip) {
          id
          name
          symbol
        }
      }
    `;

    const response = await fetch(SUBGRAPH_URL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ query, variables: { skip } }),
    });

    const result = await response.json();
    const newTokens = result.data.tokens;

    setTokens([...tokens, ...newTokens]);
    setSkip(skip + 10);
    setHasMore(newTokens.length === 10);
    setLoading(false);
  };

  useEffect(() => {
    fetchMore();
  }, []);

  return (
    <div>
      {tokens.map((token) => (
        <div key={token.id}>{token.name}</div>
      ))}
      {hasMore && (
        <button onClick={fetchMore} disabled={loading}>
          Load More
        </button>
      )}
    </div>
  );
};
```

#### ✅ After: Use TanStack Query Infinite Query

```tsx
const TOKENS_QUERY = `
  query GetTokens($skip: Int!) {
    tokens(first: 10, skip: $skip) { id name symbol }
  }
`;

const fetchTokens = async ({ pageParam = 0 }) => {
  const response = await fetch(SUBGRAPH_URL, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ query: TOKENS_QUERY, variables: { skip: pageParam } }),
  });
  return (await response.json()).data.tokens;
};

const TokenList = () => {
  const { data, fetchNextPage, hasNextPage, isFetchingNextPage } = useInfiniteQuery({
    queryKey: ["tokens"],
    queryFn: fetchTokens,
    getNextPageParam: (lastPage, pages) => (lastPage.length === 10 ? pages.length * 10 : undefined),
    initialPageParam: 0,
  });

  return (
    <div>
      {data?.pages.flat().map((token) => (
        <div key={token.id}>{token.name}</div>
      ))}
      {hasNextPage && (
        <button onClick={() => fetchNextPage()} disabled={isFetchingNextPage}>
          Load More
        </button>
      )}
    </div>
  );
};
```

**Reduction: 45 lines → 25 lines (44% less code)**

---

## Complete Examples: Before & After

### General Example

#### ❌ Before: Verbose Component (45 lines)

```tsx
const TokenTransfer: FC<Props> = (props) => {
  const { tokenAddress } = props;
  const { address: userAddress } = useAccount();

  const [toAddress, setToAddress] = useState("");
  const [amount, setAmount] = useState("");
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);
  const [success, setSuccess] = useState(false);

  const transfer = useTokenTransfer();

  const handleToAddressChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const value = e.target.value;
    setToAddress(value);
  };

  const handleAmountChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const value = e.target.value;
    setAmount(value);
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    setLoading(true);
    setError(null);
    setSuccess(false);

    try {
      await transfer.mutateAsync({
        to: toAddress as `0x${string}`,
        amount: amount,
        tokenAddress: tokenAddress,
      });

      setLoading(false);
      setSuccess(true);
      setToAddress("");
      setAmount("");
    } catch (err) {
      setLoading(false);
      setError(err as Error);
      setSuccess(false);
    }
  };

  if (!userAddress) {
    return <div>Please connect wallet</div>;
  }

  return (
    <form onSubmit={handleSubmit}>
      <input value={toAddress} onChange={handleToAddressChange} />
      <input value={amount} onChange={handleAmountChange} />
      <button type="submit" disabled={loading}>
        {loading ? "Sending..." : "Send"}
      </button>
      {error && <div>Error: {error.message}</div>}
      {success && <div>Success!</div>}
    </form>
  );
};
```

#### ✅ After: Concise Component (23 lines)

```tsx
const TokenTransfer: FC<Props> = ({ tokenAddress }) => {
  const { address } = useAccount();
  const [to, setTo] = useState("");
  const [amount, setAmount] = useState("");
  const transfer = useTokenTransfer();

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    try {
      await transfer.mutateAsync({ to: to as `0x${string}`, amount, tokenAddress });
      setTo("");
      setAmount("");
    } catch (err) {
      // Error handled by mutation
    }
  };

  if (!address) return <div>Please connect wallet</div>;

  return (
    <form onSubmit={handleSubmit}>
      <input value={to} onChange={(e) => setTo(e.target.value)} />
      <input value={amount} onChange={(e) => setAmount(e.target.value)} />
      <button type="submit" disabled={transfer.isPending}>
        {transfer.isPending ? "Sending..." : "Send"}
      </button>
      {transfer.error && <div>Error: {transfer.error.message}</div>}
      {transfer.isSuccess && <div>Success!</div>}
    </form>
  );
};
```

**Reduction: 45 lines → 23 lines (49% less code)**

---

### Web3 Example: Full Dashboard

#### ❌ Before: Verbose Web3 Component (60 lines)

```tsx
const TokenDashboard: FC = () => {
  const accountData = useAccount();
  const address = accountData.address;
  const isConnected = accountData.isConnected;

  const balanceResult = useReadContract({
    address: TOKEN_ADDRESS,
    abi: erc20Abi,
    functionName: "balanceOf",
    args: [address],
  });

  const balance = balanceResult.data;
  const balanceLoading = balanceResult.isLoading;

  const [transfers, setTransfers] = useState([]);
  const [transfersLoading, setTransfersLoading] = useState(false);

  useEffect(() => {
    const fetchTransfers = async () => {
      if (!address) return;

      setTransfersLoading(true);
      const query = `
        query GetTransfers($address: String!) {
          transfers(where: { from: $address }) {
            id
            to
            value
          }
        }
      `;

      const response = await fetch(SUBGRAPH_URL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ query, variables: { address } }),
      });

      const result = await response.json();
      setTransfers(result.data.transfers);
      setTransfersLoading(false);
    };

    fetchTransfers();
  }, [address]);

  if (!isConnected) {
    return <div>Please connect wallet</div>;
  }

  if (balanceLoading || transfersLoading) {
    return <div>Loading...</div>;
  }

  return (
    <div>
      <h1>Token Dashboard</h1>
      <p>Balance: {balance ? formatUnits(balance, 18) : "0"}</p>
      <h2>Recent Transfers</h2>
      {transfers.map((t) => (
        <div key={t.id}>
          {t.to}: {t.value}
        </div>
      ))}
    </div>
  );
};
```

#### ✅ After: Concise Web3 Component (28 lines)

```tsx
const TRANSFERS_QUERY = `
  query GetTransfers($address: String!) {
    transfers(where: { from: $address }) { id to value }
  }
`;

const TokenDashboard: FC = () => {
  const { address, isConnected } = useAccount();

  const { data: balance, isLoading: balanceLoading } = useReadContract({
    address: TOKEN_ADDRESS,
    abi: erc20Abi,
    functionName: "balanceOf",
    args: [address],
  });

  const { data: transfers, isLoading: transfersLoading } = useQuery({
    queryKey: ["transfers", address],
    queryFn: async () => {
      const res = await fetch(SUBGRAPH_URL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ query: TRANSFERS_QUERY, variables: { address } }),
      });
      return (await res.json()).data.transfers;
    },
    enabled: Boolean(address),
  });

  if (!isConnected) return <div>Please connect wallet</div>;
  if (balanceLoading || transfersLoading) return <div>Loading...</div>;

  return (
    <div>
      <h1>Token Dashboard</h1>
      <p>Balance: {balance ? formatUnits(balance, 18) : "0"}</p>
      <h2>Recent Transfers</h2>
      {transfers?.map((t) => (
        <div key={t.id}>
          {t.to}: {t.value}
        </div>
      ))}
    </div>
  );
};
```
