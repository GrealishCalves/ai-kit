# Modern JavaScript Playbook

## Overview

Use modern ES6+ patterns for cleaner, more maintainable code. Avoid legacy patterns that make code harder to read and maintain.

## When to Use

- All new JavaScript/TypeScript code
- When refactoring existing code
- During code reviews
- When modernizing legacy codebases

---

## Core Patterns

### 1. Variable Declarations

❌ **Don't:**

```javascript
var x = 1;
var name = "test";
```

✅ **Do:**

```javascript
const x = 1; // Use const for values that don't change
let name = "test"; // Use let for values that change
```

**Why:**

- `const` prevents accidental reassignment
- `let` has block scope (safer than function scope)
- `var` has confusing hoisting behavior

---

### 2. Functions

❌ **Don't:**

```javascript
function add(a, b) {
  return a + b;
}

function getName() {
  return this.name;
}
```

✅ **Do:**

```javascript
const add = (a, b) => a + b;

const getName = () => name; // Arrow functions for simple returns

// Use function declarations only when you need 'this' binding
function Component() {
  this.name = "Component";
}
```

**Why:**

- Arrow functions are more concise
- Implicit return for single expressions
- Lexical `this` binding (no surprises)

---

### 3. Array Iteration

❌ **Don't:**

```javascript
// Manual for loop
for (let i = 0; i < items.length; i++) {
  console.log(items[i]);
}

// Manual accumulation
let sum = 0;
for (const item of items) {
  sum += item.value;
}

// Manual filtering
const filtered = [];
for (const item of items) {
  if (item.active) {
    filtered.push(item);
  }
}
```

✅ **Do:**

```javascript
// Use forEach for side effects
items.forEach((item) => console.log(item));

// Use reduce for accumulation
const sum = items.reduce((acc, item) => acc + item.value, 0);

// Use filter for filtering
const filtered = items.filter((item) => item.active);

// Use map for transformation
const names = items.map((item) => item.name);

// Chain methods
const activeNames = items.filter((item) => item.active).map((item) => item.name);
```

**Why:**

- More declarative (what, not how)
- Less error-prone (no index management)
- Easier to read and understand intent

---

### 4. Optional Chaining

❌ **Don't:**

```javascript
const name = obj && obj.user && obj.user.name;

if (config && config.settings && config.settings.enabled) {
  // do something
}
```

✅ **Do:**

```javascript
const name = obj?.user?.name;

if (config?.settings?.enabled) {
  // do something
}
```

**Why:**

- Shorter, cleaner syntax
- Prevents "Cannot read property of undefined" errors
- More readable

---

### 5. Nullish Coalescing

❌ **Don't:**

```javascript
const value = input || "default"; // Fails for 0, false, ''

const port = process.env.PORT || 3000; // '0' would use 3000
```

✅ **Do:**

```javascript
const value = input ?? "default"; // Only null/undefined use default

const port = process.env.PORT ?? 3000; // '0' is valid
```

**Why:**

- `||` treats 0, false, '' as falsy (often wrong)
- `??` only treats null/undefined as nullish (correct)

---

### 6. Object Destructuring

❌ **Don't:**

```javascript
function processUser(user) {
  const name = user.name;
  const email = user.email;
  const age = user.age;

  console.log(name, email, age);
}

const config = getConfig();
const host = config.host;
const port = config.port;
```

✅ **Do:**

```javascript
function processUser({ name, email, age }) {
  console.log(name, email, age);
}

const { host, port } = getConfig();

// With defaults
const { timeout = 30000 } = options;

// With renaming
const { name: userName } = user;
```

**Why:**

- Less repetitive code
- Clear parameter expectations
- Easy to add defaults

---

### 7. Array Destructuring

❌ **Don't:**

```javascript
const parts = path.split("/");
const first = parts[0];
const second = parts[1];

const temp = a;
a = b;
b = temp;
```

✅ **Do:**

```javascript
const [first, second] = path.split("/");

// Swap values
[a, b] = [b, a];

// Skip elements
const [first, , third] = array;
```

**Why:**

- Cleaner syntax
- No temporary variables needed
- More expressive

---

### 8. Template Literals

❌ **Don't:**

```javascript
const message = "Hello, " + name + "! You have " + count + " messages.";

const url = baseUrl + "/api/" + version + "/users/" + userId;
```

✅ **Do:**

```javascript
const message = `Hello, ${name}! You have ${count} messages.`;

const url = `${baseUrl}/api/${version}/users/${userId}`;

// Multi-line strings
const html = `
  <div>
    <h1>${title}</h1>
    <p>${content}</p>
  </div>
`;
```

**Why:**

- More readable
- Supports multi-line strings
- Easier to maintain

---

### 9. Spread Operator

❌ **Don't:**

```javascript
// Array concatenation
const combined = arr1.concat(arr2);

// Array copy
const copy = arr.slice();

// Object merge
const merged = Object.assign({}, obj1, obj2);

// Function arguments
fn.apply(null, args);
```

✅ **Do:**

```javascript
// Array concatenation
const combined = [...arr1, ...arr2];

// Array copy
const copy = [...arr];

// Object merge
const merged = { ...obj1, ...obj2 };

// Function arguments
fn(...args);

// Add to array
const newArray = [...oldArray, newItem];

// Override properties
const updated = { ...user, name: "New Name" };
```

**Why:**

