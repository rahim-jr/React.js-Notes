# JavaScript Fundamentals — Classes 1 to 7

---

## 1. Execution Context

Every time JavaScript runs code, it creates an **Execution Context**. Think of it as a box where your code lives and runs.

An Execution Context has **2 parts**:

| Part | Also Called | What It Does |
|---|---|---|
| Memory Component | Variable Environment | Stores variables and functions |
| Code Component | Thread of Execution | Runs code line by line |

An Execution Context also has **2 phases**:

1. **Creation Phase** — All variables and functions are stored in memory. Variables start as `undefined`.
2. **Execution Phase** — Code actually runs. Variables get their real values.

> 💡 There is one **Global Execution Context** always created first. Every function call creates its own **Local Execution Context** on top of the call stack.

---

## 2. Call Stack

The **Call Stack** keeps track of all the execution contexts.

- When a function is **called** → its execution context is **pushed** onto the stack
- When a function **finishes** → its execution context is **popped** off the stack

```
Call Stack (top to bottom):
[ inner()         ]  ← currently running
[ outer()         ]
[ Global Context  ]  ← always at the bottom
```

---

## 3. Hoisting

**Hoisting** is what happens during the **Creation Phase** — JavaScript allocates memory for all variables and functions *before* the code runs.

- `var` variables are hoisted and set to `undefined`
- `function` declarations are hoisted **with their full body**
- `let` and `const` are hoisted but placed in the **Temporal Dead Zone** (see below)

```javascript
console.log(a); // undefined (hoisted, but no value yet)
var a = 10;
console.log(a); // 10
```

---

## 4. Lexical Environment

A **Lexical Environment** = the current Execution Context's memory + its parent's Lexical Environment.

In simple terms: a function "knows about" variables in its own scope AND in all the scopes outside it.

---

## 5. Scope & Scope Chain

**Scope** = where a variable can be accessed from.

**Scope Chain** = when a variable is not found in the current execution context, JavaScript goes up and searches in the parent's memory, then the parent's parent, and so on — forming a **chain**.

```javascript
function outer() {
    var a = 10;

    function inner() {
        console.log(a); // Found in parent scope via scope chain ✅
    }

    inner();
}
outer();
```

---

## 6. Closure

A **Closure** is formed when a function **remembers** the variables from its outer (lexical) scope, even after that outer function has finished running.

```javascript
// ✅ Closure IS formed — inner is returned, and it still has access to 'a'
function outer() {
    var a = 10;

    function inner() {
        console.log(a); // 10
    }

    return inner;
}

var x = outer();
x(); // Still prints 10 — closure kept 'a' alive!
```

### Nested Closure Example

```javascript
function outer() {
    var a = 10;
    var c = 4;

    function inner() {
        console.log(a); // 10

        function mostInner() {
            console.log(c); // 4 — found through scope chain
        }
        mostInner();
    }

    return inner;
}

var x = outer();
x();
```

> 💡 The parent's Lexical Environment lives inside the closure. Without a closure, a **ReferenceError** would occur because the variable would be lost once the outer function finishes.

---

## 7. Functions — Types & Concepts

### Function Declaration (Function Statement)
```javascript
function greet() {
    console.log("Hello!");
}
```

### Function Expression
A function **assigned to a variable**.
```javascript
const greet = function() {
    console.log("Hello!");
};
```

### Anonymous Function
A function **with no name**. Often used inside other functions.
```javascript
function() {
    console.log("I have no name!");
}
```

### Named Function Expression
```javascript
const greet = function sayHello() {
    console.log("Hello!");
};
```

### Arrow Function (ES6)
```javascript
const greet = () => {
    console.log("Hello!");
};
```

### Parameters vs Arguments

| Term | When | Example |
|---|---|---|
| **Parameter** | When *defining* a function | `function add(x, y)` → `x` and `y` are parameters |
| **Argument** | When *calling* a function | `add(5, 10)` → `5` and `10` are arguments |

> 💡 The parameter **receives** the argument when the function is called.

---

## 8. First Class Functions

A function is called a **First Class Function** when it either:
- **Receives another function as a parameter** (that received function is called a **Callback Function**)
- **Returns another function**

