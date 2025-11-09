# Linear Workflow Examples

Real-world scenarios demonstrating Linear workflow patterns for software development teams.

---

## Table of Contents

1. [Scenario 1: Feature with Sub-tasks](#scenario-1-feature-with-sub-tasks-planned-work-breakdown)
2. [Scenario 2: Simple Feature Implementation](#scenario-2-simple-feature-implementation)
3. [Scenario 3: Bug Discovered During Development](#scenario-3-bug-discovered-during-development)
4. [Scenario 4: Database Performance Optimization](#scenario-4-database-performance-optimization)
5. [Scenario 5: API Rate Limiting](#scenario-5-api-rate-limiting)
6. [Scenario 6: Cross-Component Feature](#scenario-6-cross-component-feature)
7. [Scenario 7: Blocking Dependencies](#scenario-7-blocking-dependencies)
8. [Scenario 8: Smart Contract Deployment](#scenario-8-smart-contract-deployment)
9. [Scenario 9: Web3 Integration Feature](#scenario-9-web3-integration-feature)

---

## Scenario 1: Feature with Sub-tasks (Planned Work Breakdown)

### Context

Implement OAuth authentication with 3 specific tasks known upfront.

### Create Parent Feature

**Title:** `User Authentication - Add OAuth login support`

**Labels:** `Feature`, `backend`, `frontend`

**Using Linear MCP:**

```
Create issue with:
- title: "User Authentication - Add OAuth login support"
- description: "Implement OAuth 2.0 authentication for Google and GitHub providers"
- teamId: "team-uuid"
- priority: 2
- labelIds: ["Feature-uuid", "backend-uuid", "frontend-uuid"]
```

### Create Sub-tasks

**Sub-task 1: OAuth Credentials**

```
Create issue with:
- title: "Setup OAuth provider credentials"
- parentId: "parent-uuid"
- labelIds: ["Feature-uuid", "backend-uuid"]
```

**Sub-task 2: OAuth Routes**

```
Create issue with:
- title: "Implement OAuth callback routes"
- parentId: "parent-uuid"
- labelIds: ["Feature-uuid", "backend-uuid"]
```

**Sub-task 3: OAuth UI**

```
Create issue with:
- title: "Add OAuth login buttons to UI"
- parentId: "parent-uuid"
- labelIds: ["Feature-uuid", "frontend-uuid"]
```

---

### Key Takeaways

✅ **Use sub-tasks when:**

- Work breakdown is known at planning time
- Tasks are part of the same feature
- All sub-tasks must complete before parent is "Done"

❌ **Don't use sub-tasks for:**

- Bugs discovered during implementation
- Independent work that can be done separately

---

## Scenario 2: Simple Feature Implementation

### Context

Add a simple feature without sub-tasks.

### Create Feature

**Title:** `API - Add rate limiting middleware`

**Labels:** `Feature`, `backend`, `api`

**Using Linear MCP:**

```
Create issue with:
- title: "API - Add rate limiting middleware"
- description: "Implement rate limiting to prevent API abuse"
- teamId: "team-uuid"
- priority: 2
- labelIds: ["Feature-uuid", "backend-uuid", "api-uuid"]
```

---

### Key Takeaways

✅ **Simple features:**

- No sub-tasks needed for straightforward work
- Single issue is sufficient
- Easier to track and complete

---

## Scenario 3: Bug Discovered During Development

### Context

While implementing OAuth feature (ENG-99), discovered a bug in session handling.

### Create Bug Issue

**Title:** `Bug: Backend - Session not persisting after OAuth login`

**Labels:** `Bug`, `backend`, `security`

**Using Linear MCP:**

```
Create issue with:
- title: "Bug: Backend - Session not persisting after OAuth login"
- description: "Sessions are not being saved correctly after OAuth callback"
- teamId: "team-uuid"
- priority: 1
- labelIds: ["Bug-uuid", "backend-uuid", "security-uuid"]
```

### Link to Original Feature

```
Create issue relation with:
- issueId: "ENG-99-oauth-feature-uuid"
- relatedIssueId: "ENG-210-bug-uuid"
- type: "related"
```

---

### Key Takeaways

✅ **Bugs discovered during development:**

- Create separate bug issue (not sub-task)
- Link to original feature using "related"
- Can be fixed independently
- Can be prioritized separately

❌ **Don't use sub-tasks for:**

- Bugs discovered after planning
- Issues that weren't known upfront

---

## Scenario 4: Database Performance Optimization

### Context

Improve database query performance for user search.

### Create Improvement Issue

**Title:** `Database - Optimize user search query performance`

**Labels:** `Improvement`, `database`, `performance`

**Using Linear MCP:**

```
Create issue with:
- title: "Database - Optimize user search query performance"
- description: "Add indexes and optimize query to reduce search time from 2.3s to <200ms"
- teamId: "team-uuid"
- priority: 2
- labelIds: ["Improvement-uuid", "database-uuid", "performance-uuid"]
```

---

### Key Takeaways

✅ **Improvements:**

- Use for enhancing existing functionality
- Include success metrics in description
- Prioritize based on impact

---

## Scenario 5: API Rate Limiting

### Context

Implement API rate limiting with Redis backend.

### Create Feature with Technical Details

**Title:** `API - Implement Redis-based rate limiting`

**Labels:** `Feature`, `backend`, `api`, `infrastructure`

**Using Linear MCP:**

```
Create issue with:
- title: "API - Implement Redis-based rate limiting"
- description: "Implement rate limiting using Redis to prevent API abuse (100 req/min per IP)"
- teamId: "team-uuid"
- priority: 2
- labelIds: ["Feature-uuid", "backend-uuid", "api-uuid", "infrastructure-uuid"]
```

---

### Key Takeaways

✅ **Multi-component features:**

- Use multiple component labels
- Include technical details in description
- Specify success criteria

---

## Scenario 6: Cross-Component Feature

### Context

Implement user profile editing across frontend and backend.

### Create Feature

**Title:** `User Profile - Add profile editing`

**Labels:** `Feature`, `frontend`, `backend`, `database`

**Using Linear MCP:**

```
Create issue with:
- title: "User Profile - Add profile editing"
- description: "Allow users to edit their profile information (name, email, avatar)"
- teamId: "team-uuid"
- priority: 2
- labelIds: ["Feature-uuid", "frontend-uuid", "backend-uuid", "database-uuid"]
```

---

### Key Takeaways

✅ **Cross-component work:**

- Use all relevant component labels
- Single issue for cohesive feature
- Don't split unless work can be done independently

---

## Scenario 7: Blocking Dependencies

### Context

Feature ENG-150 (Payment Integration) is blocked by ENG-140 (User Verification).

### Create Blocking Relationship

```
Create issue relation with:
- issueId: "ENG-150-payment-uuid"
- relatedIssueId: "ENG-140-verification-uuid"
- type: "blocks"
```

---

### Key Takeaways

✅ **Blocking relationships:**

- Use "blocks" type for dependencies
- Clearly shows what's blocking what
- Helps with prioritization

---

## Scenario 8: Smart Contract Deployment

### Context

Deploy a new ERC-20 token contract to testnet and mainnet.

### Create Feature

**Title:** `Contract - Deploy ERC-20 token contract`

**Labels:** `Feature`, `contract`, `infrastructure`

**Using Linear MCP:**

```
Create issue with:
- title: "Contract - Deploy ERC-20 token contract"
- description: "Deploy MyToken ERC-20 contract to Sepolia testnet and Ethereum mainnet"
- teamId: "team-uuid"
- priority: 2
- labelIds: ["Feature-uuid", "contract-uuid", "infrastructure-uuid"]
```

---

### Key Takeaways

✅ **Smart contract features:**

- Use `contract` component label
- Include `infrastructure` for deployment-related work
- Specify testnet and mainnet deployment steps
- Include security audit requirements

---

## Scenario 9: Web3 Integration Feature

### Context

Implement wallet connection and token balance display in the frontend.

### Create Feature with Sub-tasks

**Title:** `Web3 Integration - Add wallet connection and token balance`

**Labels:** `Feature`, `frontend`, `contract`

**Using Linear MCP:**

```
Create issue with:
- title: "Web3 Integration - Add wallet connection and token balance"
- description: "Allow users to connect their Web3 wallet and view their token balance"
- teamId: "team-uuid"
- priority: 2
- labelIds: ["Feature-uuid", "frontend-uuid", "contract-uuid"]
```

### Create Sub-tasks

**Sub-task 1: Wallet Connection**

```
Create issue with:
- title: "Implement wallet connection UI"
- parentId: "parent-uuid"
- labelIds: ["Feature-uuid", "frontend-uuid"]
```

**Sub-task 2: Contract Integration**

```
Create issue with:
- title: "Integrate token contract for balance queries"
- parentId: "parent-uuid"
- labelIds: ["Feature-uuid", "contract-uuid"]
```

**Sub-task 3: Balance Display**

```
Create issue with:
- title: "Display token balance in UI"
- parentId: "parent-uuid"
- labelIds: ["Feature-uuid", "frontend-uuid"]
```

---

### Key Takeaways

✅ **Web3 features:**

- Use both `frontend` and `contract` labels for cross-component work
- Break down into frontend and contract sub-tasks
- Include wallet connection, contract interaction, and UI display
- Consider testnet testing before mainnet

---

**Last Updated:** 2025-11-02
**Maintained By:** R&D Team