- More intuitive syntax
- Immutable operations (safer)
- Flexible and composable

---

### 10. Default Parameters

❌ **Don't:**

```javascript
function request(url, method, timeout) {
  method = method || "GET";
  timeout = timeout || 30000;
  // ...
}
```

✅ **Do:**

```javascript
function request(url, method = "GET", timeout = 30000) {
  // ...
}

// With destructuring
function createServer({ host = "localhost", port = 3000 } = {}) {
  // ...
}
```

**Why:**

- Clearer function signature
- Handles undefined correctly
- Self-documenting

---

### 11. Object Property Shorthand

❌ **Don't:**

```javascript
const name = "John";
const age = 30;

const user = {
  name: name,
  age: age,
  getName: function () {
    return this.name;
  },
};
```

✅ **Do:**

```javascript
const name = "John";
const age = 30;

const user = {
  name,
  age,
  getName() {
    return this.name;
  },
};
```

**Why:**

- Less repetitive
- Cleaner syntax
- Easier to refactor

---

### 12. Async/Await

❌ **Don't:**

```javascript
function fetchData() {
  return fetch(url)
    .then((res) => res.json())
    .then((data) => processData(data))
    .catch((err) => handleError(err));
}

// Nested promises
fetch(url1).then((res1) => {
  return fetch(url2).then((res2) => {
    return [res1, res2];
  });
});
```

✅ **Do:**

```javascript
async function fetchData() {
  try {
    const res = await fetch(url);
    const data = await res.json();
    return processData(data);
  } catch (err) {
    handleError(err);
  }
}

// Parallel requests
const [res1, res2] = await Promise.all([fetch(url1), fetch(url2)]);
```

**Why:**

- Reads like synchronous code
- Better error handling
- Easier to debug

---

## Anti-Patterns to Avoid

### 1. Mutating Arrays/Objects

❌ **Don't:**

```javascript
const addItem = (array, item) => {
  array.push(item); // Mutates original
  return array;
};

const updateUser = (user, name) => {
  user.name = name; // Mutates original
  return user;
};
```

✅ **Do:**

```javascript
const addItem = (array, item) => [...array, item];

const updateUser = (user, name) => ({ ...user, name });
```

---

### 2. Unnecessary Conditionals

❌ **Don't:**

```javascript
const isActive = user.active === true ? true : false;

if (condition) {
  return true;
} else {
  return false;
}
```

✅ **Do:**

```javascript
const isActive = user.active;

return condition;
```

---

### 3. Verbose Array Methods

❌ **Don't:**

```javascript
const names = users.map((user) => {
  return user.name;
});

const active = users.filter((user) => {
  return user.active === true;
});
```

✅ **Do:**

```javascript
const names = users.map((user) => user.name);

const active = users.filter((user) => user.active);
```

---

## Quick Reference

| Pattern       | Old Way                   | Modern Way                         |
| ------------- | ------------------------- | ---------------------------------- |
| Variables     | `var x = 1`               | `const x = 1` or `let x = 1`       |
| Functions     | `function f() {}`         | `const f = () => {}`               |
| Iteration     | `for (let i = 0...)`      | `.map()`, `.filter()`, `.reduce()` |
| Safe Access   | `obj && obj.prop`         | `obj?.prop`                        |
| Defaults      | `x \|\| 'default'`        | `x ?? 'default'`                   |
| Strings       | `'Hello ' + name`         | `` `Hello ${name}` ``              |
| Copy Array    | `arr.slice()`             | `[...arr]`                         |
| Merge Objects | `Object.assign({}, a, b)` | `{ ...a, ...b }`                   |
| Async         | `.then().catch()`         | `async/await`                      |

---

## Checklist

Before committing code:

- [ ] No `var` declarations (use `const`/`let`)
- [ ] Arrow functions for simple functions
- [ ] Array methods instead of for loops
- [ ] Optional chaining for safe property access
- [ ] Nullish coalescing for defaults
- [ ] Template literals for strings
- [ ] Spread operator for copying/merging
- [ ] Destructuring for object/array access
- [ ] async/await for promises
- [ ] No mutations (use immutable operations)

---

## Example: Before & After

### Before (Legacy)

```javascript
function processUsers(users, filter) {
  filter = filter || "active";
  var result = [];

  for (var i = 0; i < users.length; i++) {
    var user = users[i];
    if (user && user.status && user.status === filter) {
      result.push({
        name: user.name,
        email: user.email,
      });
    }
  }

  return result;
}
```

### After (Modern)

```javascript
const processUsers = (users, filter = "active") => users.filter((user) => user?.status === filter).map(({ name, email }) => ({ name, email }));
```

**Improvements:**

- 12 lines → 4 lines (67% reduction)
- No manual iteration
- No temporary variables
- Immutable operations
- Optional chaining for safety
- Destructuring for clarity

---

## Key Takeaways

1. **Use `const` by default** - Only use `let` when reassignment is needed
2. **Prefer arrow functions** - Shorter syntax, lexical `this`
3. **Use array methods** - More declarative than loops
4. **Optional chaining** - Safer property access
5. **Nullish coalescing** - Better defaults than `||`
6. **Template literals** - Cleaner string concatenation
7. **Spread operator** - Immutable operations
8. **Destructuring** - Less repetitive code
9. **async/await** - Cleaner async code
10. **Immutability** - Avoid mutations, use spread/map/filter