```javascript
// Example 1: Receiving a function as a parameter
function outer(callback) {
    callback("Hello");
}

function print(text) {
    console.log(text);
}

outer(print); // print is the callback function here

// Example 2: Returning a function
function outer() {
    return function() {
        console.log("I was returned!");
    };
}
```

### Callback Function
The function that gets **passed as an argument** to a First Class Function.
```javascript
function call(callback) {
    callback("Hello");
}

call(function print(text) {
    console.log(text); // "Hello"
});
```

---

## 9. IIFE (Immediately Invoked Function Expression)

An **IIFE** is a function that runs **immediately** after it is defined.

```javascript
(function() {
    console.log("I run immediately!");
})();
```

---

## 10. `let` and `const` (ES6)

`let` and `const` were introduced in **ES6 (2015)**. They solve problems that `var` causes.

### The Problem with `var` — Memory Leaking

```javascript
var a = 10;

if (false) {
    var p = 10; // This block never runs...
}

console.log(p); // ...but p is still undefined! (memory leak)
```

This happens because `var` is **globally hoisted** — it leaks out of blocks.

### The Solution — `let` and `const`

`let` and `const` are **block-scoped**. They stay inside the `{}` they are defined in.

```javascript
if (true) {
    let a = 10;
    console.log(a); // ✅ 10
}
console.log(a); // ❌ ReferenceError — 'a' is not accessible outside the block
```

### Temporal Dead Zone (TDZ)

The **Temporal Dead Zone** is the time between:
1. When a `let`/`const` variable is **hoisted** (creation phase)
2. When it actually gets **assigned a value** (execution phase)

Accessing a `let`/`const` variable inside its TDZ causes a `ReferenceError`.

```javascript
console.log(a); // ❌ ReferenceError: Cannot access 'a' before initialization

let a = 10;

console.log(a); // ✅ 10
```

### `const`
Works exactly like `let`, **but the value can never be reassigned**.

```javascript
const x = 10;
x = 20; // ❌ TypeError: Assignment to constant variable
```

> 💡 Always prefer `const`. Use `let` only when you know the value will change.

---

## 11. Memory Scopes — The 3 Parts

JavaScript's memory component is divided into **3 scopes**:

| Scope | Created by | Where it lives |
|---|---|---|
| **Global/Local** | `var` | Accessible everywhere |
| **Script** | `let` / `const` at the top level | Accessible throughout the script |
| **Block** | `let` / `const` inside `{}` | Only inside that block |

### Scope Examples

```javascript
// Script scope
let p = 10;

if (true) {
    // Block scope
    let a = 10;
    console.log(a); // ✅ 10

    if (true) {
        console.log(a); // ✅ 10 — accessed from parent block via scope chain
    }
}

// console.log(a); // ❌ ReferenceError — 'a' only lives in its block
```

### Variable Shadowing

When an inner block declares a variable with the **same name** as an outer variable, the inner one takes over *within that block*. It does NOT overwrite the outer one.

```javascript
let a = 5;

if (true) {
    let a = 20;     // This is a different 'a', only inside this block
    console.log(a); // 20
}

console.log(a); // 5 — outer 'a' is unchanged
```

---

## Quick Review — Key Questions

| # | Question | Answer |
|---|---|---|
| 1 | What is a Lexical Environment? | The current execution context's memory + its parent's lexical environment |
| 2 | What is Hoisting? | Memory allocation during the creation phase |
| 3 | What is a Scope Chain? | The chain of memory lookups going from child to parent contexts |
| 4 | What is the difference between parameter and argument? | Parameter receives the argument at the time of the function call |
| 5 | What is a Function Expression? | A function assigned to a variable |
| 6 | What is an Anonymous Function? | A function with no name |
| 7 | What is a Closure? | A function together with its lexical environment |
| 8 | What is a First Class Function? | A function that receives or returns another function |
| 9 | What is a Callback Function? | The function passed as an argument to a First Class Function |
| 10 | What is IIFE? | A function that is immediately invoked after being defined |
| 11 | What is the Temporal Dead Zone? | The time between `let`/`const` being hoisted and being assigned a value |