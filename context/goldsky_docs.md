# Goldsky Documentation

Scraped from docs.goldsky.com

Total pages: 27

---

## 1. Shared vs. dedicated

**URL:** https://docs.goldsky.com/subgraphs/serverless-vs-dedicated

# Shared vs. dedicated

## Serverless subgraphs

When you make a new subgraph on Goldsky, by default it's hosted on our highly resilient **Serverless Subgraph Platform**.

The platform is fully autoscaling, with a re-engineered RPC and storage layer, and is tuned for fast indexing across the majority of use-cases. It's also completely backwards compatible and runs the same WASM engine as the vanilla open-source graph-node engine.

* Optimized RPC multi-provider layer with a global cache that uses a combination of dedicated and commercial RPC APIs for uptime
* I/O optimized database with under 1ms average commit times

## Dedicated subgraph indexers

When you need improved customizability and performance, Goldsky offers dedicated subgraph indexing nodes. Dedicated machines are provisioned for your project, allowing for customization and optimization at both the indexing and querying layers.

### Indexing enhancements

* support for any EVM-compatible private chain or app chain
* custom RPC layer optimizations methods based on subgraph needs to improve indexing speed

### Querying enhancements

* enable caching with custom rules
* custom database optimizations to speed up specific query patterns, bringing expensive queries down from seconds to milliseconds

To launch a dedicated indexer, please contact us via email at [sales@goldsky.com](mailto:sales@goldsky.com) to get started within one business day.

### Limitations

By default, dedicated indexers are disconnected from Goldsky's [Mirror](mirror/sources/subgraphs) functionality; if you'd like to index and mirror a custom EVM chain, [contact us](mailto:sales@goldsky.com).

---

## 2. Create no-code subgraphs

**URL:** https://docs.goldsky.com/subgraphs/guides/create-a-no-code-subgraph

# Create no-code subgraphs

## What you'll need

1. The contract address you're interested in indexing.
2. The ABI (Application Binary Interface) of the contract.

