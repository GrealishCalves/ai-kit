# Clack UI & User Experience Playbook

## Overview

Build interactive CLI tools with excellent user experience using @clack/prompts. Focus on transparency, clear feedback, and user-friendly workflows.

## When to Use

- Interactive deployment wizards
- Configuration setup tools
- User confirmations for destructive operations
- Multi-step guided workflows

## When NOT to Use

- Automated scripts (use CLI arguments instead)
- CI/CD pipelines (non-interactive environments)
- Background processes
- Headless environments

---

## Core Principles

### 1. Transparency

**Always show what's happening:**

- Display actual commands being executed
- Show real-time progress
- Log all operations to file
- Provide verbose mode for debugging

### 2. User Confirmation

**Ask before destructive operations:**

- Deleting resources
- Deploying to production
- Overwriting existing data
- Making irreversible changes

### 3. Clear Feedback

**Provide immediate, actionable feedback:**

- Success messages with next steps
- Error messages with solutions
- Progress indicators for long operations
- Context for each step

---

## Clack Prompts API

### Basic Prompts

```javascript
import { intro, outro, text, select, confirm, spinner, log } from "@clack/prompts";

// Start wizard
intro("Deployment Wizard");

// Text input
const name = await text({
  message: "Enter subgraph name:",
  placeholder: "production-lottery",
  validate: (value) => {
    if (!value) return "Name is required";
    if (!/^[a-z0-9-]+$/.test(value)) return "Only lowercase letters, numbers, and hyphens";
  },
});

// Select from options
const network = await select({
  message: "Select network:",
  options: [
    { value: "testnet", label: "Base Sepolia", hint: "Testnet (84532)" },
    { value: "mainnet", label: "Base Mainnet", hint: "Production (8453)" },
  ],
});

// Confirmation
const shouldDeploy = await confirm({
  message: "Proceed with deployment?",
});

// Spinner for long operations
const s = spinner();
s.start("Deploying to Goldsky...");
await deployToGoldsky();
s.stop("Deployment complete");

// End wizard
outro("Deployment successful!");
```

---

## Patterns

### 1. Intro/Outro Pattern

**Always wrap wizards with intro/outro:**

```javascript
async function deployWizard() {
  intro("Goldsky Subgraph Deployment");

  try {
    // Wizard steps here

    outro("Deployment complete!");
  } catch (error) {
    log.error(`Error: ${error.message}`);
    outro("Deployment failed");
    process.exit(1);
  }
}
```

**Why:**

- Clear start/end boundaries
- Consistent user experience
- Proper error handling

---

### 2. Validation Pattern

**Validate user input immediately:**

```javascript
const version = await text({
  message: "Enter version (e.g., 1.0.0):",
  placeholder: "1.0.0",
  validate: (value) => {
    if (!value) return "Version is required";
    if (!/^\d+\.\d+\.\d+$/.test(value)) {
      return "Version must be in format: major.minor.patch (e.g., 1.0.0)";
    }
  },
});
```

**Why:**

- Immediate feedback
- Prevents invalid input
- Clear error messages

---

### 3. Cancellation Pattern

**Handle user cancellation gracefully:**

```javascript
import { isCancel, cancel } from "@clack/prompts";

const name = await text({
  message: "Enter name:",
});

if (isCancel(name)) {
  cancel("Operation cancelled");
  return;
}

// Continue with name
```

**Why:**

- Respects user choice
- Clean exit
- No confusing errors

---

### 4. Spinner Pattern

**Show progress for long operations:**

```javascript
const s = spinner();

s.start("Fetching deployment block from Etherscan...");
const block = await fetchDeploymentBlock(address);
s.stop(`Start block: ${block}`);

s.start("Building subgraph...");
await buildSubgraph();
s.stop("Build complete");

s.start("Deploying to Goldsky...");
await deploy();
s.stop("Deployment complete");
```

**Why:**

- User knows something is happening
- Clear progress indication
- Prevents "is it frozen?" confusion

---

### 5. Transparent Command Execution

**Show actual commands being executed:**

