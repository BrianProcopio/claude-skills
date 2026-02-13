# TypeScript Code Principles Reference

## Table of Contents

1. [Early Returns Over Deep Nesting](#1-early-returns-over-deep-nesting)
2. [Clear, Descriptive Names](#2-clear-descriptive-names)
3. [Single Responsibility](#3-single-responsibility)
4. [Meaningful Comments Only](#4-meaningful-comments-only)
5. [DRY for Business Logic](#5-dry-for-business-logic)
6. [Fail Fast and Early](#6-fail-fast-and-early)
7. [No Magic Values](#7-no-magic-values)
8. [Pure Functions Over Side Effects](#8-pure-functions-over-side-effects)
9. [Readable Over Clever](#9-readable-over-clever)
10. [No Premature Optimization](#10-no-premature-optimization)

---

## 1. Early Returns Over Deep Nesting

Avoid unnecessary nesting and branching. Use guard clauses to handle error cases first, keeping the happy path at the lowest indentation level.

**Bad:**

```typescript
function process(user) {
  if (user != null) {
    if (user.hasSubscription) {
      if (user.company === 'External') {
        showExternalVersion();
      } else {
        showInternalVersion();
      }
    } else {
      throw new Error('User does not have a subscription');
    }
  } else {
    throw new Error('User is not found');
  }
}
```

**Good:**

```typescript
function processUser(user) {
  if (user == null) {
    throw new Error('User is not found');
  }

  if (!user.hasSubscription) {
    throw new Error('User does not have a subscription');
  }

  if (user.company === 'External') {
    showExternalVersion();
  }

  showInternalVersion();
}
```

---

## 2. Clear, Descriptive Names

Use unambiguous names in plain English for functions, constants, and variables. Names should convey purpose without needing a comment.

**Bad:**

```typescript
const MIN_PASSWORD = 6;

function checkPasswordLength(password) {
  return password.length >= MIN_PASSWORD;
}
```

**Good:**

```typescript
const MIN_PASSWORD_LENGTH = 6;

function isPasswordLengthValid(password) {
  return password.length >= MIN_PASSWORD_LENGTH;
}
```

---

## 3. Single Responsibility

Keep functions small and focused on one task. If a function does validation, calculation, persistence, and notification — break it apart.

**Bad:**

```typescript
function processOrder(order: Order) {
  // Validate
  if (!order.items.length) throw new Error('Empty');
  if (!order.customer) throw new Error('No customer');

  // Calculate
  let total = 0;
  for (const item of order.items) {
    total += item.price * item.quantity;
    if (item.discount) {
      total -= item.discount;
    }
  }

  // Apply tax
  const taxRate = getTaxRate(order.customer.state);
  total = total * (1 + taxRate);

  // Save
  db.orders.insert({ ...order, total });

  // Notify
  emailService.send(order.customer.email, 'Order confirmed');
}
```

**Good:**

```typescript
function processOrder(order: Order) {
  validateOrder(order);
  const total = calculateTotal(order);
  saveOrder(order, total);
  notifyCustomer(order);
}
```

---

## 4. Meaningful Comments Only

Do not add comments that restate what the code does. Only comment to explain non-obvious logic — the "why", not the "what".

**Bad:**

```typescript
// Function to check if a number is prime
function isPrime(number) {
  // Check if number is less than 2
  if (number < 2) {
    // If less than 2, not a prime number
    return false;
  }

  for (let i = 2; i <= Math.sqrt(number); i++) {
    // Check if number is divisible by i
    if (number % i === 0) {
      // If divisible, not a prime number
      return false;
    }
  }

  // After all checks, if not divisible by any i, number is prime
  return true;
}
```

**Good:**

```typescript
function isPrime(number) {
  if (number < 2) {
    return false;
  }

  // At least 1 divisor must be less than square root, so we can stop there
  for (let i = 2; i <= Math.sqrt(number); i++) {
    if (number % i === 0) {
      return false;
    }
  }

  return true;
}
```

---

## 5. DRY for Business Logic

Avoid repeating business logic. Duplicating a few lines of simple code is acceptable, but duplicated logic should be extracted.

**Bad:**

```typescript
function logLogin() {
  console.log('User logged in at ' + new Date());
}

function logLogout() {
  console.log('User logged out at ' + new Date());
}

function logSignUp() {
  console.log('User signed up at ' + new Date());
}
```

**Good:**

```typescript
function logAction(action: string) {
  console.log(`User ${action} at ${new Date()}`);
}
```

---

## 6. Fail Fast and Early

Validate inputs and throw errors at the top of a function before doing any work. Do not perform operations on potentially invalid data.

**Bad:**

```typescript
function getUppercaseInput(input) {
  const result = input?.toUpperCase?.();

  if (typeof input !== 'string' || input.trim() === '') {
    throw new Error('Invalid input');
  }

  return result;
}
```

**Good:**

```typescript
function getUppercaseInput(input) {
  if (typeof input !== 'string' || input.trim() === '') {
    throw new Error('Invalid input');
  }
  return input.toUpperCase();
}
```

---

## 7. No Magic Values

Give names to hardcoded numbers and strings whose purpose is unclear. Named constants make code self-documenting.

**Bad:**

```typescript
let price = 10;
if (transactionType === 1) {
  price *= 1.1;
}
```

**Good:**

```typescript
const TAXABLE_TRANSACTION_TYPE = 1;
const TAX_MULTIPLIER = 1.1;

let price = 10;
if (transactionType === TAXABLE_TRANSACTION_TYPE) {
  price *= TAX_MULTIPLIER;
}
```

---

## 8. Pure Functions Over Side Effects

Prefer pure functions that return values without modifying external state. Separate computation from state mutation.

**Bad:**

```typescript
let area = 0;
function calculateAndUpdateArea(radius) {
  const newArea = Math.PI * radius * radius;
  area = newArea;
  return newArea;
}
```

**Good:**

```typescript
let area = 0;

function calculateArea(radius) {
  return Math.PI * radius * radius;
}

area = calculateArea(5);
```

---

## 9. Readable Over Clever

Avoid dense one-liners that are hard to parse. Prioritize readability and debuggability over brevity.

**Bad:**

```typescript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const result = numbers.reduce((acc, n) => (n & 1 ? [...acc, n * n] : acc), []);
```

**Good:**

```typescript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const result = numbers.filter((n) => n % 2 !== 0).map((n) => n * n);
```

---

## 10. No Premature Optimization

Use the simplest, most readable approach first. Only optimize when there is a measured performance problem.

**Bad:**

```typescript
// Custom counting sort for a small array
function countingSort(arr, min, max) {
  let count = new Array(max - min + 1).fill(0);
  arr.forEach((element) => {
    count[element - min]++;
  });

  let index = 0;
  for (let i = min; i <= max; i++) {
    while (count[i - min] > 0) {
      arr[index++] = i;
      count[i - min]--;
    }
  }
  return arr;
}

const arr = [4, 2, 2, 8, 3, 3, 1];
console.log(countingSort(arr, 1, 8));
```

**Good:**

```typescript
const arr = [4, 2, 2, 8, 3, 3, 1];
arr.sort((a, b) => a - b);
```