## Walkthrough






 If the contract you’re interested in indexing is a contract you deployed, then you’ll have the contract address and ABI handy. Otherwise, you can use a mix of public explorer tools to find this information. For example, if we’re interested in indexing the [friend.tech](http://friend.tech) contract…

 1. Find the contract address from [Dappradar](https://dappradar.com/)
 2. Click through to the [block explorer](https://basescan.org/address/0xcf205808ed36593aa40a44f10c7f7c2f67d4a4d4#code) where the ABI can be found under the `Contract ABI` section.

 Save the ABI to your local file system and make a note of the contract address. Also make a note of the block number the contract was deployed at, you’ll need this at a later step.



 The next step is to create the Instant Subgraph configuration file (e.g. `friendtech-config.json`). This file consists of five key sections:

 1. Config version number
 2. Config name
 3. ABIs
 4. Chains
 5. Contract instances

 ### Version number


 As of October 2023, our Instant Subgraph configuration system is on version 1.
 This may change in the future. This is **not the version number of your
 subgraph**, but of Goldsky's configuration file format.


 ### Config name

 This is a name of your choice that helps you understand what this config is for. It is only used for internal debugging. For this guide, we'll use `friendtech`.

 ### ABIs, chains, and contract instances

 These three sections are interconnected.

 1. Name your ABI and enter the path to the ABI file you saved earlier (relative to where this config file is located). In this case, `ftshares` and `abi.json`.
 2. Write out the contract instance, referencing the ABI you named earlier, address it's deployed at, chain it's on, the start block.

 ```json friendtech-config.json theme={null}
 {
 "version": "1",
 "name": "friendtech",
 "abis": {
 "ftshares": {
 "path": "./abi.json"
 }
 },
 "instances": [
 {
 "abi": "ftshares",
 "address": "0xCF205808Ed36593aa40a44F10c7f7C2F67d4A4d4",
 "startBlock": 2430440,
 "chain": "base"
 }
 ]
 }
 ```


 The abi name in `instances` should match a key in `abis`, in this example,
 `ftshares`. It is possible to have more than one `chains` and more than one
 ABI. Multiple chains will result in multiple subgraphs. The file `abi.json` in
 this example should contain the friendtech ABI [downloaded from
 here](https://api.basescan.org/api?module=contract\&action=getabi\&address=0xcf205808ed36593aa40a44f10c7f7c2f67d4a4d4\&format=raw).


 This configuration can handle multiple contracts with distinct ABIs, the same contract across multiple chains, or multiple contracts with distinct ABIs on multiple chains.

 **For a complete reference of the various properties, please see the [Instant Subgraphs references docs](/reference/config-file/instant-subgraph)**



 With your configuration file ready, it's time to deploy the subgraph.

 1. Open the CLI and log in to your Goldsky account with the command: `goldsky login`.
 2. Deploy the subgraph using the command: `goldsky subgraph deploy name/version --from-abi

`, then pass in the path to the config file you created. Note - do NOT pass in the ABI itself, but rather the config file defined above. Example: `goldsky subgraph deploy friendtech/1.0 --from-abi friendtech-config.json`

 Goldsky will generate all the necessary subgraph code, deploy it, and return an endpoint that you can start querying.

 Clicking the endpoint link takes you to a web client where you can browse the schema and draft queries to integrate into your app.



## Extending your subgraph with enrichments

Enrichments are a powerful way to add additional data to your subgraph by performing eth calls in the middle of an event or call handler.

See the [enrichments configuration reference](/reference/config-file/instant-subgraph#instance-enrichment) for more information on how to define these enrichments, and for an [example configuration with enrichments](/reference/config-file/instant-subgraph#nouns-enrichment-with-balances-on-transfer).

### Concepts

* Enrichments are defined at the instance level, and executed at the trigger handler level. This means that you can have different enrichments for different data sources or templates and that all enrichment executions are isolated to the handler they are being called from.
 * any additional imports from `@graphprotocol/graph-ts` beyond `BigInt`, `Bytes`, and `store` can be declared in the `options.imports` field of the enrichment (e.g., `BigDecimal`).
* Enrichments always begin by performing all eth calls first, if any eth calls are aborted then the enrichment as a whole is aborted.
 * calls marked as `required` or having another call declare them as a `depends_on` dependency will abort if the call is not successful, otherwise the call output value will remain as `null`.
 * calls marked as `declared` will configure the subgraph to execute the call prior to invoking the mapping handler. This can be useful for performance reasons, but only works for eth calls that have no mapping handler dependencies.
 * calls support `pre` and `post` expressions for `conditions` to test before and after the call, if either fails the call is aborted. Since these are expressions, they can be dynamic or constant values.
 * call `source` is an expression and therefore allows for dynamic values using math or concatenations. If the `source` is simply a contract address then it will be automatically converted to an `Address` type.
 * call `params` is an expression list and can also be dynamic values or constants.
* Enrichments support defining new entities as well as updating existing entities. If the entity name matches the trigger entity name, then the entity field mappings will be applied to the existing entity.
 * entity names should be singular and capitalized, this will ensure that the generated does not produce naming conflicts.
 * entity field mapping values are expressions and can be dynamic or constant values.
 * new enrichment entities are linked to the parent (trigger) entity that created them, with the parent (trigger) entity also linking to the new entity or entities in the opposite direction (always a collection type).
 * note that while you can define existing entities that are not the trigger entity, you may not update existing entities only create new instances of that entity.
 * entities support being created multiple times in a single enrichment, but require a unique `id` expression to be defined for each entity, `id` can by a dynamic value or a constant. this `id` is appended to the parent entity `id` to create a unique `id` for each enrichment entity in the list.
 * entities can be made mutable by setting the `explicit_id` flag to `true`, this will use the value of `id` without appending it to the parent entity `id`, creating an addressable entity that can be updated.

### Snippets

Below are some various examples of configurations for different scenarios. To keep each example brief, we will only show the `enrich` section of the configuration, and in most cases only the part of the `enrich` section that is relevant. See the [enrichments configuration reference](/reference/config-file/instant-subgraph#instance-enrichment) for the full configuration reference.

#### Options

Here we are enabling debugging for the enrichment (this will output the enrichment steps to the subgraph log), as well as importing `BigDecimal` for use in a `calls` or `entities` section.

```json theme={null}
"enrich": {
 "options": {
 "imports": ["BigDecimal"],
 "debugging": true
 }
}
```

#### Call self

Here we are calling a function on the same contract as the trigger event. This means we can omit the `abi` and `source` configuration fields, as they are implied in this scenario, we only need to include the `name` and `params` fields (if the function declares paramters). We can refer to the result of this call using `calls.balance`.

```json theme={null}
"calls": {
 "balance": {
 "name": "balanceOf",
 "params": "event.params.owner"
 }
}
```

#### Call dependency

Here we are creating a 2-call dependency, where the second call depends on the first call (the params are `calls.owner` meaning we need the value of the `owner` call before we can invoke `balanceOf`). This means that if the first call fails, the second call will not be executed. Calls are always executed in the order they are configured, so the second call will have access to the output of the first call (in this example, we use that output as a parameter to the second call). We can list multiple calls in the `depends_on` array to create a dependency graph (if needed). Adding a call to the `depends_on` array will not automatically re-order the calls, so be sure to list them in the correct order.

```json theme={null}
"calls": {
 "owner": {
 "name": "ownerOf",
 "params": "event.params.id"
 },
 "balance": {
 "depends_on": ["owner"],
 "name": "balanceOf",
 "params": "calls.owner"
 }
}
```

#### External contract call for known address

Here we are calling a function on an external contract, where we know the address of the contract ahead of time. In this case, we need to include the `abi` and `source` configuration fields.

```json theme={null}
"calls": {
 "usdc_balance": {
 "abi": "erc20",
 "source": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
 "name": "balanceOf",
 "params": "event.params.owner"
 }
}
```

#### External contract call for dynamic address

Here we are setting up a 2 call chain to first determine the contract address, then call a function on that contract. In our example, the `contractAddress` function is returning an `Address` type so we can use the call result directly in the `source` field of the second call. If `contractAddress` was instead returning a `string` type, then we would use `"source": "Address.fromString(calls.contract_address)"`, though this would be an unusual case to observe.

```json theme={null}
"calls": {
 "contract_address": {
 "name": "contractAddress",
 "params": "event.params.id"
 },
 "balance": {
 "depends_on": ["contract_address"],
 "abi": "erc20",
 "source": "calls.contract_address",
 "name": "balanceOf",
 "params": "event.params.owner"
 }
}
```

#### Required call

Here we are marking a call as required, meaning that if the call fails then the enrichment as a whole will be aborted. This is useful when you do not want to create a new entity (or enrich an existing entity) if the call does not return any meaningful data. Also note that when using `depends_on`, the dependency call is automatically marked as required. This should be used when the address of the contract being called may not always implement the function being called.

```json theme={null}
"calls": {
 "balance": {
 "abi": "erc20",
 "name": "balanceOf",
 "source": "event.params.address",
 "params": "event.params.owner",
 "required": true
 }
}
```

#### Pre and post conditions

Here we are using conditions to prevent a call from being executed or to abort the enrichment if the call result is not satisfactory. Avoiding an eth call can have a big performance impact if the inputs to the call are often invalid. Avoiding the creation of an entity can save on entity counts if the entity is not needed or useful for various call results. Conditions are simply checked at their target site in the enrichment, and evaluated to its negation to check if an abort is necessary (e.g., `true` becomes `!(true)`, which is always false and therefore never aborts). In this example, we're excluding the call if the `owner` is in a deny list, and we're aborting the enrichment if the balance is `0`.

```json theme={null}
"calls": {
 "balance": {
 "name": "balanceOf",
 "params": "event.params.owner",
 "conditions": {
 "pre": "![Address.zero().toHexString(), \"0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03\"].includes(event.params.owner.toHexString())",
 "post": "result.value.gt(BigInt.zero())"
 }
 }
}
```

#### Simple entity field mapping constant

Here were are simply replicating the `id` field from the event params into our enrichment entity. This can be useful if you want to filter or sort the enrichment entities by this field.

```json theme={null}
 "MyEntity": {
 "id uint256": "event.params.id"
 },
```

#### Simple entity field mapping expression

Here we are applying a serialization function to the value of a call result. This is necessary as the enrichment code generator does not resolve the effective type of an expression, so if there is a type mismatch a serialization function must be applied (in this case `String` vs `Address`).

```json theme={null}
 "MyEntity": {
 "owner address": "calls.owner.toHexString()"
 },
```

#### Complex entity field mapping expression

Here we are conditionally setting the value of `usd_balance` on whether or not the `usdc_balance` call was successful. If the call was not successful, then we set the value to `BigDecimal.zero()`, otherwise we divide the call result by `10^6` (USDC decimals) to convert the balance to a `USD` value.

```json theme={null}
 "MyEntity": {
 "usd_balance fixed": "calls.usdc_balance === null ? BigDecimal.zero() : calls.usdc_balance!.divDecimal(BigInt.fromU32(10).pow(6).toBigDecimal())"
 },
```

Can't find what you're looking for? Reach out to us at [support@goldsky.com](mailto:support@goldsky.com) for help.

#### Multiple entity instances

Here we are creating multiple instances of an entity in a single enrichment. Each entity id will be suffixed with the provided `id` value.

```json theme={null}
 "MyEntity": [
 {
 "id": "'sender'",
 "mapping": {
 "balance fixed": "calls.balance"
 }
 },
 {
 "id": "'receiver'",
 "mapping": {
 "balance fixed": "calls.balance"
 }
 }
 ]
```

#### Addressable entity

Here we are creating an entity that is addressable by an explicit id. This means that we can update this entity with new values.

```json theme={null}
 "MyEntity": [
 {
 "id": "calls.owner.toHexString()",
 "explicit_id": true,
 "mapping": {
 "current_balance fixed": "calls.balance"
 }
 }
 ]
```


 *We must use an array for our entity definition to allow setting the
 `explicit_id` flag.*


## Examples

Here are some examples of various instant subgraph configurations. Each example builds on the previous example.

Each of these examples can be saved locally to a file (e.g., `subgraph.json`) and deployed using `goldsky subgraph deploy nouns/1.0.0 --from-abi subgraph.json`.

### Simple NOUNS example

This is a basic instant subgraph configuration, a great starting point for learning about instant subgraphs.

```json5 simple-nouns-config.json theme={null}
{
 version: "1",
 name: "nouns/1.0.0",
 abis: {
 nouns: [
 {
 anonymous: false,
 inputs: [
 {
 indexed: true,
 internalType: "address",
 name: "from",
 type: "address",
 },
 {
 indexed: true,
 internalType: "address",
 name: "to",
 type: "address",
 },
 {
 indexed: true,
 internalType: "uint256",
 name: "tokenId",
 type: "uint256",
 },
 ],
 name: "Transfer",
 type: "event",
 },
 ],
 },
 instances: [
 {
 abi: "nouns",
 address: "0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03",
 startBlock: 12985438,
 chain: "mainnet",
 },
 ],
}
```

### NOUNS enrichment with receiver balance on transfer

This example describes a very simple enrichment that adds a `balance` field to a `Balance` enrichment entity. This `balance` field is populated by calling the `balanceOf` function on the `to` address of the `Transfer` event.

```json5 nouns-balance-config.json theme={null}
{
 version: "1",
 name: "nouns/1.0.0",
 abis: {
 nouns: [
 {
 anonymous: false,
 inputs: [
 {
 indexed: true,
 internalType: "address",
 name: "from",
 type: "address",
 },
 {
 indexed: true,
 internalType: "address",
 name: "to",
 type: "address",
 },
 {
 indexed: true,
 internalType: "uint256",
 name: "tokenId",
 type: "uint256",
 },
 ],
 name: "Transfer",
 type: "event",
 },
 {
 inputs: [{ internalType: "address", name: "owner", type: "address" }],
 name: "balanceOf",
 outputs: [{ internalType: "uint256", name: "", type: "uint256" }],
 stateMutability: "view",
 type: "function",
 },
 ],
 },
 instances: [
 {
 abi: "nouns",
 address: "0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03",
 startBlock: 12985438,
 chain: "mainnet",
 enrich: {
 handlers: {
 "Transfer(indexed address,indexed address,indexed uint256)": {
 calls: {
 balance: {
 name: "balanceOf",
 params: "event.params.to",
 required: true,
 },
 },
 entities: {
 Balance: {
 "owner address": "event.params.to.toHexString()",
 "balance uint256": "calls.balance",
 },
 },
 },
 },
 },
 },
 ],
}
```

### NOUNS enrichment with sender & receiver balance on transfer entities

This example alters our previous example by capturing the `balance` field on both `FromBalance` and `ToBalance` enrichment entities. This `balance` field is populated by calling the `balanceOf` function on both the `from` and `to` address of the `Transfer` event.

```json5 nouns-balance-config-2.json theme={null}
{
 version: "1",
 name: "nouns/1.0.0",
 abis: {
 nouns: [
 {
 anonymous: false,
 inputs: [
 {
 indexed: true,
 internalType: "address",
 name: "from",
 type: "address",
 },
 {
 indexed: true,
 internalType: "address",
 name: "to",
 type: "address",
 },
 {
 indexed: true,
 internalType: "uint256",
 name: "tokenId",
 type: "uint256",
 },
 ],
 name: "Transfer",
 type: "event",
 },
 {
 inputs: [{ internalType: "address", name: "owner", type: "address" }],
 name: "balanceOf",
 outputs: [{ internalType: "uint256", name: "", type: "uint256" }],
 stateMutability: "view",
 type: "function",
 },
 ],
 },
 instances: [
 {
 abi: "nouns",
 address: "0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03",
 startBlock: 12985438,
 chain: "mainnet",
 enrich: {
 handlers: {
 "Transfer(indexed address,indexed address,indexed uint256)": {
 calls: {
 from_balance: {
 name: "balanceOf",
 params: "event.params.from",
 required: true,
 },
 to_balance: {
 name: "balanceOf",
 params: "event.params.to",
 required: true,
 },
 },
 entities: {
 FromBalance: {
 "owner address": "event.params.from.toHexString()",
 "balance uint256": "calls.from_balance",
 },
 ToBalance: {
 "owner address": "event.params.to.toHexString()",
 "balance uint256": "calls.to_balance",
 },
 },
 },
 },
 },
 },
 ],
}
```

### NOUNS enrichment with mutable current balance on transfer for both sender & receiver

This example alters our previous example balance entities to become a single mutable `Balance` entity, so that both sender and receiver use the same entity.

```json5 nouns-mutable-balance-config.json theme={null}
{
 version: "1",
 name: "nouns/1.0.0",
 abis: {
 nouns: [
 {
 anonymous: false,
 inputs: [
 {
 indexed: true,
 internalType: "address",
 name: "from",
 type: "address",
 },
 {
 indexed: true,
 internalType: "address",
 name: "to",
 type: "address",
 },
 {
 indexed: true,
 internalType: "uint256",
 name: "tokenId",
 type: "uint256",
 },
 ],
 name: "Transfer",
 type: "event",
 },
 {
 inputs: [{ internalType: "address", name: "owner", type: "address" }],
 name: "balanceOf",
 outputs: [{ internalType: "uint256", name: "", type: "uint256" }],
 stateMutability: "view",
 type: "function",
 },
 ],
 },
 instances: [
 {
 abi: "nouns",
 address: "0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03",
 startBlock: 12985438,
 chain: "mainnet",
 enrich: {
 handlers: {
 "Transfer(indexed address,indexed address,indexed uint256)": {
 calls: {
 from_balance: {
 name: "balanceOf",
 params: "event.params.from",
 required: true,
 },
 to_balance: {
 name: "balanceOf",
 params: "event.params.to",
 required: true,
 },
 },
 entities: {
 Balance: [
 {
 id: "event.params.from.toHexString()",
 explicit_id: true,
 mapping: {
 "balance uint256": "calls.from_balance",
 },
 },
 {
 id: "event.params.to.toHexString()",
 explicit_id: true,
 mapping: {
 "balance uint256": "calls.to_balance",
 },
 },
 ],
 },
 },
 },
 },
 },
 ],
}
```


 We can now query the `Balance` entity by the owner address (`id`) to see the current balance.

 ```graphql theme={null}
 {
 balance(id: "0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03") {
 id
 balance
 }
 }
 ```


### NOUNS enrichment with declared eth call

This example alters our previous example by adding the `declared` flag to boost performance of the `balanceOf` eth calls. declared calls only work for eth calls that have no mapping handler dependencies, in other words the call can be executed from the event params only. Also note that call handlers do not support delcared calls (yet), if `declared` is set on a call handler enrichment it will be ignored.

```json5 nouns-declared-calls-config.json theme={null}
{
 version: "1",
 name: "nouns/1.0.0",
 abis: {
 nouns: [
 {
 anonymous: false,
 inputs: [
 {
 indexed: true,
 internalType: "address",
 name: "from",
 type: "address",
 },
 {
 indexed: true,
 internalType: "address",
 name: "to",
 type: "address",
 },
 {
 indexed: true,
 internalType: "uint256",
 name: "tokenId",
 type: "uint256",
 },
 ],
 name: "Transfer",
 type: "event",
 },
 {
 inputs: [{ internalType: "address", name: "owner", type: "address" }],
 name: "balanceOf",
 outputs: [{ internalType: "uint256", name: "", type: "uint256" }],
 stateMutability: "view",
 type: "function",
 },
 ],
 },
 instances: [
 {
 abi: "nouns",
 address: "0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03",
 startBlock: 12985438,
 chain: "mainnet",
 enrich: {
 handlers: {
 "Transfer(indexed address,indexed address,indexed uint256)": {
 calls: {
 from_balance: {
 name: "balanceOf",
 params: "event.params.from",
 required: true,
 declared: true,
 },
 to_balance: {
 name: "balanceOf",
 params: "event.params.to",
 required: true,
 declared: true,
 },
 },
 entities: {
 Balance: [
 {
 id: "event.params.from.toHexString()",
 explicit_id: true,
 mapping: {
 "balance uint256": "calls.from_balance",
 },
 },
 {
 id: "event.params.to.toHexString()",
 explicit_id: true,
 mapping: {
 "balance uint256": "calls.to_balance",
 },
 },
 ],
 },
 },
 },
 },
 },
 ],
}
```

---

## 3. Manage endpoints with tags

**URL:** https://docs.goldsky.com/subgraphs/tags

# Manage endpoints with tags

Tags are used to maintain a consistent GraphQL endpoint. You can treat them like pointers or aliases to specific versions, allowing you to swap in new subgraphs in your app without changing your front-end code.

By default, subgraph API endpoints are named after the subgraph name and version, so if you update your subgraph to a new version, you'll need to update your front end to point to the new endpoint.

Using tags, you can manage your versions and seamlessly upgrade your subgraph version without having the URL change.

In this example, we'll assume you have already deployed a subgraph with the name and version `poap-subgraph/1.0.0`. We'll show you how to create a tag and how to move it to another subgraph version.

First, create a tag using the Goldsky CLI and associate it with your subgraph.

```shell theme={null}
goldsky subgraph tag create subgraph/1.0.0 --tag prod
```

We've now created a new tag called `prod`. Now our GraphQL endpoint will use the word `prod` instead of the version number. You should see the new GraphQL endpoint listed in your terminal after running the command.

Let's say you've upgraded your `poap-subgraph` to verison `2.0.0` and want to start querying it with your `prod` GraphQL endpoint. It's as simple as creating the tag again on the new version.

```shell theme={null}
goldsky subgraph tag create subgraph/2.0.0 --tag prod
```

Like before, you should see the GraphQL endpoint after running this command, and it should be the same as before. Now your queries will be routed to the `2.0.0` version of the subgraph seamlessly

---

## 4. Getting started

**URL:** https://docs.goldsky.com/mirror/create-a-pipeline

# Getting started

> Step by step instructions on how to create a Goldsky Mirror pipeline.


You have two options to create a Goldsky Mirror pipeline:

1. **[Goldsky Flow](/mirror/create-a-pipeline#goldsky-flow)**: With a guided web experience in the dashboard
2. **[CLI](/mirror/create-a-pipeline#creating-mirror-pipelines-with-the-cli)**: interactively or by providing a pipeline configuration

## Goldsky Flow

Flow allows you to deploy pipelines by simply dragging and dropping its component into a canvas. You can open up Flow by going to the [Pipelines page](https://app.goldsky.com/dashboard/pipelines) on the dashboard and clicking on the `New pipeline` button.


You'll be redirected to Goldsky Flow, which starts with an empty canvas representing the initial state.


The draggable components that will make up the pipeline are located on the left side menu.


Let's now look at how we can deploy a simple pipeline; in the following section we are going to see the steps needed to stream Ethereum raw logs into a ClickHouse database. Since the steps are the same for any pipeline, feel free to adapt the components to fit your specific use case.

1. **Select the Data Source**

Start by dragging and dropping a `Data Source` card onto the canvas. Once you do that, you'll to need select the chain you are interested in. We currently support [100+ chains](/chains/supported-networks). For this example we are going to choose `Ethereum`.


Next, we need to define the type of data source we want to use:

* Onchain datasets: these are [Direct Indexing datasets](/mirror/sources/direct-indexing) representing both raw data (e.g. Raw Blocks) as well as curated datasets (e.g. ERC-20 Transfers)
* [Subgraphs](/mirror/sources/subgraphs): this can be community subgraphs or existing subgraphs in your project for the choosen network

For this example, we are going to choose `Raw Logs`.


After selecting the data source, you have some optional configuration fields to use, in the case of `Onchain Datasets` you can configure:

* `Start indexing at`: here you can define whether you want to do a full backfill (`Beginning`) or read from edge (`Current`)
* `Filter by contract address`: optional contract address (in lowercase) to filter from
* `Filter by topics`: optional list of topics (in lowercase) to filter from, separated by commas.
* `View Schema`: view the data schema to get a better idea of the shape of the data as well as see some sample records.


2. **(Optional) Select a Transform**

Optionally select a Transform for your data by clicking on the `+` button at the top right edge of the `Data Source` card and you'll have the option to add a Transform or a Sink.


Tranforms are optional intermediate compute processors that allow you to modify the original data (you can find more information on the support Transform types [here](/mirror/transforms/transforms-overview)). For this example, we are going to create a simple SQL transform to select a subset of the available
data in the source. To do that, select `Custom SQL`.


Click on the `Query` field of the card to bring up the SQL editor.


In this inline editor you can define the logic of your transformation and run the
SQL code at the top right corner to experiment with the data and see the result of your queries. For this example we are adding `SELECT id, block_number, transaction_hash, data FROM source_1`

If you click on the `Run` button on the top right corner you'll see a preview of the final shape of the data. Once satisfied with the results in your Transforms, press `Save` to add it to the pipeline.

3. **Select the Sink**

The last pipeline component to define is the [Sink](/mirror/sinks/supported-sinks), this is, the destination of our data. Click on the `+` button at the top right edge of the `Transform Card` and select a Sink.


If you already have configured any sinks previously (for more information, see [Mirror Secrets](/mirror/manage-secrets)) you'll be able to
choose it from the list. Alternatively, you'll need to create a new sink by creating its corresponding secret. In our example, we’ll use an existing sink to a ClickHouse database.


Once you select the sink, you'll have some configuration options available to define how the data will be written into your database as well as anoter `Preview Output` button to see the what the final shape of the data will be; this is a very convenient utility
in cases where you might have multiple sources and transforms in your pipeline and you want to iterate on its logic without having to redeploy the actual pipeline every time.

4. **Confirm and deploy**

Last but not least, we need to define a name for the pipeline. You can do that at the top center input of your screen. For this example, we are going to call it `ethereum-raw-logs`
Up to this point, your canvas should look similar to this:


Click on the `Deploy` button on the top right corner and specify the [resource size](/mirror/about-pipeline#resource-sizing); for this example you can choose the default `Small`.


You should now be redirected to the pipelines details page


Congratulations, you just deployed your first pipeline using Goldsky Flow! 🥳

Assuming the sink is properly configured you should start seeing data flowing into your database after a few seconds.

If you would like to update the components of your pipeline and deploy a newer version (more on this topic [here](/mirror/about-pipeline))
you can click on the `Update Pipeline` button on the top right corner of the page and it will take you back onto the Flow canvas so you can do any updates on it.


There's a couple of things about Flow worth highlighting:

* Pipelines are formally defined using [configuration files](/reference/config-file/pipeline) in YAML. Goldsky Flow abstract that complexity for us so that we can just
 create the pipeline by dragging and dropping its components. You can at any time see the current configuration definition of the pipeline by switching the view to `YAML` on the top left corner. This is quite useful
 in cases where you'd like to version control your pipeline logic and/or automate its deployment via CI/CD using the CLI (as explained in next section)


* Pipeline components are interconnected via reference names: in our example, the source has a default reference name of `source_1`; the transform (`sql_1`) reads from `source_1` in its SQL query; the sink (`sink_1`)
 reads the result from the transform (see its `Input source` value) to finally emit the data into the destination. You can modify the reference names of every component of the pipeline on the canvas, just bear in mind
 the connecting role these names play.

Read on the following sections if you would like to know how to deploy pipelines using the CLI.

## Goldsky CLI


 1. Install the Goldsky CLI:

 **For macOS/Linux:**

 ```shell theme={null}
 curl https://goldsky.com | sh
 ```

 **For Windows:**

 ```shell theme={null}
 npm install -g @goldskycom/cli
 ```

 Windows users need to have Node.js and npm installed first. Download from [nodejs.org](https://nodejs.org) if not already installed.
 2. Go to your [Project Settings](https://app.goldsky.com/dashboard/settings) page and create an API key.
 3. Back in your Goldsky CLI, log into your Project by running the command `goldsky login` and paste your API key.
 4. Now that you are logged in, run `goldsky` to get started:
 ```shell theme={null}
 goldsky
 ```


There are two ways in which you can create pipelines with the CLI:

* Interactive
* Non-Interactive

### Guided CLI experience

This is a simple and guided way to create pipelines via the CLI.
Run `goldsky pipeline create ` in your terminal and follow the prompts.

In short, the CLI guides you through the following process:

1. Select one or more source(s)
2. Depending on the selected source(s), define transforms
3. Configure one or more sink(s)

### Custom Pipeline Configuration File

This is an advanced way to create a new pipeline. Instead of using the guided CLI experience (see above), you create the pipeline configuration on your own.
A pipeline configuration is a YAML structure with the following top-level properties:

```yaml theme={null}
name:
apiVersion: 3
sources: {}
transforms: {}
sinks: {}
```

Both `sources` and `sinks` are required with a minimum of one entry each. `transforms` is optional and an empty object (`{}`) can be used if no transforms are needed.

Full configuration details for Pipelines is available in the [reference](/reference/config-file/pipeline) page.

As an example, see below a pipeline configuration which uses the Ethereum Decoded Logs dataset as source, uses a transform to select specific data fields and sinks that data into a Postgres database whose connection details are stored within the `A_POSTGRESQL_SECRET` secret:



 ```yaml pipeline.yaml theme={null}
 name: ethereum-decoded-logs
 apiVersion: 3
 sources:
 ethereum_decoded_logs:
 dataset_name: ethereum.decoded_logs
 version: 1.0.0
 type: dataset
 start_at: latest

 transforms:
 select_relevant_fields:
 sql: |
 SELECT
 id,
 address,
 event_signature,
 event_params,
 raw_log.block_number as block_number,
 raw_log.block_hash as block_hash,
 raw_log.transaction_hash as transaction_hash
 FROM
 ethereum_decoded_logs
 primary_key: id

 sinks:
 postgres:
 type: postgres
 table: eth_logs
 schema: goldsky
 secret_name: A_POSTGRESQL_SECRET
 from: select_relevant_fields
 ```



Note that to create a pipeline from configuration that sinks to your datastore, you need to have a [secret](/mirror/manage-secrets) already configured on your Goldsky project and reference it in the sink configuration.

Run `goldsky pipeline apply ` in your terminal to create a pipeline.

Once your pipeline is created, run `goldsky pipeline start ` to start your pipeline.

## Monitor a pipeline

When you create a new pipeline, the CLI automatically starts to monitor the status and outputs it in a table format.

If you want to monitor an existing pipeline at a later time, use the `goldsky pipeline monitor ` CLI command. It refreshes every ten seconds and gives you insights into how your pipeline performs.

Or you may monitor in the Pipeline Dashboard page at `https://app.goldsky.com/dashboard/pipelines/stream/

/` where you can see the pipeline's `status`, `logs`, `metrics`.

---

## 5. Direct indexing

**URL:** https://docs.goldsky.com/mirror/sources/direct-indexing

# Direct indexing

With mirror pipelines, you can access to indexed on-chain data. Define them as a source and pipe them into any sink we support.

## Use-cases

* Mirror specific logs and traces from a set of contracts into a postgres database to build an API for your protocol
* ETL data into a data warehouse to run analytics
* Push the full blockchain into Kafka or S3 to build a datalake for ML

## Supported Chains

### EVM chains

For EVM chains we support the following 4 datasets:

| Dataset | Description |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| Blocks | Metadata for each block on the chain including hashes, transaction count, difficulty, and gas used. |
| Logs | Raw logs for events emitted from contracts. Contains the contract address, data, topics, and metadata for blocks and transactions. |
| Enriched Transactions | Transaction data including input, value, from and to address, and metadata for the block, gas, and receipts. |
| Traces | Traces of all function calls made on the chain including metadata for block, trace, transaction, and gas. |

### Fast Scan

Some datasets have support for [Fast Scan](/mirror/sources/direct-indexing#backfill-vs-fast-scan) which allows you to more quickly backfill filtered data. If a chain has partial support for Fast Scan, the dataset that doesn't support fast scan will have an asterisk `*` next to it.

Here's a breakdown of the EVM chains we support and their corresponding datasets:

| | Blocks | Enriched Transactions | Logs | Traces | Fast Scan |
| -------------------- | ------ | --------------------- | ---- | ------ | --------- |
| 0G | ✓ | ✓ | ✓ | ✗ | ✗ |
| 0G Galileo Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Abstract | ✓ | ✓ | ✓ | ✗ | ✗ |
| Align Testnet | ✓ | ✓ | ✓ | ✓ | ✓ |
| Apechain | ✓ | ✓ | ✓ | ✗ | ✗ |
| Apechain Curtis | ✓ | ✓ | ✓ | ✓\* | ✓ |
| Proof of Play Apex | ✓ | ✓ | ✓ | ✗ | ✓ |
| Arbitrum Nova | ✓ | ✓ | ✓ | ✗ | ✓ |
| Arbitrum One | ✓ | ✓ | ✓ | ✓\* | ✓ |
| Arbitrum Sepolia | ✓ | ✓ | ✓ | ✗ | ✓ |
| Arena-Z | ✓ | ✓ | ✓ | ✓ | ✗ |
| Arena-Z Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Arweave \* | ✓ | ✓ | N/A | N/A | ✗ |
| Automata | ✓ | ✓ | ✓ | ✓ | ✗ |
| Automata Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Avalanche | ✓ | ✓ | ✓ | ✓ | ✓ |
| B3 | ✓ | ✓ | ✓ | ✓ | ✗ |
| B3 Sepolia | ✓ | ✓ | ✓ | ✓ | ✗ |
| Base | ✓ | ✓ | ✓ | ✓ | ✓ |
| Base Sepolia | ✓ | ✓ | ✓ | ✓ | ✓ |
| Berachain Bepolia | ✓ | ✓ | ✓ | ✓ | ✗ |
| Berachain Mainnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Bitcoin | ✓ | ✓ | ✗ | ✗ | ✗ |
| Blast | ✓ | ✓ | ✓ | ✓ | ✓ |
| Build on Bitcoin | ✓ | ✓ | ✓ | ✓ | ✓ |
| Binance Smart Chain | ✓ | ✓ | ✓ | ✓ | ✓ |
| Camp Testnet | ✓ | ✓ | ✓ | ✓ | ✓ |
| Celo | ✓\* | ✓\* | ✓ | ✓ | ✓ |
| Celo Dango Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Codex | ✓ | ✓ | ✓ | ✓ | ✗ |
| Corn Maizenet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Cronos zkEVM | ✓ | ✓ | ✓ | ✗ | ✗ |
| Cronos zkEVM Sepolia | ✓ | ✓ | ✓ | ✗ | ✗ |
| Cyber | ✓ | ✓ | ✓ | ✓ | ✓ |
| Cyber Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Degen | ✓ | ✓ | ✓ | ✓ | ✗ |
| Ethena Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Ethereum | ✓ | ✓ | ✓ | ✓ | ✓ |
| Ethereum Holesky | ✓ | ✓ | ✓ | ✓ | ✓ |
| Ethereum Sepolia | ✓ | ✓ | ✓ | ✓ | ✓ |
| Etherlink | ✓ | ✓ | ✓ | ✓ | ✗ |
| Etherlink Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Ethernity | ✓ | ✓ | ✓ | ✓ | ✗ |
| Ethernity Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Fantom | ✓ | ✓ | ✓ | ✓ | ✓ |
| Flare | ✓ | ✓ | ✓ | ✗ | ✗ |
| Flare Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Fluent Devnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Forma | ✓ | ✓ | ✓ | ✓ | ✓ |
| Frax | ✓ | ✓ | ✓ | ✓ | ✓ |
| Gnosis | ✓ | ✓ | ✓ | ✓ | ✗ |
| Gravity | ✓ | ✓ | ✓ | ✓ | ✗ |
| Ham | ✓ | ✓ | ✓ | ✓ | ✗ |
| HashKey | ✓ | ✓ | ✓ | ✗ | ✗ |
| HyperEVM | ✓ | ✓ | ✓ | ✗ | ✗ |
| Immutable Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Immutable zkEVM | ✓ | ✓ | ✓ | ✗ | ✗ |
| Ink | ✓ | ✓ | ✓ | ✓ | ✗ |
| Ink Sepolia | ✓ | ✓ | ✓ | ✓ | ✗ |
| IOTA EVM | ✓ | ✓ | ✓ | ✓ | ✗ |
| Kroma | ✓ | ✓ | ✓ | ✓ | ✗ |
| Linea | ✓ | ✓ | ✓ | ✓ | ✗ |
| Lisk | ✓ | ✓ | ✓ | ✓ | ✗ |
| Lisk Sepolia | ✓ | ✓ | ✓ | ✓ | ✗ |
| Lith Testnet | ✓ | ✓ | ✓ | ✓ | ✓ |
| Lyra | ✓ | ✓ | ✓ | ✓ | ✗ |
| Lyra Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| MegaETH Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Metal | ✓ | ✓ | ✓ | ✓ | ✗ |
| Metal Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Mezo | ✓ | ✓ | ✓ | ✓ | ✗ |
| Mezo Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Midnight Devnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Mint | ✓ | ✓ | ✓ | ✓ | ✓ |
| Mint Sepolia | ✓ | ✓ | ✓ | ✓ | ✗ |
| Mode | ✓ | ✓ | ✓ | ✓ | ✓ |
| Mode Testnet | ✓ | ✓ | ✓ | ✓ | ✓ |
| Monad Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Morph | ✓ | ✓ | ✓ | ✗ | ✗ |
| Neura Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Oasys Homeverse | ✓ | ✓ | ✓ | ✓\* | ✓ |
| Optimism | ✓ | ✓ | ✓ | ✓ | ✓ |
| Optimism Sepolia | ✓ | ✓ | ✓ | ✓ | ✓ |
| Orderly | ✓ | ✓ | ✓ | ✓ | ✗ |
| Orderly Sepolia | ✓ | ✓ | ✓ | ✓ | ✗ |
| Palm | ✓ | ✓ | ✓ | ✓ | ✓ |
| Palm Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Plasma | ✓ | ✓ | ✓ | ✓ | ✗ |
| Plasma Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Plume | ✓ | ✓ | ✓ | ✗ | ✗ |
| Pharos Devnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Pharos Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Polygon | ✓ | ✓ | ✓ | ✓ | ✗ |
| Polynomial | ✓ | ✓ | ✓ | ✓ | ✗ |
| Proof of Play Barret | ✓ | ✓ | ✓ | ✓ | ✗ |
| Proof of Play Boss | ✓ | ✓ | ✓ | ✓ | ✗ |
| Proof of Play Cloud | ✓ | ✓ | ✓ | ✓ | ✗ |
| Public Good Network | ✓ | ✓ | ✓ | ✓ | ✓ |
| Race | ✓ | ✓ | ✓ | ✓ | ✗ |
| Rari | ✓ | ✓ | ✓ | ✓ | ✓ |
| Redstone | ✓ | ✓ | ✓ | ✓ | ✓ |
| Reya | ✓ | ✓ | ✓ | ✗ | ✓ |
| Rise Sepolia | ✓ | ✓ | ✓ | ✓ | ✗ |
| Ruby Testnet | ✓ | ✓ | ✓ | ✗ | ✓ |
| Scroll | ✓ | ✓ | ✓ | ✓ | ✓ |
| Scroll Sepolia | ✓ | ✓ | ✓ | ✓ | ✗ |
| Sei | ✓ | ✓ | ✓ | ✓ | ✗ |
| Settlus | ✓ | ✓ | ✓ | ✓ | ✗ |
| Shape | ✓ | ✓ | ✓ | ✓ | ✓ |
| Shape Sepolia | ✓ | ✓ | ✓ | ✓ | ✗ |
| Shrapnel | ✓ | ✓ | ✓ | ✗ | ✓ |
| SNAXchain | ✓ | ✓ | ✓ | ✓ | ✗ |
| Soneium | ✓ | ✓ | ✓ | ✗ | ✗ |
| Soneium Minato | ✓ | ✓ | ✓ | ✗ | ✗ |
| Sonic | ✓ | ✓ | ✓ | ✗ | ✗ |
| Sophon | ✓ | ✓ | ✓ | ✓ | ✗ |
| Sophon Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Story | ✓ | ✓ | ✓ | ✓ | ✗ |
| Story Aeneid Testnet | ✓ | ✓ | ✓ | ✓ | ✗ |
| Superseed | ✓ | ✓ | ✓ | ✓ | ✗ |
| Superseed Sepolia | ✓ | ✓ | ✓ | ✓ | ✓ |
| Swan | ✓ | ✓ | ✓ | ✓ | ✗ |
| Swellchain | ✓ | ✓ | ✓ | ✗ | ✗ |
| Swellchain Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| TAC | ✓ | ✓ | ✓ | ✗ | ✗ |
| TAC Turin Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| Taiko Hoodi Testnet | ✓ | ✓ | ✓ | ✗ | ✗ |
| TRON | ✓ | ✓ | ✓ | ✗ | ✗ |
| Unichain | ✓ | ✓ | ✓ | ✓ | ✗ |
| Unichain Sepolia | ✓ | ✓ | ✓ | ✗ | ✗ |
| Viction | ✓ | ✓ | ✓ | ✗ | ✗ |
| World Chain | ✓ | ✓ | ✓ | ✗ | ✗ |
| XPLA | ✓ | ✓ | ✓ | ✗ | ✗ |
| XR Sepolia | ✓ | ✓ | ✓ | ✗ | ✗ |
| Xterio | ✓ | ✓ | ✓ | ✓ | ✗ |
| Zero | ✓ | ✓ | ✓ | ✓ | ✗ |
| Zero Sepolia | ✓ | ✓ | ✓ | ✗ | ✗ |
| Zetachain | ✓ | ✓ | ✓ | ✓ | ✓ |
| zkSync Era | ✓ | ✓ | ✓ | ✓ | ✓ |
| Zora | ✓ | ✓ | ✓ | ✓ | ✓ |
| Zora Sepolia | ✓ | ✓ | ✓ | ✓ | ✗ |

\* The Arweave dataset includes bundled/L2 data.

### Non-EVM chains

#### Beacon

| Dataset | Description |
| ------------------------------------------ | ----------------------------------------------------------------------------------- |
| Attestations | Attestations (votes) from validators for the block. |
| Attester Slashing | Metadata for attester slashing. |
| Blocks | Metadata for each block on the chain including hashes, deposit count, and gas used. |
| BLS Signature to Execution Address Changes | BLS Signature to Execution Address Changes. |
| Deposits | Metadata for deposits. |
| Proposer Slashing | Metadata for proposer slashing. |
| Voluntary Exits | Metadata for voluntary exits. |
| Withdrawls | Metadata for withdrawls. |

#### Fogo

| Dataset | Description |
| ------------------------------ | -------------------------------------------------------------------------------------------------------- |
| Transactions with Instructions | Enriched transaction data including instructions, accounts, balance changes, and metadata for the block. |
| Rewards | Records of rewards distributed to validators for securing and validating the network. |
| Blocks | Metadata for each block on the chain including hashes, transaction count, slot and leader rewards. |

#### IOTA

| Dataset | Description |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Checkpoints | A checkpoint is a periodic, finalized snapshot of the blockchain's state in the Movement VM, batching transactions to ensure consistency and scalability across the network. |
| Epochs | An epoch is a defined time period in the Movement VM during which a fixed set of validators processes transactions and manages governance, with transitions enabling validator rotation and network updates. |
| Events | Events in the Movement VM are structured data emissions from smart contracts, recorded on the blockchain to log significant actions or state changes for external monitoring and interaction. |
| Move Calls | Move calls are a function invocation within a Move smart contract, executed by the Movement VM to perform specific operations or state transitions on the blockchain. |
| Transactions | A transaction in the Movement VM is a signed instruction executed by the Move smart contract to modify the blockchain's state, such as transferring assets or invoking contract functions. |

#### Movement

| Dataset | Description |
| --------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Account Transactions | All raw onchain transactions involving account-level actions (e.g., transaction version, account address). |
| Block Metadata Transactions | Metadata about blocks and block-level transactions (e.g., block height, epoch, version). |
| Fungible Asset Balances | Real-time balances of fungible tokens across accounts. |
| Current Token Data | Latest metadata for tokens - includes name, description, supply, etc. |
| Current Token Ownerships | Snapshot of token ownership across the chain. |
| Events | All emitted contract event logs - useful for indexing arbitrary contract behavior. |
| Fungible Asset Activities | Track activity for fungible tokens - owner address, amount, and type. |
| Fungible Asset Balances | Historical balance tracking for fungible assets (not just the current state). |
| Fungible Asset Metadata | Static metadata for fungible tokens - like decimals, symbol, and name. |
| Signatures | Cryptographic signature data from transactions, useful for validating sender authenticity. |
| Token Activities | Detailed logs of token movements and interactions across tokens and NFTs. |

#### Solana

| Dataset | Description |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Edge Accounts | Contains details of all active accounts on the Solana blockchain, including balance and owner information. Live data from slot 271611201. |
| Edge Blocks | Metadata for each block on the chain including hashes, transaction count, difficulty, and gas used. Live data from slot 271611201. |
| Edge Instructions | Specific operations within transactions that describe the actions to be performed on the Solana blockchain. Live data from slot 271611201. |
| Edge Rewards | Records of rewards distributed to validators for securing and validating the Solana network. Live data from slot 271611201. |
| Edge Token Transfers | Transactions involving the movement of tokens between accounts on the Solana blockchain. Live data from slot 271611201. |
| Edge Tokens | Information about different token types issued on the Solana blockchain, including metadata and supply details. Live data from slot 271611201. |
| Edge Transactions | Enriched transaction data including input, value, from and to address, and metadata for the block, gas and receipt. Live data from slot 271611201. |
| Edge Transactions with Instructions | Enriched transaction data including instructions, input, value, from and to address, and metadata for the block, gas and receipt. Live data from slot 316536533. |


 You can interact with these Solana datasets at no cost at
 [https://crypto.clickhouse.com/](https://crypto.clickhouse.com/)


#### Starknet

| Dataset | Description |
| ------------ | ------------------------------------------------------------------------------------------------------------ |
| Blocks | Metadata for each block on the chain including hashes, transaction count, difficulty, and gas used. |
| Events | Consists of raw event data from the blockchain, documenting various on-chain activities and triggers. |
| Messages | Messaging data from the Starknet blockchain, used for L2 & L1 communication. |
| Transactions | Transaction data including input, value, from and to address, and metadata for the block, gas, and receipts. |

#### Stellar

| Dataset | Description |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Assets | Contains information about all assets issued on the Stellar network, including details like asset codes, issuers, and related metadata. |
| Contract Events | Records events related to smart contract execution on the Stellar network, detailing the interactions and state changes within contracts. |
| Effects | Captures the effects of various operations on the Stellar ledger, such as changes in balances, creation of accounts, and other state modifications. |
| Ledgers | Provides a comprehensive record of all ledger entries, summarizing the state of the blockchain at each ledger close, including transaction sets and ledger headers. |

#### Sui

| Dataset | Description |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| Checkpoints | Contains raw data of blockchain checkpoints capturing the state of the ledger at specific intervals. |
| Epochs | Includes raw data detailing the various epochs in the blockchain, which mark significant periods or phases in the network's operation |
| Events | Consists of raw event data from the blockchain, documenting various on-chain activities and triggers |
| Packages | Contains raw data about the deployed smart contract packages on the blockchain |
| Transactions | Transaction data including effects, events, senders, recipients, balance and object changes, and other metadata. |

### Curated Datasets

Beyond onchain datasets, the Goldsky team continuosly curates and publishes derived datasets that serve a specific audience or use case. Here's the list:

#### Token Transfers

You can expect every EVM chain to have the following datasets available:

| Dataset | Description |
| --------- | ------------------------------------------------- |
| ERC\_20 | Every transfer event for all fungible tokens. |
| ERC\_721 | Every transfer event for all non-fungible tokens. |
| ERC\_1155 | Every transfer event for all ERC-1155 tokens. |

#### Polymarket datasets

| Dataset | Description |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Global Open Interest | Keeps track of global open interest. |
| Market Open Interest | Keeps track of open interest for each market. |
| Order Filled | This event is emitted when a single Polymarket order is partially or completely filled. For example: a 50c YES buy for 100 YES matched against a 50c YES sell for 100 YES will emit 2 Orderi Filled events, from the perspective of the YES buy and of the YES sell. This is useful for granular tracking of trading activity and history. |
| Orders Matched | This event is emitted when a Polymarket taker order is matched against a set of Polymarket maker(limit) orders. For example: a 50c YES buy for 200 YES matched against 2 50c YES sells for 100 YES each will emit a single Orders Matched event. Orders Matched gives a more high level view of trading activity as it only tracks taker activity. |
| User Balances | This event keeps track of all user outcome token positions. |
| User Positions | Keeps track of outcome token positions along with pnl specific data including average price and realized pnl. |

Additional chains, including roll-ups, can be indexed on demand. Contact us at [sales@goldsky.com](mailto:sales@goldsky.com) to learn more.

## Schema

The schema for each of these datasets can be found [here](/reference/schema/EVM-schemas).

## Backfill vs Fast Scan

Goldsky allows you either backfill the entire datasets or alternatively pre-filter the data based on specific attributes.
This allows for an optimal cost and time efficient streaming experience based on your specific use case.

For more information on how to enable each streaming mode in your pipelines visit our [reference documentation](/reference/config-file/pipeline#backfill-vs-fast-scan).

---

## 6. Overview

**URL:** https://docs.goldsky.com/mirror/extensions/channels/overview

# Overview

> Use channels to integrate Mirror into your existing data stack.

## What are channels?

Channels are a special type of [Sink](/mirror/sinks/supported-sinks) that represent intermediate storage layers designed to absorb the Goldsky
firehose. They aren't designed to be queryable on their own - instead, you
should plan to connect them to your existing data stack or sink that's not
currently supported by Goldsky.



 AWS S3 offers unparalleled scalability and durability for storing vast
 amounts of data.



 AWS SQS provides reliable message queuing for decoupling distributed systems
 with ease.



 Kafka excels in handling high-throughput, real-time data streams with strong
 fault tolerance.



## What should I use?

### For durable storage

Goldsky supports export of raw blockchain or custom data to [AWS S3](/mirror/extensions/channels/aws-s3) or GCS, either in Iceberg format or in plain Parquet format. GCS support is currently available upon request, please reach out to us at [support@goldsky.com](mailto:support@goldsky.com).

Keep in mind that the blockchain is eventually consistent, so reorgs in append-only mode is portrayed differently.

Once data is in S3, you can use a solution like AWS Athena to query, or merge with existing spark pipelines.

### For processing events as they come

If your backend needs to process and receive data as it appears on the blockchain, you can consider using our SQS or Kafka channel sinks. These sinks are append-only, so chain reorgs are not handled for you, but you do receive all the metadata required to handle reorgs yourself.

An example architecture would have:

1. An SQS queue
2. An AWS Lambda function processing said queue

Mirror will send each event as a message into the SQS queue, and the lambda function can process it however you need through making additional enrichments, calling discord/telegram bots, or inserting into another datastore that we don’t yet support.

### For more processing

In addition to AWS S3, we support direct emits to [Kafka](/mirror/extensions/channels/kafka). Kafka can store messages at scale, making it a great choice as an initial holding place for data before you do further processing.

Our team can work with many different strategies and can give guidance on how to integrate with our data format inside Kafka. Reach out to our support team at [support@goldsky.com](mailto:support@goldsky.com) if you'd like to learn more.

---

## 7. Mirror - Supported sinks

**URL:** https://docs.goldsky.com/mirror/sinks/supported-sinks

# Mirror - Supported sinks

Sinks define the destination of your data. We support two broad categories of sinks based on their functionality and applicability:

* Standard Sinks: These sinks are destinations readily available for querying and analysis, such as traditional databases.
* Channel Sinks: These sinks serve as intermediate storage layers, facilitating further integration into your data stack. Examples: Kafka, AWS S3, or AWS SQS.

## Standard Sinks

Standard Sinks are the default and most popular type of sinks for Mirror. They are optimized for immediate querying and analysis, providing a seamless experience for real-time data access and operations. These sinks are:



 Postgres stands out with its advanced features, extensibility, and strong
 ACID compliance.



 Hosted Postgres managed by Goldsky via NeonDB. Store data securely, scale infinitely and export your data if you need it.



 MySQL stands out with its advanced features, extensibility, and strong
 ACID compliance.



 ClickHouse delivers exceptional performance for OLAP queries with its
 columnar storage format.



 Elasticsearch is a powerful tool for real-time search and analytics on large
 datasets.



 Timescale offers powerful time-series data management and analytics with
 PostgreSQL compatibility.



 Goldsky Channels are storage layers designed to absorb the Goldsky firehose and let you stream data into alternative sinks. These channels are AWS S3, AWS SQS and Kafka.



 A Webhook sink enables sending data to an external service via HTTP. This allows you to output pipeline results to your application server, to a third-party API, or a bot.



### What should I use?

#### For APIs for apps

For sub-second queries, typically you would choose a database that has row-based storage (i.e. it stores each row as it is instead of applying any sort of compression).

The drawbacks are that they take more space. This means large, non-indexed scans can take longer and storage costs can be higher.

1. [Postgres](/mirror/sinks/postgres) is the gold standard for application databases. It can scale almost infinitely with some management (You can use a Goldsky hosted version so you don't have to worrry about scaling), and can support very fast point-lookups with proper indexing.

 If you require super fast lookups by `transaction_hash` or a specific column, Postgres is a very safe choice to start with. It’s great as a backend for live data APIs.

 However, it can be slow for analytics queries with a lot of aggregations. For that, you may want to look for an analytical database.

 Great hosted solutions for Postgres include [NeonDB](https://neon.tech/), [AWS Aurora](https://aws.amazon.com/rds/aurora/), and [GCP CloudSQL](https://cloud.google.com/sql).
2. [Elasticsearch](/mirror/sinks/elasticsearch) is a no-sql database that allows for blazing fast lookups and searches. Elasticsearch is built around super-fast non-indexed scanning, meaning it can look at every single record to find the one you want. As a result, you can do queries like fuzzy matches and wildcard lookups with millisecond latency.

 Common applications include search on multiple columns, ‘instant’ auto-complete, and more.

#### For Analytics

1. [ClickHouse](/mirror/sinks/clickhouse) is a very efficient choice for storage. You can store the entire Ethereum blockchain and pay around \$50 in storage.

 We recommend considering ClickHouse as an alternative to Snowflake or BigQuery - it supports many of the same use cases, and has additional features such as materialized views. We’ve seen our customers save tens of thousands of dollars using Goldsky and ClickHouse as a solution.

 The pricing for managing ClickHouse is based on storage cost, then compute cost. The compute cost is constant and isn’t based on the amount of data scanned, so you can run concurrent queries without increasing cost.

## Channel Sinks

Channel Sinks act as an extension of the default sinks, providing intermediate storage for more complex data integration scenarios. They are designed to handle high-throughput data streams and enable further processing within your data stack. Examples include:

* AWS S3: A scalable object storage service.
* AWS SQS: A fully managed message queue for microservices, distributed systems, and serverless applications.
* Kafka: A distributed event streaming platform.

For more information on Channel Sinks and how to integrate them, visit our [Channels documentation](/mirror/extensions/channels/overview).

---

## 8. Overview

**URL:** https://docs.goldsky.com/mirror/transforms/transforms-overview

# Overview

> Learn about Mirror's powerful transformation capabilities.

While the simple pipelines let you get real-time data from one of our data sets into your own destination, most teams also do enrichment and filtering using transforms.

With transforms, you can decode check external API is call contracts storage and more. You can even call your own APIs in order to tie the pipeline into your existing system seamlessly.

## [SQL Transforms](/mirror/transforms/sql-transforms)

SQL transforms allow you to write SQL queries to modify and shape data from multiple sources within the pipeline. This is ideal for operations that need to be performed within the data pipeline itself, such as filtering, aggregating, or joining datasets.

Depending on how you choose to [source](/mirror/sources/supported-sources) your data, you might find that you run into 1 of 2 challenges:

1. **You only care about a few contracts**

 Rather than fill up your database with a ton of extra data, you'd rather ***filter*** down your data to a smaller set.
2. **The data is still a bit raw**

 Maybe you'd rather track gwei rounded to the nearest whole number instead of wei. You're looking to ***map*** data to a different format so you don't have to run this calculation over and over again.

## [External Handler Transforms](/mirror/transforms/external-handlers)

With external handler transforms, you can send data from your Mirror pipeline to an external service via HTTP and return the processed results back into the pipeline. This opens up a world of possibilities by allowing you to bring your own custom logic, programming languages, and external services into the transformation process.

Key Features of External Handler Transforms:

* Send data to external services via HTTP.
* Supports a wide variety of programming languages and external libraries.
* Handle complex processing outside the pipeline and return results in real time.
* Guaranteed at least once delivery and back-pressure control to ensure data integrity.

### How External Handlers work

1. The pipeline sends a POST request to the external handler with a mini-batch of JSON rows.
2. The external handler processes the data and returns the transformed rows in the same format and order as received

---

## 9. About Mirror pipelines

**URL:** https://docs.goldsky.com/mirror/about-pipeline

# About Mirror pipelines


 We recently released v3 of pipeline configurations which uses a more intuitive
 and user friendly format to define and configure pipelines using a yaml file.
 For backward compatibility purposes, we will still support the previous v2
 format. This is why you will find references to each format in each yaml file
 presented across the documentation. Feel free to use whichever is more
 comfortable for you but we encourage you to start migrating to v3 format.


## Overview

A Mirror Pipeline defines flow of data from `sources -> transforms -> sinks`. It is configured in a `yaml` file which adheres to Goldsky's pipeline schema.

The core logic of the pipeline is defined in `sources`, `transforms` and `sinks` attributes.

* `sources` represent origin of the data into the pipeline.
* `transforms` represent data transformation/filter logic to be applied to either a source and/or transform in the pipeline.
* `sinks` represent destination for the source and/or transform data out of the pipeline.

Each `source` and `transform` has a unique name which is referenceable in other `transform` and/or `sink`, determining dataflow within the pipeline.

While the pipeline is configured in yaml, [goldsky pipeline CLI commands](/reference/cli#pipeline) are used to take actions on the pipeline such as: `start`, `stop`, `get`, `delete`, `monitor` etc.

Below is an example pipeline configuration which sources from `base.logs` Goldsky dataset, filters the data using `sql` and sinks to a `postgresql` table:



 ```yaml base-logs.yaml theme={null}
 apiVersion: 3
 name: base-logs-pipeline
 resource_size: s
 sources:
 base.logs:
 dataset_name: base.logs
 version: 1.0.0
 type: dataset
 description: Enriched logs for events emitted from contracts. Contains the contract address, data, topics, decoded event and metadata for blocks and transactions.
 display_name: Logs
 transforms:
 filter_logs_by_block_number:
 sql: SELECT * FROM base.logs WHERE block_number > 5000
 primary_key: id
 sinks:
 postgres_base_logs:
 type: postgres
 table: base_logs
 schema: public
 secret_name: GOLDSKY_SECRET
 description: "Postgres sink for: base.logs"
 from: filter_logs_by_block_number
 ```


 Keys in v3 format for sources, transforms and sinks are user provided
 values. In the above example, the source reference name `base.logs`
 matches the actual dataset name. This is the convention that you'll
 typically see across examples and autogenerated configurations. However,
 you can use a custom name as the key.




 ```yaml base-logs.yaml theme={null}
 name: base-logs-pipeline
 resource_size: s
 apiVersion: 3
 definition:
 sources:
 - referenceName: base.logs
 type: dataset
 version: 1.0.0
 transforms: []
 sinks:
 - type: postgres
 table: base_logs
 schema: public
 secretName: GOLDSKY_SECRET
 description: 'Postgres sink for: base.logs'
 sourceStreamName: base.logs
 referenceName: postgres_base_logs
 ```



You can find the complete Pipeline configuration schema in the [reference](/reference/config-file/pipeline) page.

## Development workflow

Similar to the software development workflow of `edit -> compile -> run`, there's an implict iterative workflow of `configure -> apply -> monitor` for developing pipelines.

1. `configure`: Create/edit the configuration yaml file.
2. `apply`: Apply the configuration aka run the pipeline.
3. `monitor`: Monitor how the pipeline behaves. This will help create insights that'll generate ideas for the first step.

Eventually, you'll end up with a configuration that works for your use case.

Creating a Pipeline configuration from scratch is challenging. However, there are tools/guides/examples that make it easier to [get started](/mirror/create-a-pipeline).

## Understanding Pipeline Runtime Lifecycle

The `status` attribute represents the desired status of the pipeline and is provided by the user. Applicable values are:

* `ACTIVE` means the user wants to start the pipeline.
* `INACTIVE` means the user wants to stop the pipeline.
* `PAUSED` means the user wants to save-progress made by the pipeline so far and stop it.

A pipeline with status `ACTIVE` has a runtime status as well. Runtime represents the execution of the pipeline. Applicable runtime status values are:

* `STARTING` means the pipeline is being setup.
* `RUNNING` means the pipeline has been setup and is processing records.
* `FAILING` means the pipeline has encountered errors that prevents it from running successfully.
* `TERMINATED` means the pipeline has failed and the execution has been terminated.

There are several [goldsky pipeline CLI commands](/reference/config-file/pipeline#pipeline-runtime-commands) that help with pipeline execution.

For now, let's see how these states play out on successful and unsuccessful scenarios.

### Successful pipeline lifecycle

In this scenario the pipeline is succesfully setup and processing data without encountering any issues.
We consider the pipeline to be in a healthy state which translates into the following statuses:

* Desired `status` in the pipeline configuration is `ACTIVE`
* Runtime Status goes from `STARTING` to `RUNNING`


 ```mermaid theme={null}
 stateDiagram-v2
 state ACTIVE {
 [*] --> STARTING
 STARTING --> RUNNING
 }
 ```


Let's look at a simple example below where we configure a pipeline that consumes Logs from Base chain and streams them into a Postgres database:



 ```yaml base-logs.yaml theme={null}
 name: base-logs-pipeline
 resource_size: s
 apiVersion: 3
 sources:
 base.logs:
 dataset_name: base.logs
 version: 1.0.0
 type: dataset
 description: Enriched logs for events emitted from contracts. Contains the contract address, data, topics, decoded event and metadata for blocks and transactions.
 display_name: Logs
 transforms: {}
 sinks:
 postgres_base_logs:
 type: postgres
 table: base_logs
 schema: public
 secret_name: GOLDSKY_SECRET
 description: "Postgres sink for: base.logs"
 from: base.logs
 ```



 ```yaml base-logs.yaml theme={null}
 name: base-logs-pipeline
 definition:
 sources:
 - referenceName: base.logs
 type: dataset
 version: 1.0.0
 transforms: []
 sinks:
 - type: postgres
 table: base_logs
 schema: public
 secretName: GOLDSKY_SECRET
 description: 'Postgres sink for: base.logs'
 sourceStreamName: base.logs
 referenceName: postgres_base_logs
 ```



Let's attempt to run it using the command `goldsky pipeline apply base-logs.yaml --status ACTIVE` or `goldsky pipeline start base-logs.yaml`

```
❯ goldsky pipeline apply base-logs.yaml --status ACTIVE
│
◇ Successfully validated config file
│
◇ Successfully applied config to pipeline: base-logs-pipeline

To monitor the status of your pipeline:

Using the CLI: `goldsky pipeline monitor base-logs-pipeline`
Using the dashboard: https://app.goldsky.com/dashboard/pipelines/stream/base-logs-pipeline/1
```

At this point we have set the desired status to `ACTIVE`. We can confirm this using `goldsky pipeline list`:

```
❯ goldsky pipeline list
✔ Listing pipelines
────────────────────────────────────────
│ Name │ Version │ Status │ Resource │
│ │ │ │ Size │
│───────────────────────────────────────
│ base-logs-pipeline │ 1 │ ACTIVE │ s │
────────────────────────────────────────

```

We can then check the runtime status of this pipeline using the `goldsky pipeline monitor base-logs-pipeline` command:


We can see how the pipeline starts in `STARTING` status and becomes `RUNNING` as it starts processing data successfully into our Postgres sink.
This pipeline will start processing the historical data of the source dataset, reach its edge and continue streaming data in real time until we either stop it or it encounters error(s) that interrupts it's execution.

### Unsuccessful pipeline lifecycle

Let's now consider the scenario where the pipeline encounters errors during its lifetime and ends up failing.

There can be multitude of reasons for a pipeline to encounter errors such as:

* secrets not being correctly configured
* sink availability issues
* policy rules on the sink preventing the pipeline from writing records
* resource size incompatiblity
* and many more

These failure scenarios prevents a pipeline from getting-into or staying-in a `RUNNING` runtime status.


 ```mermaid theme={null}
 ---
 title: Healthy pipeline becomes unhealthy
 ---
 stateDiagram-v2
 state status:ACTIVE {
 [*] --> STARTING
 STARTING --> RUNNING
 RUNNING --> FAILING
 FAILING --> TERMINATED
 }
 ```

 ```mermaid theme={null}
 ---
 title: Pipeline cannot start
 ---
 stateDiagram-v2
 state status:ACTIVE {
 [*] --> STARTING
 STARTING --> FAILING
 FAILING --> TERMINATED
 }
 ```


A Pipeline can be in an `ACTIVE` desired status but a `TERMINATED` runtime status in scenarios that lead to terminal failure.

Let's see an example where we'll use the same configuration as above but set a `secret_name` that does not exist.



 ```yaml bad-base-logs.yaml theme={null}
 name: bad-base-logs-pipeline
 resource_size: s
 apiVersion: 3
 sources:
 base.logs:
 dataset_name: base.logs
 version: 1.0.0
 type: dataset
 description: Enriched logs for events emitted from contracts. Contains the contract address, data, topics, decoded event and metadata for blocks and transactions.
 display_name: Logs
 transforms: {}
 sinks:
 postgres_base_logs:
 type: postgres
 table: base_logs
 schema: public
 secret_name: YOUR_DATABASE_SECRET
 description: "Postgres sink for: base.logs"
 from: base.logs
 ```



 ```yaml bad-base-logs.yaml theme={null}
 name: bad-base-logs-pipeline
 definition:
 sources:
 - referenceName: base.logs
 type: dataset
 version: 1.0.0
 transforms: []
 sinks:
 - type: postgres
 table: base_logs
 schema: public
 secretName: YOUR_DATABASE_SECRET
 description: 'Postgres sink for: base.logs'
 sourceStreamName: base.logs
 referenceName: postgres_base_logs
 ```



Let's start it using the command `goldsky pipeline apply bad-base-logs.yaml`.

```
❯ goldsky pipeline apply bad-base-logs.yaml
│
◇ Successfully validated config file
│
◇ Successfully applied config to pipeline: base-logs-pipeline

To monitor the status of your pipeline:

Using the CLI: `goldsky pipeline monitor bad-base-logs-pipeline`
Using the dashboard: https://app.goldsky.com/dashboard/pipelines/stream/bad-base-logs-pipeline/1
```

The pipeline configuration is valid, however, the pipeline runtime will encounter error since the secret that contains credentials to communicate with the sink does not exist.

Running `goldsky pipeline monitor bad-base-logs-pipeline` we see:


As expected, the pipeline has encountered a terminal error. Please note that the desired status is still `ACTIVE` even though the pipeline runtime status is `TERMINATED`

```
❯ goldsky pipeline list
✔ Listing pipelines
─────────────────────────────────────────
│ Name │ Version │ Status │ Resource │
│ │ │ │ Size │
─────────────────────────────────────────
│ bad-base-logs-pipeline │ 1 │ ACTIVE │ s │
─────────────────────────────────────────
```

## Runtime visibility

Pipeline runtime visibility is an important part of the pipeline development workflow. Mirror pipelines expose:

1. Runtime status and error messages
2. Logs emitted by the pipeline
3. Metrics on `Records received`, which counts all the records the pipeline has received from source(s) and, `Records written` which counts all records the pipeline has written to sink(s).
4. [Email notifications](/mirror/about-pipeline#email-notifications)

Runtime status, error messages and metrics can be seen via two methods:

1. Pipeline dashboard at `https://app.goldsky.com/dashboard/pipelines/stream/

/`
2. `goldsky pipeline monitor ` CLI command

Logs can only be seen in the pipeline dashboard.

Mirror attempts to surface appropriate and actionable error message and status for users, however, there is always room for imporovements. Please [reachout](/getting-support) if you think the experience can be improved.

### Email notifications

If a pipeline fails terminally the project members will get notified via an email.


You can configure this nofication in the [Notifications section](https://app.goldsky.com/dashboard/settings#notifications) of your project

## Error handling

There are two broad categories of errors.

**Pipeline configuration schema error**

This means the schema of the pipeline configuration is not valid. These errors are usually caught before pipeline execution. Some possible scenarios:

* a required attribute is missing
* transform SQL has syntax errors
* pipeline name is invalid

**Pipeline runtime error**

This means the pipeline encountered error during execution at runtime.

Some possible scenarios:

* credentails stored in the secret are incorrect or do not have needed access privilages
* sink availability issues
* poison-pill record that breaks the business logic in the transforms
* `resource_size` limitation

Transient errors are automatically retried as per retry-policy (for upto 6 hours) whearas non-transient ones immediately terminate the pipeline.

While many errors can be resolved by user intervention, there is a possibility of platform errors as well. Please [reachout to support](/getting-support) for investigation.

## Resource sizing

`resource_size` represents the compute (vCPUs and RAM) available to the pipeline. There are several options for pipeline sizes: `s, m, l, xl, xxl`. This attribute influences [pricing](/pricing/summary#mirror) as well.

Resource sizing depends on a few different factors such as:

* number of sources, transforms, sinks
* expected amount of data to be processed.
* transform sql involves joining multiple sources and/or transforms

Here's some general information that you can use as reference:

* A `small` resource size is usually enough in most use case: it can handle full backfill of small chain datasets and write to speeds of up to 300K records per second. For pipelines using
 subgraphs as source it can reliably handle up to 8 subgraphs.
* Larger resource sizes are usually needed when backfilling large chains or when doing large JOINS (example: JOIN between accounts and transactions datasets in Solana)
* It's recommended to always follow a defensive approach: start small and scale up if needed.

## Snapshots

A Pipeline snapshot captures a point-in-time state of a `RUNNING` pipeline allowing users to resume from it in the future.

It can be useful in various scenarios:

* evolving your `RUNNING` pipeline (eg: adding a new source, sink) without losing progress made so far.
* recover from new bug introductions where the user fix the bug and resume from an earlier snapshot to reprocess data.

Please note that snapshot only contains info about the progress made in reading the source(s) and the sql transform's state. It isn't representative of the state of the source/sink. For eg: if all data in the sink database table is deleted, resuming the pipeline from a snapshot does not recover it.

Currently, a pipeline can only be resumed from the latest available snapshot. If you need to resume from older snapshots, please [reachout to support](/getting-support)

Snapshots are closely tied to pipeline runtime in that all [commands](/reference/config-file/pipeline#pipeline-runtime-commands) that changes pipeline runtime has options to trigger a new snapshot and/or resume from the latest one.

```mermaid theme={null}
%%{init: { 'gitGraph': {'mainBranchName': 'myPipeline-v1'}, 'theme': 'default' , 'themeVariables': { 'git0': '#ffbf60' }}}%%
gitGraph
 commit id: " " type: REVERSE tag:"start"
 commit id: "snapshot1"
 commit id: "snapshot2"
 commit id: "snapshot3"
 commit id: "snapshot4" tag:"stop" type: HIGHLIGHT
 branch myPipeline-v2
 commit id: "snapshot4 " type: REVERSE tag:"start"
```

### When are snapshots taken?

1. When updating a `RUNNING` pipeline, a snapshot is created before applying the update. This is to ensure that there's an up-to-date snapshot in case the update introduces issues.
2. When pausing a pipeline.
3. Automatically on regular intervals. For `RUNNING` pipelines in healthy state, automatic snapshots are taken every 4 hours to ensure minimal data loss in case of errors.
4. Users can request snapshot creation via the following CLI command:

* `goldsky pipeline snapshot create `
* `goldsky pipeline apply --from-snapshot new`
* `goldsky pipeline apply --save-progress true` (CLI version \`

### How long does it take to create a snapshot

The amount of time it takes for a snapshot to be created depends largly on two factors. First, the amount of state accumulated during pipeline execution. Second, how fast records are being processed end-end in the pipeline.

In case of a long running snapshot that was triggered as part of an update to the pipeline, any future updates are blocked until snapshot is completed. Users do have an option to cancel the update request.

There is a scenario where the the pipeline was healthy at the time of starting the snapshot however, became unhealthy later preventing snapshot creation. Here, the pipeline will attempt to recover however, may need user intervention that involves restarting from last successful snapshot.

### Scenarios and Snapshot Behavior

Happy Scenario:

* Suppose a pipeline is at 50% progress, and an automatic snapshot is taken.
* The pipeline then progresses to 60% and is in a healthy state. If you pause the pipeline at this point, a new snapshot is taken.
* You can later start the pipeline from the 60% snapshot, ensuring continuity from the last known healthy state.

Bad Scenario:

* If the pipeline reaches 50%, and an automatic snapshot is taken.
* It then progresses to 60% but enters a bad state. Attempting to pause the pipeline in this state will fail.
* If you restart the pipeline, it will resume from the last successful snapshot at 50%, there was no snapshot created at 60%

Can't find what you're looking for? Reach out to us at [support@goldsky.com](mailto:support@goldsky.com) for help.

---

## 10. Database secrets

**URL:** https://docs.goldsky.com/mirror/manage-secrets

# Database secrets

## Overview

In order for Goldsky to connect to your sink, you have to configure secrets. Secrets refer to your datastore credentials which are securely stored in your Goldsky account.

You can create and manage your secrets with the `goldsky secret` command. To see a list of available commands and how to use them, please refer to the output of `goldsky secret -h`.

For sink-specific secret information, please refer to the [individual sink pages](/mirror/sinks).

## Guided CLI experience

If you create a pipeline with `goldsky pipeline create

`, there is no need to create a secret beforehand. The CLI will list existing secrets and offer you the option of creating a new secret as part of the pipeline creation flow.

---

## 11. Kafka

**URL:** https://docs.goldsky.com/mirror/extensions/channels/kafka

# Kafka

[Kafka](https://kafka.apache.org/) is a distributed streaming platform that is used to build real-time data pipelines and streaming applications. It is designed to be fast, scalable, and durable.

You can use Kafka to deeply integrate into your existing data ecosystem. Goldsky supplies a message format that allows you to handle blockchain forks and reorganizations with your downstream data pipelines.

Kafka has a rich ecosystem of SDKs and connectors you can make use of to do advanced data processing.


 **Less Magic Here**

 The Kafka integration is less end to end - while Goldsky will handle a ton of the topic partitioning balancing and other details, using Kafka is a bit more involved compared to getting data directly mirrored into a database.


Full configuration details for Kafka sink is available in the [reference](/reference/config-file/pipeline#kafka) page.

## Secrets

```shell theme={null}

goldsky secret create --name A_KAFKA_SECRET --value '{
 "type": "kafka",
 "bootstrapServers": "Type.String()",
 "securityProtocol": "Type.Enum(SecurityProtocol)",
 "saslMechanism": "Type.Optional(Type.Enum(SaslMechanism))",
 "saslJaasUsername": "Type.Optional(Type.String())",
 "saslJaasPassword": "Type.Optional(Type.String())",
 "schemaRegistryUrl": "Type.Optional(Type.String())",
 "schemaRegistryUsername": "Type.Optional(Type.String())",
 "schemaRegistryPassword": "Type.Optional(Type.String())"
}'
```

---

## 12. Webhook

**URL:** https://docs.goldsky.com/mirror/sinks/webhook

# Webhook

A Webhook sink allows you to send data to an external service via HTTP. This provides considerable flexibility for forwarding pipeline results to your application server, a third-party API, or a bot.

Webhook sinks ensure at least once delivery and manage back-pressure, meaning data delivery adapts based on the responsiveness of your endpoints. The pipeline sends a POST request with a JSON payload to a specified URL, and the receiver only needs to return a 200 status code to confirm successful delivery.

Here is a snippet of YAML that specifies a Webhook sink:

## Pipeline configuration



 ```yaml theme={null}
 sinks:
 my_webhook_sink:
 type: webhook

 # The webhook url
 url: Type.String()

 # The object key coming from either a source or transform.
 # Example: ethereum.raw_blocks.
 from: Type.String()

 # The name of a goldsky httpauth secret you created which contains a header that can be used for authentication. More on how to create these in the section below.
 secret_name: Type.Optional(Type.String())

 # Optional metadata that you want to send on every request.
 headers:
 SOME-HEADER-KEY: Type.Optional(Type.String())

 # Whether to send only one row per http request (better for compatibility with third-party integrations - e.g bots) or to mini-batch it (better for throughput).
 one_row_per_request: Type.Optional(Type.Boolean())

 # The number of records the sink will send together in a batch. Default `100`
 batch_size: Type.Optional(Type.Integer())

 # The maximum time the sink will batch records before flushing. Examples: 60s, 1m, 1h. Default: '1s'
 batch_flush_interval: Type.Optional(Type.String())
 ```



 ```yaml theme={null}
 sinks:
 myWebhookSink:
 type: webhook

 # The webhook url
 url: Type.String()

 # The object key coming from either a source or transform.
 # Example: ethereum.raw_blocks.
 from: Type.String()

 # The name of a goldsky httpauth secret you created which contains a header that can be used for authentication. More on how to create these in the section below.
 secretName: Type.Optional(Type.String())

 # Optional metadata that you want to send on every request.
 headers:
 SOME-HEADER-KEY: Type.Optional(Type.String())

 # Whether to send only one row per http request (better for compatibility with third-party integrations - e.g bots) or to mini-batch it (better for throughput).
 oneRowPerRequest: Type.Optional(Type.Boolean())

 # The number of records the sink will send together in a batch. Default `100`
 batchSize: Type.Optional(Type.Integer())

 # The maximum time the sink will batch records before flushing. Examples: 60s, 1m, 1h. Default: '1s'
 batchFlushInterval: Type.Optional(Type.String())
 ```



## Key considerations

* **Failure Handling:** In case of failures, the pipeline retries requests indefinitely with exponential backoff.
* **Networking & Performance:** For optimal performance, deploy your webhook server in a region close to where the pipelines are deployed (we use aws `us-west-2`). Aim to keep p95 latency under 100 milliseconds for best results.
* **Latency vs Throughput:** Use lower batch\_size/batch\_flush\_interval to achive low latency and higher values to achieve high throughput (useful when backfilling/bootstraping).
* **Connection & Response times**: The maximum allowed response time is 5 minutes and the maximum allowed time to establish a connection is 1 minute.

## Secret creation

Create a httpauth secret with the following CLI command:

```shell theme={null}
goldsky secret create
```

Select `httpauth` as the secret type and then follow the prompts to finish creating your httpauth secret.

## Example Webhook sink configuration

```yaml theme={null}
sinks:
 my_webhook_sink:
 type: webhook
 url: https://my-webhook-service.com/webhook-1
 from: ethereum.raw_blocks
 secret_name: ETH_BLOCKS_SECRET
```

---

## 13. External Handler Transforms

**URL:** https://docs.goldsky.com/mirror/transforms/external-handlers

# External Handler Transforms

> Transforming data with an external http service.

With external handler transforms, you can send data from your Mirror pipeline to an external service via HTTP and return the processed results back into the pipeline. This opens up a world of possibilities by allowing you to bring your own custom logic, programming languages, and external services into the transformation process.

[In this repo](https://github.com/goldsky-io/documentation-examples/tree/main/mirror-pipelines/goldsky-enriched-erc20-pipeline) you can see an example implementation of enriching ERC-20 Transfer Events with an HTTP service.

**Key Features of External Handler Transforms:**

* Send data to external services via HTTP.
* Supports a wide variety of programming languages and external libraries.
* Handle complex processing outside the pipeline and return results in real time.
* Guaranteed at least once delivery and back-pressure control to ensure data integrity.

### How External Handlers work

1. The pipeline sends a POST request to the external handler with a mini-batch of JSON rows.
2. The external handler processes the data and returns the transformed rows in the same format and order as received.

### Example workflow

1. The pipeline sends data to an external service (e.g. a custom API).
2. The service processes the data and returns the results to the pipeline.
3. The pipeline continues processing the enriched data downstream.

### Example HTTP Request

```json theme={null}
 POST /external-handler
 [
 {"id": 1, "value": "abc"},
 {"id": 2, "value": "def"}
 ]
```

### Example HTTP Response

```json theme={null}
 [
 {"id": 1, "transformed_value": "xyz"},
 {"id": 2, "transformed_value": "uvw"}
 ]
```

### YAML config with an external transform


 ```YAML theme={null}
 transforms:
 my_external_handler_transform:
 type: handler # the transform type. [required]
 primary_key: hash # [required]
 url: http://example-url/example-transform-route # url that your external handler is bound to. [required]
 headers: # [optional]
 	 Some-Header: some_value # use http headers to pass any tokens your server requires for authentication or any metadata that you think is useful.
 from: ethereum.raw_blocks # the input for the handler. Data sent to your handler will have the same schema as this source/transform. [required]
 # A schema override signals to the pipeline that the handler will respond with a schema that differs from the upstream source/transform (in this case ethereum.raw_blocks).
 # No override means that the handler will do some processing, but that its output will maintain the upstream schema.
 # The return type of the handler is equal to the upstream schema after the override is applied. Make sure that your handler returns a response with rows that follow this schema.
 schema_override: # [optional]
 new_column_name: datatype # if you want to add a new column, do so by including its name and datatype.
 existing_column_name: new_datatype # if you want to change the type of an existing column (e.g. cast an int to string), do so by including its name and the new datatype
 other_existing_column_name: null # if you want to drop an existing column, do so by including its name and setting its datatype to null
 # The number of records the pipeline will send together in a batch. Default `100`
 batch_size: Type.Optional(Type.Integer())
 # The maximum time the pipeline will batch records before flushing. Examples: 60s, 1m, 1h. Default: '1s'
 batch_flush_interval: Type.Optional(Type.String())
 ```


### Schema override datatypes

When overriding the schema of the data returned by the handler it’s important to get the datatypes for each column right. The schema\_override property is a map of column names to Flink SQL datatypes.

Data types are nullable by default. If you need non-nullable types use \ NOT NULL. For example: STRING NOT NULL.


 | Data Type | Notes |
 | :------------- | :---------------------------------- |
 | STRING | |
 | BOOLEAN | |
 | BYTE | |
 | DECIMAL | Supports fixed precision and scale. |
 | SMALLINT | |
 | INTEGER | |
 | BIGINT | |
 | FLOAT | |
 | DOUBLE | |
 | TIME | Supports only a precision of 0. |
 | TIMESTAMP | |
 | TIMESTAMP\_LTZ | |
 | ARRAY | |
 | ROW | |


### Key considerations

* **Schema Changes:** If the external handler’s output schema changes, you will need to redeploy the pipeline with the relevant schema\_override.
* **Failure Handling:** In case of failures, the pipeline retries requests indefinitely with exponential backoff.
* **Networking & Performance:** For optimal performance, deploy your handler in a region close to where the pipelines are deployed (we use aws `us-west-2`). Aim to keep p95 latency under 100 milliseconds for best results.
* **Latency vs Throughput:** Use lower batch\_size/batch\_flush\_interval to achive low latency and higher values to achieve high throughput (useful when backfilling/bootstraping).
* **Connection & Response times**: The maximum allowed response time is 5 minutes and the maximum allowed time to establish a connection is 1 minute.

### In-order mode for external handlers

In-Order mode allows for subgraph-style processing inside mirror. Records are emitted to the handler in the order that they appear on-chain.

**How to get started**

1. Make sure that the sources that you want to use currently support [Fast Scan](/mirror/sources/direct-indexing). If they don’t, submit a request to support.
2. In your pipeline definition specify the `filter` and `in_order` attributes for your source.
3. Declare a transform of type handler or a sink of type webhook.

Simple transforms (e.g filtering) in between the source and the handler/webhook are allowed, but other complex transforms (e.g. aggregations, joins) can cause loss of ordering.

**Example YAML config, with in-order mode**


 ```YAML theme={null}
 name: in-order-pipeline
 sources:
 ethereum.raw_transactions:
 dataset_name: ethereum.raw_transactions
 version: 1.1.0
 type: dataset
 filter: block_number > 21875698 # [required]
 in_order: true # [required] enables in-order mode on the given source and its downstream transforms and sinks.
 sinks:
 my_in_order_sink:
 type: webhook
 url: https://my-handler.com/process-in-order
 headers:
 WEBHOOK-SECRET: secret_two
 secret_name: HTTPAUTH_SECRET_TWO
 from: another_transform
 my_sink:
 type: webhook
 url: https://python-handler.fly.dev/echo
 from: ethereum.raw_transactions
 ```


**Example in-order webhook sink**

```javascript theme={null}
const express = require('express');
const { Pool } = require('pg');

const app = express();
app.use(express.json());

// Database connection settings
const pool = new Pool({
 user: 'your_user',
 host: 'localhost',
 database: 'your_database',
 password: 'your_password',
 port: 5432,
});

async function isDuplicate(client, key) {
 const res = await client.query("SELECT 1 FROM processed_messages WHERE key = $1", [key]);
 return res.rowCount > 0;
}

app.post('/webhook', async (req, res) => {
 const client = await pool.connect();
 try {
 await client.query('BEGIN');

 const payload = req.body;
 const metadata = payload.metadata || {};
 const data = payload.data || {};
 const op = metadata.op;
 const key = metadata.key;

 if (!key || !op || !data) {
 await client.query('ROLLBACK');
 return res.status(400).json({ error: "Invalid payload" });
 }

 if (await isDuplicate(client, key)) {
 await client.query('ROLLBACK');
 return res.status(200).json({ message: "Duplicate request processed without write side effects" });
 }

 if (op === "INSERT") {
 const fields = Object.keys(data);
 const values = Object.values(data);
 const placeholders = fields.map((_, i) => `$${i + 1}`).join(', ');
 const query = `INSERT INTO my_table (${fields.join(', ')}) VALUES (${placeholders})`;
 await client.query(query, values);
 } else if (op === "DELETE") {
 const conditions = Object.keys(data).map((key, i) => `${key} = $${i + 1}`).join(' AND ');
 const values = Object.values(data);
 const query = `DELETE FROM my_table WHERE ${conditions}`;
 await client.query(query, values);
 } else {
 await client.query('ROLLBACK');
 return res.status(400).json({ error: "Invalid operation" });
 }

 await client.query("INSERT INTO processed_messages (key) VALUES ($1)", [key]);
 await client.query('COMMIT');
 return res.status(200).json({ message: "Success" });
 } catch (e) {
 await client.query('ROLLBACK');
 return res.status(500).json({ error: e.message });
 } finally {
 client.release();
 }
});

app.listen(5000, () => {
 console.log('Server running on port 5000');
});
```

**In-order mode tips**

* To observe records in order, either have a single instance of your handler responding to requests OR introduce some coordination mechanism to make sure that only one replica of the service can answer at a time.
* When deploying your service, avoid having old and new instances running at the same time. Instead, discard the current instance and incur a little downtime to preserve ordering.
* When receiving messages that have already been processed in the handler (pre-existing idempotency key or previous index (e.g already seen block number)) **don't** introduce any side effects on your side, but **do** respond to the message as usual (i.e., processed messages for handlers, success code for webhook sink) so that the pipeline knows to keep going.

### Useful tips

Schema Changes: A change in the output schema of the external handler requires redeployment with schema\_override.

* **Failure Handling:** The pipeline retries indefinitely with exponential backoff.
* **Networking:** Deploy the handler close to where the pipeline runs for better performance.
* **Latency:** Keep handler response times under 100ms to ensure smooth operation.

---

## 14. SQL Transforms

**URL:** https://docs.goldsky.com/mirror/transforms/sql-transforms

# SQL Transforms

> Transforming blockchain data with Streaming SQL

## SQL Transforms

SQL transforms allow you to write SQL queries to modify and shape data from multiple sources within the pipeline. This is ideal for operations that need to be performed within the data pipeline itself, such as filtering, aggregating, or joining datasets.

Depending on how you choose to [source](/mirror/sources/supported-sources) your data, you might find that you run into 1 of 2 challenges:

1. **You only care about a few contracts**

 Rather than fill up your database with a ton of extra data, you'd rather ***filter*** down your data to a smaller set.
2. **The data is still a bit raw**

 Maybe you'd rather track gwei rounded to the nearest whole number instead of wei. You're looking to ***map*** data to a different format so you don't have to run this calculation over and over again.

### The SQL Solution

You can use SQL-based transforms to solve both of these challenges that normally would have you writing your own indexer or data pipeline. Instead, Goldsky can automatically run these for you using just 3 pieces of info:

* `name`: **A shortname for this transform**

 You can refer to this from sinks via `from` or treat it as a table in SQL from other transforms.
* `sql`: **The actual SQL**

 To filter your data, use a `WHERE` clause, e.g. `WHERE liquidity > 1000`.

 To map your data, use an `AS` clause combined with `SELECT`, e.g. `SELECT wei / 1000000000 AS gwei`.
* `primary_key`: **A unique ID**

 This should be unique, but you can also use this to intentionally de-duplicate data - the latest row with the same ID will replace all the others.

Combine them together into your [config](/reference/config-file/pipeline):



 ```yaml theme={null}
 transforms:
 negative_fpmm_scaled_liquidity_parameter:
 sql: SELECT id FROM polymarket.fixed_product_market_maker WHERE scaled_liquidity_parameter


 ```yaml theme={null}
 transforms:
 - referenceName: negative_fpmm_scaled_liquidity_parameter
 type: sql
 sql: SELECT id FROM polygon.fixed_product_market_maker WHERE scaled_liquidity_parameter


That's it. You can now filter and map data to exactly what you need.

---

## 15. Timescale

**URL:** https://docs.goldsky.com/mirror/sinks/timescale

# Timescale


 **Closed Beta**

 This feature is in closed beta and only available for our enterprise customers.

 Please contact us at [support@goldsky.com](mailto:support@goldsky.com) to request access to this feature.


We partner with [Timescale](https://www.timescale.com) to provide teams with real-time data access on on-chain data, using a database powerful enough for time series analytical queries and fast enough for transactional workloads like APIs.

Timescale support is in the form of hypertables - any dataset that has a `timestamp`-like field can be used to create a Timescale hypertable.

You can also use the traditional JDBC/postgres sink with Timecale - you would just need to create the hypertable yourself.

You use TimescaleDB for anything you would use PostgreSQL for, including directly serving APIs and other simple indexed table look-ups. With Timescale Hypertables, you can also make complex database queries like time-windowed aggregations, continuous group-bys, and more.

Learn more about Timescale here: [https://docs.timescale.com/api/latest/](https://docs.timescale.com/api/latest/)

---

## 16. Elasticsearch

**URL:** https://docs.goldsky.com/mirror/sinks/elasticsearch

# Elasticsearch

Give your users blazing-fast auto-complete suggestions, full-text fuzzy searches, and scored recommendations based off of on-chain data.

[Elasticsearch](https://www.elastic.co/) is the leading search datastore, used for a wide variety of usecase for billions of datapoints a day, including search, roll-up aggregations, and ultra-fast lookups on text data.

Goldsky supports real-time insertion into Elasticsearch, with event data updating in Elasticsearch indexes as soon as it gets finalized on-chain.

See the [Elasticsearch docs](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/elasticsearch-intro.html) to see more of what it can do!

Full configuration details for Elasticsearch sink is available in the [reference](/reference/config-file/pipeline#elasticsearch) page.

Contact us at [sales@goldsky.com](mailto:sales@goldsky.com) to learn more about how we can power search for your on-chain data!

## Secrets

Create an Elasticsearch secret with the following CLI command:

```shell theme={null}
goldsky secret create --name AN_ELASTICSEARCH_SECRET --value '{
 "host": "Type.String()",
 "username": "Type.String()",
 "password": "Type.String()",
 "type": "elasticsearch"
}'
```

---

## 17. ClickHouse

**URL:** https://docs.goldsky.com/mirror/sinks/clickhouse

# ClickHouse

[ClickHouse](https://clickhouse.com/) is a highly performant and cost-effective OLAP database that can support real-time inserts. Mirror pipelines can write subgraph or blockchain data directly into ClickHouse with full data guarantees and reorganization handling.

Mirror can work with any ClickHouse setup, but we have several strong defaults. From our experimentation, the `ReplacingMergeTree` table engine with `append_only_mode` offers the best real-time data performance for large datasets.

[ReplacingMergeTree](https://clickhouse.com/docs/en/engines/table-engines/mergetree-family/replacingmergetree) engine is used for all sink tables by default. If you don't want to use a ReplacingMergeTree, you can pre-create the table with any data engine you'd like. If you don't want to use a ReplacingMergeTree, you can disable `append_only_mode`.

Full configuration details for Clickhouse sink is available in the [reference](/reference/config-file/pipeline#clickhouse) page.

## Secrets


 **Use HTTP**
 Mirror writes to ClickHouse via the `http` interface (often port `8443`), rather than the `tcp` interface (often port `9000`).


```shell theme={null}

goldsky secret create --name A_CLICKHOUSE_SECRET --value '{
 "url": "clickhouse://blah.host.com:8443?ssl=true",
 "type": "clickHouse",
 "username": "default",
 "password": "qwerty123",
 "databaseName": "myDatabase"
}'
```

## Required permissions

The user will need the following permissions for the target database.

* CREATE DATABASE permissions for that database
* INSERT, SELECT, CREATE, DROP table permissions for tables within that database

```sql theme={null}
CREATE USER 'username' IDENTIFIED WITH password 'user_password';

GRANT CREATE DATABASE ON goldsky.* TO 'username';
GRANT SELECT, INSERT, DROP, CREATE ON goldsky.* TO 'username';
```

It's highly recommended to assign a ROLE to the user as well, and restrict the amount of total memory and CPU the pipeline has access to. The pipeline will take what it needs to insert as fast as possible, and while that may be desired for a backfill, in a production scenario you may want to isolate those resources.

## Data consistency with ReplacingMergeTrees

With `ReplacingMergeTree` tables, we can write, overwrite, and flag rows with the same primary key for deletes without actually mutating. As a result, the actual raw data in the table may contain duplicates.

ClickHouse allows you to clean up duplicates and deletes from the table by running

```sql theme={null}
OPTIMIZE FINAL;
```

which will merge rows with the same primary key into one. This may not be deterministic and fully clean all data up, so it's recommended to also add the `FINAL` keyword after the table name for queries.

```SQL theme={null}
SELECT
FROM FINAL
```

This will run a clean-up process, though there may be performance considerations.

## Append-Only Mode


 **Proceed with Caution**

 Without `append_only_mode=true` (v2: `appendOnlyMode=true`), the pipeline may hit ClickHouse mutation flush limits. Write speed will also be slower due to mutations.


Append-only mode means the pipeline will only *write* and not *update* or *delete* tables. There will be no mutations, only inserts.

This drastically increases insert speed and reduces Flush exceptions (which happen when too many mutations are queued up).

It's highly recommended as it can help you operate a large dataset with many writes with a small ClickHouse instance.

When `append_only_mode` (v2: `appendOnlyMode`) is `true` (default and recommended for ReplacingMergeTrees), the sink behaves the following way:

* All updates and deletes are converted to inserts.
* `is_deleted` column is automatically added to a table. It contains `1` in case of deletes, `0` otherwise.
* If `versionColumnName` is specified, it's used as a [version number column](https://clickhouse.com/docs/en/engines/table-engines/mergetree-family/replacingmergetree#ver) for deduplication. If it's not specified, `insert_time` column is automatically added to a table. It contains insertion time and is used for deduplication.
* Primary key is used in the `ORDER BY` clause.

This allows us to handle blockchain reorganizations natively while providing high insert speeds.

When `append_only_mode` (v2: `appendOnlyMode`) is `false`:

* All updates and deletes are propagated as is.
* No extra columns are added.
* Primary key is used in the `PRIMARY KEY` clause.

---

## 18. Untitled

**URL:** https://docs.goldsky.com/mirror/sinks

-
-
-
-
-
-
- (function(a,b,c){try{let d=localStorage.getItem(a);if(null==d)for(let c=0;c
 document.addEventListener('DOMContentLoaded', () => {
 const link = document.querySelector('link[href="https://d4tuoctqmanu0.cloudfront.net/katex.min.css"]');
 link.rel = 'stylesheet';
 });


((a,b,c,d,e,f,g,h)=>{let i=document.documentElement,j=["light","dark"];function k(b){var c;(Array.isArray(a)?a:[a]).forEach(a=>{let c="class"===a,d=c&&f?e.map(a=>f[a]||a):e;c?(i.classList.remove(...d),i.classList.add(f&&f[b]?f[b]:b)):i.setAttribute(a,b)}),c=b,h&&j.includes(c)&&(i.style.colorScheme=c)}if(d)k(d);else try{let a=localStorage.getItem(b)||c,d=g&&"system"===a?window.matchMedia("(prefers-color-scheme: dark)").matches?"dark":"light":a;k(d)}catch(a){}})("class","isDarkMode","system",null,["dark","light","true","false","system"],{"true":"dark","false":"light","dark":"dark","light":"light"},true,true):root {
 --primary: 255 173 51;
 --primary-light: 255 191 96;
 --primary-dark: 255 173 51;
 --background-light: 255 255 255;
 --background-dark: 14 13 13;
 --gray-50: 250 248 244;
 --gray-100: 245 243 239;
 --gray-200: 230 227 224;
 --gray-300: 213 211 207;
 --gray-400: 166 163 160;
 --gray-500: 119 117 113;
 --gray-600: 87 85 81;
 --gray-700: 70 67 64;
 --gray-800: 45 42 38;
 --gray-900: 30 28 24;
 --gray-950: 17 15 11;
 }


Access Restricted

To gain access to this doc, provide your access code below.


Enter access code


Access


(self.__next_f=self.__next_f||[]).push([0])self.__next_f.push([1,"1:\"$Sreact.fragment\"\n2:I[47132,[],\"\"]\n3:I[55983,[\"3473\",\"static/chunks/891cff7f-2c9e6e8550c9a551.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"3558\",\"static/chunks/3558-fddc172a72b9afd8.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7841\",\"static/chunks/7841-00fecd9e8f1bb70f.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7261\",\"static/chunks/7261-d416a358707b6550.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7892\",\"static/chunks/7892-e62a7cf6ffb2ccf4.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"4518\",\"static/chunks/4518-6b7118c60cd905b1.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"8039\",\"static/chunks/app/error-b040d5f8cf841de1.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\"],\"default\",1]\n4:I[75082,[],\"\"]\n"])self.__next_f.push([1,"5:I[85506,[\"3473\",\"static/chunks/891cff7f-2c9e6e8550c9a551.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"1725\",\"static/chunks/d30757c7-de787cbe1c08669b.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"3558\",\"static/chunks/3558-fddc172a72b9afd8.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7841\",\"static/chunks/7841-00fecd9e8f1bb70f.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7261\",\"static/chunks/7261-d416a358707b6550.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7545\",\"static/chunks/7545-60869a4114f7ae15.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"4436\",\"static/chunks/4436-d0ce83d5e11f11de.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"6527\",\"static/chunks/6527-0cfb2d96505d7cbd.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"9242\",\"static/chunks/9242-3e34f8ac634357ac.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"2864\",\"static/chunks/2864-04288363fc5c3c65.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"3258\",\"static/chunks/3258-4939df85402d2773.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7498\",\"static/chunks/7498-99247f6c149997d0.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"8083\",\"static/chunks/8083-d1cd2287cdfdb928.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7892\",\"static/chunks/7892-e62a7cf6ffb2ccf4.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"5907\",\"static/chunks/5907-97ae5c2afd50f738.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"5042\",\"static/chunks/5042-b0c5439e99785afa.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"9319\",\"static/chunks/9319-5d740d5b4131bcc5.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"1750\",\"static/chunks/1750-b8d62ab6b08d4eba.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"8047\",\"static/chunks/8047-d0623440424c2cb7.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"5456\",\"static/chunks/app/%255Fsites/%5Bsubdomain%5D/(multitenant)/layout-d6018563a37b4650.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\"],\"ThemeProvider\"]\n"])self.__next_f.push([1,"6:I[89481,[\"3473\",\"static/chunks/891cff7f-2c9e6e8550c9a551.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"3558\",\"static/chunks/3558-fddc172a72b9afd8.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7841\",\"static/chunks/7841-00fecd9e8f1bb70f.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7261\",\"static/chunks/7261-d416a358707b6550.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7892\",\"static/chunks/7892-e62a7cf6ffb2ccf4.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"2967\",\"static/chunks/app/%255Fsites/%5Bsubdomain%5D/not-found-def8880edfc41373.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\"],\"RecommendedPagesList\"]\nd:I[71256,[],\"\"]\n:HL[\"/mintlify-assets/_next/static/media/bb3ef058b751a6ad-s.p.woff2\",\"font\",{\"crossOrigin\":\"\",\"type\":\"font/woff2\"}]\n:HL[\"/mintlify-assets/_next/static/media/e4af272ccee01ff0-s.p.woff2\",\"font\",{\"crossOrigin\":\"\",\"type\":\"font/woff2\"}]\n:HL[\"/mintlify-assets/_next/static/css/641aaa5e2088f47f.css?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"style\"]\n:HL[\"/mintlify-assets/_next/static/css/d910ce6c26d880b3.css?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"style\"]\n"])self.__next_f.push([1,"0:{\"P\":null,\"b\":\"CimFi6HHLm2JdddW11G9U\",\"p\":\"/mintlify-assets\",\"c\":[\"\",\"_sites\",\"docs.goldsky.com\",\"_hidden-login-pages\",\"login\"],\"i\":false,\"f\":[[[\"\",{\"children\":[\"%5Fsites\",{\"children\":[[\"subdomain\",\"docs.goldsky.com\",\"d\"],{\"children\":[\"(login)\",{\"children\":[\"%5Fhidden-login-pages\",{\"children\":[\"login\",{\"children\":[\"__PAGE__\",{}]}]}]}]}]}]},\"$undefined\",\"$undefined\",true],[\"\",[\"$\",\"$1\",\"c\",{\"children\":[[[\"$\",\"link\",\"0\",{\"rel\":\"stylesheet\",\"href\":\"/mintlify-assets/_next/static/css/641aaa5e2088f47f.css?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"precedence\":\"next\",\"crossOrigin\":\"$undefined\",\"nonce\":\"$undefined\"}],[\"$\",\"link\",\"1\",{\"rel\":\"stylesheet\",\"href\":\"/mintlify-assets/_next/static/css/d910ce6c26d880b3.css?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"precedence\":\"next\",\"crossOrigin\":\"$undefined\",\"nonce\":\"$undefined\"}]],[\"$\",\"html\",null,{\"suppressHydrationWarning\":true,\"lang\":\"en\",\"className\":\"__variable_8c6b06 __variable_3bbdad dark\",\"data-banner-state\":\"visible\",\"data-page-mode\":\"none\",\"children\":[[\"$\",\"head\",null,{\"children\":[[\"$\",\"script\",null,{\"type\":\"text/javascript\",\"dangerouslySetInnerHTML\":{\"__html\":\"(function(a,b,c){try{let d=localStorage.getItem(a);if(null==d)for(let c=0;c\u003clocalStorage.length;c++){let e=localStorage.key(c);if(e?.endsWith(`-${b}`)\u0026\u0026(d=localStorage.getItem(e),null!=d)){localStorage.setItem(a,d),localStorage.setItem(e,d);break}}let e=document.getElementById(\\\"banner\\\")?.innerText,f=null==d||!!e\u0026\u0026d!==e;document.documentElement.setAttribute(c,f?\\\"visible\\\":\\\"hidden\\\")}catch(a){console.error(a),document.documentElement.setAttribute(c,\\\"hidden\\\")}})(\\n \\\"__mintlify-bannerDismissed\\\",\\n \\\"bannerDismissed\\\",\\n \\\"data-banner-state\\\",\\n)\"}}],[\"$\",\"link\",null,{\"rel\":\"preload\",\"href\":\"https://d4tuoctqmanu0.cloudfront.net/katex.min.css\",\"as\":\"style\"}],[\"$\",\"script\",null,{\"type\":\"text/javascript\",\"children\":\"\\n document.addEventListener('DOMContentLoaded', () =\u003e {\\n const link = document.querySelector('link[href=\\\"https://d4tuoctqmanu0.cloudfront.net/katex.min.css\\\"]');\\n link.rel = 'stylesheet';\\n });\\n \"}]]}],[\"$\",\"body\",null,{\"children\":[[\"$\",\"$L2\",null,{\"parallelRouterKey\":\"children\",\"error\":\"$3\",\"errorStyles\":[],\"errorScripts\":[],\"template\":[\"$\",\"$L4\",null,{}],\"templateStyles\":\"$undefined\",\"templateScripts\":\"$undefined\",\"notFound\":[[\"$\",\"$L5\",null,{\"children\":[[\"$\",\"style\",null,{\"children\":\":root {\\n --primary: 22 163 74;\\n --primary-light: 74 222 128;\\n --primary-dark: 22 101 52;\\n --background-light: 255 255 255;\\n --background-dark: 10 13 13;\\n --gray-50: 243 247 245;\\n --gray-100: 238 242 240;\\n --gray-200: 223 227 224;\\n --gray-300: 206 211 208;\\n --gray-400: 159 163 160;\\n --gray-500: 112 116 114;\\n --gray-600: 80 84 82;\\n --gray-700: 63 67 64;\\n --gray-800: 38 42 39;\\n --gray-900: 23 27 25;\\n --gray-950: 10 15 12;\\n }\"}],null,null,[\"$\",\"style\",null,{\"children\":\":root {\\n --primary: 17 120 102;\\n --primary-light: 74 222 128;\\n --primary-dark: 22 101 52;\\n --background-light: 255 255 255;\\n --background-dark: 15 17 23;\\n}\"}],[\"$\",\"main\",null,{\"className\":\"h-screen bg-background-light dark:bg-background-dark text-left\",\"children\":[\"$\",\"article\",null,{\"className\":\"bg-custom bg-fixed bg-center bg-cover relative flex flex-col items-center justify-center h-full\",\"children\":[\"$\",\"div\",null,{\"className\":\"w-full max-w-xl px-10\",\"children\":[[\"$\",\"span\",null,{\"className\":\"inline-flex mb-6 rounded-full px-3 py-1 text-sm font-semibold mr-4 text-white p-1 bg-primary\",\"children\":[\"Error \",404]}],[\"$\",\"h1\",null,{\"className\":\"font-semibold mb-3 text-3xl\",\"children\":\"Page not found!\"}],[\"$\",\"p\",null,{\"className\":\"text-lg text-gray-600 dark:text-gray-400 mb-6\",\"children\":\"We couldn't find the page.\"}],[\"$\",\"$L6\",null,{}]]}]}]}]]}],[]],\"forbidden\":\"$undefined\",\"unauthorized\":\"$undefined\"}],null]}]]}]]}],{\"children\":[\"%5Fsites\",[\"$\",\"$1\",\"c\",{\"children\":[null,[\"$\",\"$L2\",null,{\"parallelRouterKey\":\"children\",\"error\":\"$undefined\",\"errorStyles\":\"$undefined\",\"errorScripts\":\"$undefined\",\"template\":[\"$\",\"$L4\",null,{}],\"templateStyles\":\"$undefined\",\"templateScripts\":\"$undefined\",\"notFound\":\"$undefined\",\"forbidden\":\"$undefined\",\"unauthorized\":\"$undefined\"}]]}],{\"children\":[[\"subdomain\",\"docs.goldsky.com\",\"d\"],\"$L7\",{\"children\":[\"(login)\",\"$L8\",{\"children\":[\"%5Fhidden-login-pages\",\"$L9\",{\"children\":[\"login\",\"$La\",{\"children\":[\"__PAGE__\",\"$Lb\",{},null,false]},null,false]},null,false]},null,false]},null,false]},null,false]},null,false],\"$Lc\",false]],\"m\":\"$undefined\",\"G\":[\"$d\",[]],\"s\":false,\"S\":true}\n"])self.__next_f.push([1,"e:I[81925,[\"3473\",\"static/chunks/891cff7f-2c9e6e8550c9a551.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"3558\",\"static/chunks/3558-fddc172a72b9afd8.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7841\",\"static/chunks/7841-00fecd9e8f1bb70f.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7261\",\"static/chunks/7261-d416a358707b6550.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7892\",\"static/chunks/7892-e62a7cf6ffb2ccf4.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"4518\",\"static/chunks/4518-6b7118c60cd905b1.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"9249\",\"static/chunks/app/%255Fsites/%5Bsubdomain%5D/error-bee1299859815f18.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\"],\"default\",1]\n10:I[50700,[],\"OutletBoundary\"]\n12:I[87748,[],\"AsyncMetadataOutlet\"]\n14:I[50700,[],\"ViewportBoundary\"]\n16:I[50700,[],\"MetadataBoundary\"]\n17:\"$Sreact.suspense\"\n"])self.__next_f.push([1,"7:[\"$\",\"$1\",\"c\",{\"children\":[null,[\"$\",\"$L2\",null,{\"parallelRouterKey\":\"children\",\"error\":\"$e\",\"errorStyles\":[],\"errorScripts\":[],\"template\":[\"$\",\"$L4\",null,{}],\"templateStyles\":\"$undefined\",\"templateScripts\":\"$undefined\",\"notFound\":[[\"$\",\"$L5\",null,{\"children\":[[\"$\",\"style\",null,{\"children\":\":root {\\n --primary: 22 163 74;\\n --primary-light: 74 222 128;\\n --primary-dark: 22 101 52;\\n --background-light: 255 255 255;\\n --background-dark: 10 13 13;\\n --gray-50: 243 247 245;\\n --gray-100: 238 242 240;\\n --gray-200: 223 227 224;\\n --gray-300: 206 211 208;\\n --gray-400: 159 163 160;\\n --gray-500: 112 116 114;\\n --gray-600: 80 84 82;\\n --gray-700: 63 67 64;\\n --gray-800: 38 42 39;\\n --gray-900: 23 27 25;\\n --gray-950: 10 15 12;\\n }\"}],null,null,[\"$\",\"style\",null,{\"children\":\":root {\\n --primary: 17 120 102;\\n --primary-light: 74 222 128;\\n --primary-dark: 22 101 52;\\n --background-light: 255 255 255;\\n --background-dark: 15 17 23;\\n}\"}],[\"$\",\"main\",null,{\"className\":\"h-screen bg-background-light dark:bg-background-dark text-left\",\"children\":[\"$\",\"article\",null,{\"className\":\"bg-custom bg-fixed bg-center bg-cover relative flex flex-col items-center justify-center h-full\",\"children\":[\"$\",\"div\",null,{\"className\":\"w-full max-w-xl px-10\",\"children\":[[\"$\",\"span\",null,{\"className\":\"inline-flex mb-6 rounded-full px-3 py-1 text-sm font-semibold mr-4 text-white p-1 bg-primary\",\"children\":[\"Error \",404]}],[\"$\",\"h1\",null,{\"className\":\"font-semibold mb-3 text-3xl\",\"children\":\"Page not found!\"}],[\"$\",\"p\",null,{\"className\":\"text-lg text-gray-600 dark:text-gray-400 mb-6\",\"children\":\"We couldn't find the page.\"}],[\"$\",\"$L6\",null,{}]]}]}]}]]}],[]],\"forbidden\":\"$undefined\",\"unauthorized\":\"$undefined\"}]]}]\n"])self.__next_f.push([1,"8:[\"$\",\"$1\",\"c\",{\"children\":[null,[\"$\",\"$L2\",null,{\"parallelRouterKey\":\"children\",\"error\":\"$undefined\",\"errorStyles\":\"$undefined\",\"errorScripts\":\"$undefined\",\"template\":[\"$\",\"$L4\",null,{}],\"templateStyles\":\"$undefined\",\"templateScripts\":\"$undefined\",\"notFound\":\"$undefined\",\"forbidden\":\"$undefined\",\"unauthorized\":\"$undefined\"}]]}]\n9:[\"$\",\"$1\",\"c\",{\"children\":[null,[\"$\",\"$L2\",null,{\"parallelRouterKey\":\"children\",\"error\":\"$undefined\",\"errorStyles\":\"$undefined\",\"errorScripts\":\"$undefined\",\"template\":[\"$\",\"$L4\",null,{}],\"templateStyles\":\"$undefined\",\"templateScripts\":\"$undefined\",\"notFound\":\"$undefined\",\"forbidden\":\"$undefined\",\"unauthorized\":\"$undefined\"}]]}]\na:[\"$\",\"$1\",\"c\",{\"children\":[null,[\"$\",\"$L2\",null,{\"parallelRouterKey\":\"children\",\"error\":\"$undefined\",\"errorStyles\":\"$undefined\",\"errorScripts\":\"$undefined\",\"template\":[\"$\",\"$L4\",null,{}],\"templateStyles\":\"$undefined\",\"templateScripts\":\"$undefined\",\"notFound\":\"$undefined\",\"forbidden\":\"$undefined\",\"unauthorized\":\"$undefined\"}]]}]\nb:[\"$\",\"$1\",\"c\",{\"children\":[\"$Lf\",null,[\"$\",\"$L10\",null,{\"children\":[\"$L11\",[\"$\",\"$L12\",null,{\"promise\":\"$@13\"}]]}]]}]\nc:[\"$\",\"$1\",\"h\",{\"children\":[null,[[\"$\",\"$L14\",null,{\"children\":\"$L15\"}],[\"$\",\"meta\",null,{\"name\":\"next-size-adjust\",\"content\":\"\"}]],[\"$\",\"$L16\",null,{\"children\":[\"$\",\"div\",null,{\"hidden\":true,\"children\":[\"$\",\"$17\",null,{\"fallback\":null,\"children\":\"$L18\"}]}]}]]}]\n"])self.__next_f.push([1,"15:[[\"$\",\"meta\",\"0\",{\"charSet\":\"utf-8\"}],[\"$\",\"meta\",\"1\",{\"name\":\"viewport\",\"content\":\"width=device-width, initial-scale=1\"}]]\n11:null\n"])self.__next_f.push([1,"13:{\"metadata\":[],\"error\":null,\"digest\":\"$undefined\"}\n"])self.__next_f.push([1,"18:\"$13:metadata\"\n"])self.__next_f.push([1,"19:I[5947,[\"3473\",\"static/chunks/891cff7f-2c9e6e8550c9a551.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"3558\",\"static/chunks/3558-fddc172a72b9afd8.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7841\",\"static/chunks/7841-00fecd9e8f1bb70f.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"4436\",\"static/chunks/4436-d0ce83d5e11f11de.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"7892\",\"static/chunks/7892-e62a7cf6ffb2ccf4.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"5907\",\"static/chunks/5907-97ae5c2afd50f738.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\",\"5715\",\"static/chunks/app/%255Fsites/%5Bsubdomain%5D/(login)/%255Fhidden-login-pages/login/page-c53c4924f574dfeb.js?dpl=dpl_3oJjUFqmqAZsijDcgx3P6E6ZvVp4\"],\"LoginComponent\",1]\n"])self.__next_f.push([1,"f:[\"$\",\"$L19\",null,{\"docsConfig\":{\"name\":\"Goldsky\",\"logo\":{\"light\":\"https://mintcdn.com/goldsky-38/djvhUUMseW21frQF/images/logo/light.svg?fit=max\u0026auto=format\u0026n=djvhUUMseW21frQF\u0026q=85\u0026s=33b340d73ed215a5536d400e3d607cb3\",\"dark\":\"https://mintcdn.com/goldsky-38/djvhUUMseW21frQF/images/logo/dark.svg?fit=max\u0026auto=format\u0026n=djvhUUMseW21frQF\u0026q=85\u0026s=eddd3726aa53fad9b3361c401056302e\"},\"favicon\":\"/images/favicon.svg\",\"colors\":{\"primary\":\"#FFAD33\",\"light\":\"#FFBF60\",\"dark\":\"#FFAD33\"},\"navigation\":[]},\"favicons\":{\"icons\":[{\"rel\":\"apple-touch-icon\",\"sizes\":\"180x180\",\"type\":\"image/png\",\"href\":\"https://mintcdn.com/goldsky-38/2G5ZPIp8LG_P-NaW/_generated/favicon/apple-touch-icon.png?fit=max\u0026n=2G5ZPIp8LG_P-NaW\u0026s=09996433c8201e0a5eb43f8d8b4f4c9c\"},{\"rel\":\"icon\",\"sizes\":\"16x16\",\"type\":\"image/png\",\"media\":\"(prefers-color-scheme: light)\",\"href\":\"https://mintcdn.com/goldsky-38/2G5ZPIp8LG_P-NaW/_generated/favicon/favicon-16x16.png?fit=max\u0026n=2G5ZPIp8LG_P-NaW\u0026s=522a08e0ddb5a6acc9fcf33696751827\"},{\"rel\":\"icon\",\"sizes\":\"32x32\",\"type\":\"image/png\",\"media\":\"(prefers-color-scheme: light)\",\"href\":\"https://mintcdn.com/goldsky-38/2G5ZPIp8LG_P-NaW/_generated/favicon/favicon-32x32.png?fit=max\u0026n=2G5ZPIp8LG_P-NaW\u0026s=6040c9e88bce5763db673bc2291eaa3e\"},{\"rel\":\"shortcut icon\",\"type\":\"image/x-icon\",\"media\":\"(prefers-color-scheme: light)\",\"href\":\"https://mintcdn.com/goldsky-38/2G5ZPIp8LG_P-NaW/_generated/favicon/favicon.ico?fit=max\u0026n=2G5ZPIp8LG_P-NaW\u0026s=3b89f06df79a32c9f08298c5058fc14f\"},{\"rel\":\"icon\",\"sizes\":\"16x16\",\"type\":\"image/png\",\"media\":\"(prefers-color-scheme: dark)\",\"href\":\"https://mintcdn.com/goldsky-38/2G5ZPIp8LG_P-NaW/_generated/favicon-dark/favicon-16x16.png?fit=max\u0026n=2G5ZPIp8LG_P-NaW\u0026s=d37aca0145322b46c2444222e2723b4a\"},{\"rel\":\"icon\",\"sizes\":\"32x32\",\"type\":\"image/png\",\"media\":\"(prefers-color-scheme: dark)\",\"href\":\"https://mintcdn.com/goldsky-38/2G5ZPIp8LG_P-NaW/_generated/favicon-dark/favicon-32x32.png?fit=max\u0026n=2G5ZPIp8LG_P-NaW\u0026s=07bf28f7a5f633613713acfe9f912881\"},{\"rel\":\"shortcut icon\",\"type\":\"image/x-icon\",\"media\":\"(prefers-color-scheme: dark)\",\"href\":\"https://mintcdn.com/goldsky-38/2G5ZPIp8LG_P-NaW/_generated/favicon-dark/favicon.ico?fit=max\u0026n=2G5ZPIp8LG_P-NaW\u0026s=1ba2820bf5f50836f071b29e31c5c2df\"}],\"browserconfig\":\"https://mintcdn.com/goldsky-38/2G5ZPIp8LG_P-NaW/_generated/favicon/browserconfig.xml?n=2G5ZPIp8LG_P-NaW\u0026s=d8b37c4f486be14a74e54063080ca70f\"},\"subdomain\":\"goldsky-38\"}]\n"])

---

## 19. MySQL

**URL:** https://docs.goldsky.com/mirror/sinks/mysql

# MySQL

[MySQL](https://www.mysql.com/) is a powerful, open source object-relational database system used for OLTP workloads.

Mirror supports MySQL as a sink, allowing you to write data directly into MySQL. This provides a robust and flexible solution for both mid-sized analytical workloads and high performance REST and GraphQL APIs.

When you create a new pipeline, a table will be automatically created with columns from the source dataset. If a table is already created, the pipeline will write to it. As an example, you can set up partitions before you setup the pipeline, allowing you to scale MySQL even further.

MySQL also supports Timescale hypertables, if the hypertable is already setup. We have a separate Timescale sink in technical preview that will automatically setup hypertables for you - contact [support@goldsky.com](mailto:support@goldsky.com) for access.

Full configuration details for MySQL sink is available in the [reference](/reference/config-file/pipeline#mysql) page.

## Role Creation

Here is an example snippet to give the permissions needed for pipelines.

```sql theme={null}

CREATE ROLE goldsky_writer WITH LOGIN PASSWORD 'supersecurepassword';

-- Allow the pipeline to create schemas.
-- This is needed even if the schemas already exist
GRANT CREATE ON DATABASE mysql TO goldsky_writer;

-- For existing schemas that you want the pipeline to write to:
GRANT USAGE, CREATE ON SCHEMA TO goldsky_writer;

```

## Secret Creation

Create a MySQL secret with the following CLI command:

```shell theme={null}
goldsky secret create --name A_MYSQL_SECRET --value '{
 "type": "jdbc",
 "protocol": "mysql",
 "host": "db.host.com",
 "port": 5432,
 "databaseName": "myDatabase",
 "user": "myUser",
 "password": "myPassword"
}'
```

## Examples

### Getting an edge-only stream of decoded logs

This definition gets real-time edge stream of decoded logs straight into a MySQL table named `eth_logs` in the `goldsky` schema, with the secret `A_MYSQL_SECRET` created above.



 ```yaml theme={null}
 name: ethereum-decoded-logs-to-mysql
 apiVersion: 3
 sources:
 my_ethereum_decoded_logs:
 dataset_name: ethereum.decoded_logs
 version: 1.0.0
 type: dataset
 start_at: latest
 transforms:
 logs:
 sql: |
 SELECT
 id,
 address,
 event_signature,
 event_params,
 raw_log.block_number as block_number,
 raw_log.block_hash as block_hash,
 raw_log.transaction_hash as transaction_hash
 FROM
 my_ethereum_decoded_logs
 primary_key: id
 sinks:
 my_mysql_sink:
 type: mysql
 table: eth_logs
 schema: goldsky
 secret_name: A_MYSQL_SECRET
 from: logs
 ```



 ```yaml theme={null}
 sources:
 - name: ethereum.decoded_logs
 version: 1.0.0
 type: dataset
 startAt: latest

 transforms:
 - sql: |
 SELECT
 id,
 address,
 event_signature,
 event_params,
 raw_log.block_number as block_number,
 raw_log.block_hash as block_hash,
 raw_log.transaction_hash as transaction_hash
 FROM
 ethereum.decoded_logs
 name: logs
 type: sql
 primaryKey: id

 sinks:
 - type: mysql
 table: eth_logs
 schema: goldsky
 secretName: A_MYSQL_SECRET
 sourceStreamName: logs
 ```



## Tips for backfilling large datasets into MySQL

While MySQL offers fast access of data, writing large backfills into MySQL can sometimes be hard to scale.

Often, pipelines are bottlenecked against sinks.

Here are some things to try:

### Avoid indexes on tables until *after* the backfill

Indexes increase the amount of writes needed for each insert. When doing many writes, inserts can slow down the process significantly if we're hitting resources limitations.

### Bigger batch\_sizes for the inserts

The `sink_buffer_max_rows` setting controls how many rows are batched into a single insert statement. Depending on the size of the events, you can increase this to help with write performance. `1000` is a good number to start with. The pipeline will collect data until the batch is full, or until the `sink_buffer_interval` is met.

### Temporarily scale up the database

Take a look at your database stats like CPU and Memory to see where the bottlenecks are. Often, big writes aren't blocked on CPU or RAM, but rather on network or disk I/O.

For Google Cloud SQL, there are I/O burst limits that you can surpass by increasing the amount of CPU.

For AWS RDS instances (including Aurora), the network burst limits are documented for each instance. A rule of thumb is to look at the `EBS baseline I/O` performance as burst credits are easily used up in a backfill scenario.

### Aurora Tips

When using Aurora, for large datasets, make sure to use `Aurora I/O optimized`, which charges for more storage, but gives you immense savings on I/O credits. If you're streaming the entire chain into your database, or have a very active subgraph, these savings can be considerable, and the disk performance is significantly more stable and results in more stable CPU usage pattern.

---

## 20. Hosted PostgreSQL

**URL:** https://docs.goldsky.com/mirror/sinks/hosted-postgres

# Hosted PostgreSQL

[PostgreSQL](https://www.postgresql.org/) is a powerful, open source object-relational database system used for OLTP workloads.

Mirror now supports hosted PostgreSQL as a sink, allowing you to write data directly into a PostgreSQL hosted by Goldsky via [NeonDB](https://neon.tech). This database will scale as you grow and you'll always be able to request a data export when and if you need it.

This simplifies development and allows you to not worry about database administration. We securely store, host and scale the database for you.

## Connecting

After you create a hosted Postgres database, we will display connection information so that you can access your data and run queries on it.

You can alsways reference the sink section of the web dashboard to view connection info for your hosted databases.

## Billing

Please see the[ pricing page](https://docs.goldsky.com/pricing/summary#hosted-databases-\(beta\)) for hosted Postgres pricing.

---

## 21. PostgreSQL

**URL:** https://docs.goldsky.com/mirror/sinks/postgres

# PostgreSQL

[PostgreSQL](https://www.postgresql.org/) is a powerful, open source object-relational database system used for OLTP workloads.

Mirror supports PostgreSQL as a sink, allowing you to write data directly into PostgreSQL. This provides a robust and flexible solution for both mid-sized analytical workloads and high performance REST and GraphQL APIs.

When you create a new pipeline, a table will be automatically created with columns from the source dataset. If a table is already created, the pipeline will write to it. As an example, you can set up partitions before you setup the pipeline, allowing you to scale PostgreSQL even further.

The PostgreSQL also supports Timescale hypertables, if the hypertable is already setup. We have a separate Timescale sink in technical preview that will automatically setup hypertables for you - contact [support@goldsky.com](mailto:support@goldsky.com) for access.

Full configuration details for PostgreSQL sink is available in the [reference](/reference/config-file/pipeline#postgresql) page.

## Role Creation

Here is an example snippet to give the permissions needed for pipelines.

```sql theme={null}

CREATE ROLE goldsky_writer WITH LOGIN PASSWORD 'supersecurepassword';

-- Allow the pipeline to create schemas.
-- This is needed even if the schemas already exist
GRANT CREATE ON DATABASE postgres TO goldsky_writer;

-- For existing schemas that you want the pipeline to write to:
GRANT USAGE, CREATE ON SCHEMA TO goldsky_writer;
```

## Secret Creation

Create a PostgreSQL secret with the following CLI command:

```shell theme={null}
goldsky secret create --name A_POSTGRESQL_SECRET --value '{
 "type": "jdbc",
 "protocol": "postgresql",
 "host": "db.host.com",
 "port": 5432,
 "databaseName": "myDatabase",
 "user": "myUser",
 "password": "myPassword"
}'
```

## Examples

### Getting an edge-only stream of decoded logs

This definition gets real-time edge stream of decoded logs straight into a postgres table named `eth_logs` in the `goldsky` schema, with the secret `A_POSTGRESQL_SECRET` created above.



 ```yaml theme={null}
 name: ethereum-decoded-logs-to-postgres
 apiVersion: 3
 sources:
 my_ethereum_decoded_logs:
 dataset_name: ethereum.decoded_logs
 version: 1.0.0
 type: dataset
 start_at: latest
 transforms:
 logs:
 sql: |
 SELECT
 id,
 address,
 event_signature,
 event_params,
 raw_log.block_number as block_number,
 raw_log.block_hash as block_hash,
 raw_log.transaction_hash as transaction_hash
 FROM
 my_ethereum_decoded_logs
 primary_key: id
 sinks:
 my_postgres_sink:
 type: postgres
 table: eth_logs
 schema: goldsky
 secret_name: A_POSTGRESQL_SECRET
 from: logs
 ```



 ```yaml theme={null}
 sources:
 - name: ethereum.decoded_logs
 version: 1.0.0
 type: dataset
 startAt: latest

 transforms:
 - sql: |
 SELECT
 id,
 address,
 event_signature,
 event_params,
 raw_log.block_number as block_number,
 raw_log.block_hash as block_hash,
 raw_log.transaction_hash as transaction_hash
 FROM
 ethereum.decoded_logs
 name: logs
 type: sql
 primaryKey: id

 sinks:
 - type: postgres
 table: eth_logs
 schema: goldsky
 secretName: A_POSTGRESQL_SECRET
 sourceStreamName: logs
 ```



## Tips for backfilling large datasets into PostgreSQL

While PostgreSQL offers fast access of data, writing large backfills into PostgreSQL can sometimes be hard to scale.

Often, pipelines are bottlenecked against sinks.

Here are some things to try:

### Avoid indexes on tables until *after* the backfill

Indexes increase the amount of writes needed for each insert. When doing many writes, inserts can slow down the process significantly if we're hitting resources limitations.

### Bigger batch\_sizes for the inserts

The `sink_buffer_max_rows` setting controls how many rows are batched into a single insert statement. Depending on the size of the events, you can increase this to help with write performance. `1000` is a good number to start with. The pipeline will collect data until the batch is full, or until the `sink_buffer_interval` is met.

### Temporarily scale up the database

Take a look at your database stats like CPU and Memory to see where the bottlenecks are. Often, big writes aren't blocked on CPU or RAM, but rather on network or disk I/O.

For Google Cloud SQL, there are I/O burst limits that you can surpass by increasing the amount of CPU.

For AWS RDS instances (including Aurora), the network burst limits are documented for each instance. A rule of thumb is to look at the `EBS baseline I/O` performance as burst credits are easily used up in a backfill scenario.

# Provider Specific Notes

### AWS Aurora Postgres

When using Aurora, for large datasets, make sure to use `Aurora I/O optimized`, which charges for more storage, but gives you immense savings on I/O credits. If you're streaming the entire chain into your database, or have a very active subgraph, these savings can be considerable, and the disk performance is significantly more stable and results in more stable CPU usage pattern.

### Supabase

Supabase's direct connection URLs only support IPv6 connections and will not work with our default validation. There are two solutions

1. Use `Session Pooling`. In the connection screen, scroll down to see the connection string for the session pooler. This will be included in all Supabase plans and will work for most people. However, sessions will expire, and may lead to some warning logs in your pipeline logs. These will be dealt with gracefully and no action is needed. No data will be lost due to a session disconnection.


2. Alternatively, buy the IPv4 add-on, if session pooling doesn't fit your needs. It can lead to more persistent direct connections,

---

## 22. AWS SQS

**URL:** https://docs.goldsky.com/mirror/extensions/channels/aws-sqs

# AWS SQS

When you need to react to every new event coming from the blockchain or subgraph, SQS can be a simple and resilient way get started. SQS works with any mirror source, including subgraph updates and on-chain events.

Mirror Pipelines will send events to an SQS queue of your choosing. You can then use AWS's SDK to process events, or even create a lambda to do serverless processing of the events.

SQS is append-only, so any events will be sent with the metadata needed to handle mutations as needed.

Full configuration details for SQS sink is available in the [reference](/reference/config-file/pipeline#sqs) page.

## Secrets

Create an AWS SQS secret with the following CLI command:

```shell theme={null}
goldsky secret create --name AN_AWS_SQS_SECRET --value '{
 "accessKey": "Type.String()",
 "secretAccessKey": "Type.String()",
 "region": "Type.String()",
 "type": "sqs"
}'
```


 Secret requires `sqs:SendMessage` permission. Refer to [AWS SQS permissions
 documentation](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-api-permissions-reference.html)
 for more information.


## Processing Data

Typically, you would use SQS sync to cue data up and process for one reason or another.

The data will have two high level fields:

* `op`: The operation type like `c` for create, `u` for update, `d` for delete)
* `body`: The actual row data

In a normal pipeline driven by blockchain events, or one sending transactions, logs, blocks or traces, when you see `op` = `d`, typically it means it's processing a fork or reorganization.

By default, the `id` of each of our datasets is consistent and meant for deterministic processing of blockchain forks. If you see a `d`, you can issue a delete for your downstream logic to negate the writing or processing of the data. The full body is provided for easy negation of aggregations.

If you're enriching the data and then writing it into a database, you can just do upsert logic for `c` and `u` and then delete for `d`.

---

## 23. Object Storage (S3/GCS/R2)

**URL:** https://docs.goldsky.com/mirror/extensions/channels/aws-s3

# Object Storage (S3/GCS/R2)


 The sub-second realtime and reorg-aware advantages of mirror are greatly
 diminished when using our S3 connector due to the constraints of file-based
 storage. If possible, it's highly recommended to use one of the other channels
 or sinks instead!


The files are created in [Parquet](https://parquet.apache.org/) format.

Files will be emitted on an interval, essentially mimicing a mini-batch system.

Data will also be append-only, so if there is a reorg, data with the same id will be emitted. It's up to the downstream consumers of this data to deduplicate the data.

Full configuration details for this sink is available in the [reference](/reference/config-file/pipeline#file) page.

## Secrets

Create an AWS S3 secret with the following CLI command:

```shell theme={null}
goldsky secret create --name AN_AWS_S3_SECRET --value '{
 "accessKeyId": "Type.String()",
 "secretAccessKey": "Type.String()",
 "region": "Type.String()",
 "type": "s3"
}'
```

## Partitioning

This sink supports folder-based partitioning through the `partition_columns` option.

In this example, it will store files in a different file for each day, based on the `block_timestamp` of each Base transfer.

`s3://test-bucket/base/transfers/erc20/`

```yaml theme={null}
name: example-partition
apiVersion: 3
sources:
 base.transfers:
 dataset_name: base.erc20_transfers
 version: 1.2.0
 type: dataset
 start_at: latest
transforms:
 transform_transactions:
 type: sql
 primary_key: id
 sql: |-
 select *, from_unixtime(block_timestamp/1000, 'yyyy-MM-dd') as dt
 from base.transfers
sinks:
 filesink_transform_transactions:
 secret_name: S3_SECRET
 path: s3://test-bucket/base/transfers/erc20/
 type: file
 format: parquet
 partition_columns: dt
 from: transform_transactions
```

---

## 24. Subgraphs

**URL:** https://docs.goldsky.com/mirror/sources/subgraphs

# Subgraphs

You can use subgraphs as a pipeline source, allowing you to combined the flexibility of subgraph indexing with the expressiveness of the database of your choice.

This enables a lot of powerful use-cases:

* Reuse all your existing subgraph entities.
* Increase querying speeds drastically compared to graphql-engines.
* Flexible aggregations that weren't possible with just GraphQL.
* Analytics on protocols through Clickhouse, and more.
* Plug into BI tools, train AI, and export data for your users

Full configuration details for Subgraph Entity source is available in the [reference](/reference/config-file/pipeline#subgraph-entity) page.

## Automatic Deduplication

Subgraphs natively support time travel queries. This means every historical version of every entity is stored. To do this, each row has an `id`, `vid`, and `block_range`.

When you update an entity in a subgraph mapping handler, a new row in the database is created with the same ID, but new VID and block\_range, and the old row's `block_range` is updated to have an end.

By default, pipelines **deduplicate** on `id`, to show only the latest row per `id`. In other words, historical entity state is not kept in the sink database. This saves a lot of database space and makes for easier querying, as additional deduplication logic is not needed for simple queries. In a postgres database for example, the pipeline will update existing rows with the values from the newest block.

This deduplication happens through setting the primary key in the data going through the pipeline. By default, the primary key is `id`.

If historical data is desired, you can set the primary key to `vid` through a transform.



 ```yaml theme={null}
 name: qidao-optimism-subgraph-to-postgrse
 apiVersion: 3
 sources:
 subgraph_account:
 type: subgraph_entity
 name: account
 subgraphs:
 - name: qidao-optimism
 version: 1.1.0
 transforms:
 historical_accounts:
 sql: >-
 select * from subgraph_account
 primary_key: vid
 sinks:
 postgres_account:
 type: postgres
 table: historical_accounts
 schema: goldsky
 secret_name: A_POSTGRESQL_SECRET
 from: historical_accounts
 ```



 ```yaml theme={null}
 sources:
 - type: subgraphEntity
 # The deployment IDs you gathered above. If you put multiple,
 # they must have the same schema
 deployments:
 - id: QmPuXT3poo1T4rS6agZfT51ZZkiN3zQr6n5F2o1v9dRnnr
 # A name, referred to later in the `sourceStreamName` of a transformation or sink
 referenceName: account
 entity:
 # The name of the entities
 name: account

 transforms:
 - referenceName: historical_accounts
 type: sql
 # The `account` referenced here is the referenceName set in the source
 sql: >-
 select * from account
 primaryKey: vid


 sinks:
 - type: postgres
 table: historical_accounts
 schema: goldsky
 secretName: A_POSTGRESQL_SECRET
 # the `historical_accounts` is the referenceKey of the transformation made above
 sourceStreamName: historical_accounts
 ```



In this case, all historical versions of the entity will be retained in the pipeline sink. If there was no table, tables will be automatically created as well.

## Using the wizard

### Subgraphs from your project

Use any of your own subgraphs as a pipeline source. Use `goldsky pipeline create

` and select `Project Subgraph`, and push subgraph data into any of our supported sinks.

### Community subgraphs

When you create a new pipeline with `goldsky pipeline create `, select **Community Subgraphs** as the source type. This will display a list of available subgraphs to choose from. Select the one you are interested in and follow the prompts to complete the pipeline creation.

This will get load the subgraph into your project and create a pipeline with that subgraph as the source.

---

## 25. Mirror - Supported sources

**URL:** https://docs.goldsky.com/mirror/sources/supported-sources

# Mirror - Supported sources



 Mirror data from community subgraphs or from your own custom subgraphs into any sink.



 Mirror entire chains into your database for analysis, or filter/transform them to what you need.

---

## 26. Migrate from TheGraph

**URL:** https://docs.goldsky.com/subgraphs/migrate-from-the-graph

# Migrate from TheGraph

Goldsky provides a one-step migration for your subgraphs on TheGraph's decentralized network. This is a **drop-in replacement** with the following benefits:

* The same subgraph API that your apps already use, allowing for seamless, zero-downtime migration
* A load-balanced network of third-party and on-prem RPC nodes to improve performance and reliability
* Tagging and versioning to hotswap subgraphs, allowing for seamless updates on your frontend
* Alerts and auto-recovery in case of subgraph data consistency issues due to corruption from re-orgs or other issues
* A world-class team who monitors your subgraphs 24/7, with on-call engineering support to help troubleshoot any issues

## Migrate subgraphs to Goldsky


 1. Install the Goldsky CLI:

 **For macOS/Linux:**

 ```shell theme={null}
 curl https://goldsky.com | sh
 ```

 **For Windows:**

 ```shell theme={null}
 npm install -g @goldskycom/cli
 ```

 Windows users need to have Node.js and npm installed first. Download from [nodejs.org](https://nodejs.org) if not already installed.
 2. Go to your [Project Settings](https://app.goldsky.com/dashboard/settings) page and create an API key.
 3. Back in your Goldsky CLI, log into your Project by running the command `goldsky login` and paste your API key.
 4. Now that you are logged in, run `goldsky` to get started:
 ```shell theme={null}
 goldsky
 ```


Use the following command to seamlessly migrate your subgraph to Goldsky via IPFS hash(visible on The Graph's Explorer page for the specified subgraph):

```bash theme={null}
goldsky subgraph deploy your-subgraph-name/your-version --from-ipfs-hash
```

You can alo get this IPFS deployment hash by querying any subgraph GraphQL endpoint with the following query:

```GraphQL theme={null}
query {
 _meta {
 deployment
 }
}
```

## Monitor indexing progress

Once you started the migration with the above command, you can monitor your subgraph's indexing status with:

```bash theme={null}
goldsky subgraph list
```

Alternatively, navigate to [app.goldsky.com](https://app.goldsky.com) to see your subgraphs, their indexing progress, and more.

---

## 27. Deploy a subgraph

**URL:** https://docs.goldsky.com/subgraphs/deploying-subgraphs

# Deploy a subgraph


There are three primary ways to deploy a subgraph on Goldsky:

1. From source code
2. Migrating from The Graph or any other subgraph host
3. Via instant, no-code subgraphs

For any of the above, you'll need to have the Goldsky CLI installed and be logged in; you can do this by following the instructions below.


 1. Install the Goldsky CLI:

 **For macOS/Linux:**

 ```shell theme={null}
 curl https://goldsky.com | sh
 ```

 **For Windows:**

 ```shell theme={null}
 npm install -g @goldskycom/cli
 ```

 Windows users need to have Node.js and npm installed first. Download from [nodejs.org](https://nodejs.org) if not already installed.
 2. Go to your [Project Settings](https://app.goldsky.com/dashboard/settings) page and create an API key.
 3. Back in your Goldsky CLI, log into your Project by running the command `goldsky login` and paste your API key.
 4. Now that you are logged in, run `goldsky` to get started:
 ```shell theme={null}
 goldsky
 ```


For these examples we'll use the Ethereum contract for [POAP](https://poap.xyz).

# From source code

If you’ve developed your own subgraph, you can deploy it from the source. In our example we’ll work off of a clone of the [POAP subgraph](https://github.com/goldsky-io/poap-subgraph).

First we need to clone the Git repo.

```shell theme={null}
git clone https://github.com/goldsky-io/poap-subgraph
```

Now change into that directory. From here, we'll build the subgraph from templates. Open source subgraphs have different instructions to get them to build, so check the `README.md` or look at the `package.json` for hints as to the correct build commands. Usually it's a two step process, but since POAP is deployed on multiple chains, there's one extra step at the start to generate the correct data from templates.

```shell theme={null}
yarn prepare:mainnet
yarn codegen
yarn build
```

Then you can deploy the subgraph to Goldsky using the following command.

```shell theme={null}
goldsky subgraph deploy poap-subgraph/1.0.0 --path .
```

# From The Graph or another host


 For a detailed walkthrough, follow our [dedicated
 guide](/subgraphs/migrate-from-the-graph).


# Via instant, no-code subgraphs


 For a detailed walkthrough, follow our [dedicated
 guide](/subgraphs/guides/create-a-no-code-subgraph).

---