```javascript
const s = spinner();
s.start(`Running: goldsky subgraph deploy ${name}/${version}`);

await spawnCommand("goldsky", ["subgraph", "deploy", `${name}/${version}`]);

s.stop("Deployment complete");
```

**Why:**

- User can reproduce manually
- Debugging is easier
- Builds trust

---

### 6. Confirmation for Destructive Operations

**Always confirm before destructive actions:**

```javascript
// Warn about production
if (tagName === "production" || tagName === "prod") {
  log.warn("⚠️  WARNING: You are about to tag a production deployment");
  log.warn("This will affect live users");
}

const confirmed = await confirm({
  message: "Are you sure you want to proceed?",
});

if (isCancel(confirmed) || !confirmed) {
  cancel("Operation cancelled");
  return;
}
```

**Why:**

- Prevents accidents
- User has time to reconsider
- Clear consequences

---

### 7. Multi-Step Workflow Pattern

**Guide users through complex workflows:**

```javascript
async function deploymentWizard() {
  intro('Goldsky Subgraph Deployment');

  // Step 1: Authentication
  log.step('Step 1: Authentication');
  const isAuthed = await checkGoldskyAuth();
  if (!isAuthed) {
    log.error('Not authenticated');
    log.info('Please run: goldsky login');
    outro('Authentication required');
    process.exit(1);
  }

  // Step 2: Network Selection
  log.step('Step 2: Network Selection');
  const network = await select({
    message: 'Select network:',
    options: [...]
  });

  // Step 3: Version
  log.step('Step 3: Version');
  const version = await text({
    message: 'Enter version:',
  });

  // Step 4: Confirmation
  log.step('Step 4: Confirmation');
  const confirmed = await confirm({
    message: 'Proceed with deployment?',
  });

  if (!confirmed) {
    cancel('Deployment cancelled');
    return;
  }

  // Step 5: Deployment
  log.step('Step 5: Deployment');
  const s = spinner();
  s.start('Deploying...');
  await deploy();
  s.stop('Complete');

  outro('Deployment successful!');
}
```

**Why:**

- Clear progress through workflow
- User knows what's coming
- Easy to follow

---

### 8. Error Handling Pattern

**Provide actionable error messages:**

```javascript
try {
  await deployToGoldsky();
} catch (error) {
  log.error(`Deployment failed: ${error.message}`);

  // Provide solutions
  if (error.message.includes("authentication")) {
    log.info("💡 Solution: Run `goldsky login` to authenticate");
  } else if (error.message.includes("network")) {
    log.info("💡 Solution: Check your internet connection");
  } else {
    log.info("💡 Solution: Check the logs for more details");
  }

  outro("Deployment failed");
  process.exit(1);
}
```

**Why:**

- User knows what went wrong
- Clear next steps
- Reduces support requests

---

### 9. Logging Pattern

**Use appropriate log levels:**

```javascript
import { log } from "@clack/prompts";

// Informational
log.info("Using network: Base Sepolia (84532)");

// Success
log.success("✅ Deployment complete");

// Warning
log.warn("⚠️  Production deployment detected");

// Error
log.error("❌ Authentication failed");

// Step indicator
log.step("Building subgraph...");

// Message (neutral)
log.message("Subgraph name: production-lottery");
```

**Why:**

- Visual hierarchy
- Easy to scan
- Clear severity

---

### 10. Dynamic Options Pattern

**Load options dynamically:**

```javascript
// Fetch available versions
const s = spinner();
s.start("Fetching available versions...");
const versions = await listSubgraphVersions(name);
s.stop(`Found ${versions.length} versions`);

// Let user select
const selectedVersion = await select({
  message: "Select version to tag:",
  options: versions.map((v) => ({
    value: v.version,
    label: v.version,
    hint: v.status,
  })),
});
```

**Why:**

- Always up-to-date options
- No hardcoded values
- Better user experience

---

## Complete Example

