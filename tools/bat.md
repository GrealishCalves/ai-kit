## bat - Enhanced File Viewer

### **What It Does**

- View files with syntax highlighting
- Line numbers and Git integration
- Better alternative to `cat` for code inspection

### **Why Use It**

- **Syntax highlighting:** Automatic language detection and color-coded output
- **Line numbers:** Easy reference for code review and debugging
- **Git integration:** Shows file modifications in the gutter
- **Paging support:** Automatically pipes to less for large files
- **Non-printable characters:** Visualizes tabs, spaces, and line endings
- **Theme support:** Customizable color schemes for different environments

### **Essential Commands**

```bash
# Basic file viewing
bat file.ts

# View specific line range
bat --line-range 10:50 file.ts

# Show only line numbers (no decorations)
bat --style=numbers file.ts

# Plain output (no decorations, like cat)
bat --style=plain file.ts

# Show non-printable characters
bat --show-all file.ts

# Multiple files
bat file1.ts file2.ts

# With paging disabled (print all)
bat --paging=never file.ts
```

### **Code Review & Inspection**

```bash
# View file with Git changes highlighted
bat --diff file.ts

# Compare specific line ranges
bat --line-range 100:200 src/contracts/Lottery.sol

# View file with specific theme
bat --theme="Monokai Extended" file.ts

# List available themes
bat --list-themes

# View file with grid (vertical lines)
bat --style=grid,numbers file.ts

# View multiple related files
bat src/**/*.test.ts
```

### **Integration with Other Tools**

```bash
# Pipe ripgrep results to bat
rg "useState" -l | xargs bat

# View specific files from Knip output
cat temp/knip-unused.txt | xargs bat --paging=never

# Review high-confidence deletion candidates
bat temp/high-confidence.txt

# View files with context from search
rg "TODO" -l | xargs bat --line-range :50

# Compare files side by side (using diff)
diff -u file1.ts file2.ts | bat --language=diff
```

### **Configuration**

```bash
# Set default theme (add to ~/.zshrc or ~/.bashrc)
export BAT_THEME="Monokai Extended"

# Set default style
export BAT_STYLE="numbers,grid,changes"

# Set default pager
export BAT_PAGER="less -RF"
```

### **Common Use Cases**

```bash
# Quick file inspection during code review
bat src/contracts/ProductionLottery.sol

# View test files with line numbers
bat apps/hardhat/test/**/*.test.ts --style=numbers

# Check configuration files
bat .env.example tsconfig.json package.json

# View ABI files with JSON syntax highlighting
bat apps/hardhat/ignition/deployments/chain-84532/artifacts/*.json

# Review deployment scripts
bat apps/hardhat/ignition/modules/*.ts

# Inspect subgraph schema
bat apps/goldsky/schema.graphql apps/goldsky/subgraph.yaml
```

### **Advantages Over cat**

| Feature              | cat | bat |
| -------------------- | --- | --- |
| Syntax highlighting  | ❌  | ✅  |
| Line numbers         | ❌  | ✅  |
| Git integration      | ❌  | ✅  |
| Automatic paging     | ❌  | ✅  |
| Theme support        | ❌  | ✅  |
| Non-printable chars  | ❌  | ✅  |
| Language detection   | ❌  | ✅  |
| File concatenation   | ✅  | ✅  |