```javascript
#!/usr/bin/env node

import { intro, outro, text, select, confirm, spinner, log, isCancel, cancel } from "@clack/prompts";
import { checkGoldskyAuth } from "./utils/security.js";
import { spawnCommand } from "./utils/spawn.js";

async function main() {
  try {
    intro("Goldsky Subgraph Deployment");

    // Step 1: Auth check
    log.step("Checking authentication...");
    const isAuthed = await checkGoldskyAuth();
    if (!isAuthed) {
      log.error("❌ Not authenticated with Goldsky");
      log.info("Please run: goldsky login");
      outro("Authentication required");
      process.exit(1);
    }
    log.success("✅ Authenticated");

    // Step 2: Network selection
    const network = await select({
      message: "Select network:",
      options: [
        { value: "testnet", label: "Base Sepolia", hint: "Testnet (84532)" },
        { value: "mainnet", label: "Base Mainnet", hint: "Production (8453)" },
      ],
    });

    if (isCancel(network)) {
      cancel("Operation cancelled");
      return;
    }

    // Step 3: Subgraph name
    const name = await text({
      message: "Enter subgraph name:",
      placeholder: "production-lottery",
      validate: (value) => {
        if (!value) return "Name is required";
        if (!/^[a-z0-9-]+$/.test(value)) {
          return "Only lowercase letters, numbers, and hyphens allowed";
        }
      },
    });

    if (isCancel(name)) {
      cancel("Operation cancelled");
      return;
    }

    // Step 4: Version
    const version = await text({
      message: "Enter version:",
      placeholder: "1.0.0",
      validate: (value) => {
        if (!value) return "Version is required";
        if (!/^\d+\.\d+\.\d+$/.test(value)) {
          return "Version must be in format: major.minor.patch";
        }
      },
    });

    if (isCancel(version)) {
      cancel("Operation cancelled");
      return;
    }

    // Step 5: Confirmation
    log.message(`Network: ${network}`);
    log.message(`Subgraph: ${name}`);
    log.message(`Version: ${version}`);

    const shouldDeploy = await confirm({
      message: "Proceed with deployment?",
    });

    if (isCancel(shouldDeploy) || !shouldDeploy) {
      cancel("Deployment cancelled");
      return;
    }

    // Step 6: Deployment
    const s = spinner();

    s.start("Building subgraph...");
    await spawnCommand("graph", ["build"]);
    s.stop("Build complete");

    s.start(`Deploying ${name}/${version} to Goldsky...`);
    await spawnCommand("goldsky", ["subgraph", "deploy", `${name}/${version}`]);
    s.stop("Deployment complete");

    log.success("✅ Deployment successful!");
    log.info(`View at: https://goldsky.com/subgraphs/${name}`);

    outro("Done!");
  } catch (error) {
    log.error(`Error: ${error.message}`);
    outro("Deployment failed");
    process.exit(1);
  }
}

main();
```

---

## Best Practices

### Do's ✅

- Always use `intro()` and `outro()`
- Validate user input immediately
- Handle cancellation with `isCancel()`
- Show spinners for long operations
- Provide clear error messages with solutions
- Log actual commands being executed
- Confirm destructive operations
- Use appropriate log levels
- Keep prompts focused (one question at a time)
- Provide hints and placeholders

### Don'ts ❌

- Don't use Clack in non-interactive environments
- Don't skip validation
- Don't ignore cancellation
- Don't hide what's happening
- Don't use generic error messages
- Don't nest prompts deeply
- Don't forget to handle errors
- Don't use Clack for simple scripts (use CLI args)

---

## Checklist

Before shipping interactive CLI:

- [ ] Intro/outro wrapping
- [ ] Input validation on all prompts
- [ ] Cancellation handling
- [ ] Spinners for long operations
- [ ] Transparent command execution
- [ ] Confirmation for destructive ops
- [ ] Clear error messages with solutions
- [ ] Appropriate log levels
- [ ] Multi-step workflow indicators
- [ ] Graceful error handling

---

## Key Takeaways

1. **Transparency** - Always show what's happening
2. **Validation** - Validate input immediately
3. **Cancellation** - Handle user cancellation gracefully
4. **Feedback** - Provide clear, actionable feedback
5. **Confirmation** - Ask before destructive operations
6. **Progress** - Show spinners for long operations
7. **Errors** - Provide solutions, not just problems
8. **Logging** - Use appropriate log levels
9. **Workflow** - Guide users through complex processes
10. **Trust** - Build trust through transparency
