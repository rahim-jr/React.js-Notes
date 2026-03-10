# JavaScript & React — Full Topics Summary (Class 19)

This class is a comprehensive review of everything covered so far. Use this as your **master reference sheet**.

---

## JavaScript Fundamentals

### 1. Basics
The building blocks of any JavaScript program:

| Concept | Example |
|---|---|
| Variables | `var x = 10;` / `let y = 20;` / `const z = 30;` |
| Conditionals | `if (x > 5) { ... } else { ... }` |
| Loops | `for (let i = 0; i < 5; i++) { ... }` |
| Functions | `function greet() { console.log("Hello"); }` |

---

### 2. Execution Context

Every time JavaScript runs code, it creates an **Execution Context**.

**Two Parts:**

| Part | Also Called | Role |
|---|---|---|
| Memory Component | Variable Environment | Stores variables & functions |
| Code Component | Thread of Execution | Runs code line by line |

**Two Phases:**

| Phase | Also Called | What Happens |
|---|---|---|
| Creation Phase | Memory Creation Phase | Memory is allocated; variables set to `undefined` |
| Execution Phase | Code Execution Phase | Code actually runs; variables get real values |

---

### 3. Scope, Scope Chain & Lexical Environment

- **Scope** — where a variable can be accessed from
- **Lexical Environment** — the current context's memory + its parent's lexical environment
- **Scope Chain** — the chain of lookups going from child → parent → grandparent until the variable is found or a `ReferenceError` is thrown
- **Shadowing** — when an inner block declares a variable with the same name as an outer one; the inner one takes over inside that block

```javascript
let x = 10;

if (true) {
    let x = 99; // shadows outer x — only inside this block
    console.log(x); // 99
}

console.log(x); // 10 — outer x unchanged
```

---

### 4. Closure

A **closure** is formed when a function retains access to its outer (lexical) scope even after the outer function has finished running.

```javascript
function outer() {
    var secret = 42;

    function inner() {
        console.log(secret); // still accessible via closure
    }

    return inner;
}

const fn = outer();
fn(); // 42
```

> 💡 Without closures, the variable would be lost when `outer()` finishes. Closures keep it alive.

---

### 5. `let` and `const`

Introduced in **ES6 (2015)** to fix problems with `var`.

| Keyword | Scope | Can Reassign? | Hoisted? |
|---|---|---|---|
| `var` | Global / Function | ✅ Yes | ✅ Yes (as `undefined`) |
| `let` | Block | ✅ Yes | ✅ Yes (but in TDZ) |
| `const` | Block | ❌ No | ✅ Yes (but in TDZ) |

---

### 6. Temporal Dead Zone (TDZ)

The time between when a `let`/`const` variable is **hoisted** and when it is **assigned a value**. Accessing it during this window throws a `ReferenceError`.

```javascript
console.log(a); // ❌ ReferenceError: Cannot access 'a' before initialization
let a = 10;
console.log(a); // ✅ 10
```

---

### 7. Memory Scopes — The Three Types

| Scope | Created By | Accessible From |
|---|---|---|
| **Global/Local** | `var` | Anywhere in the script |
| **Script** | `let` / `const` at the top level | Throughout the whole script |
| **Block** | `let` / `const` inside `{}` | Only inside that block |

---

### 8. How the Modern Browser Works

```
Browser
├── JS Engine
│   ├── Call Stack        ← executes execution contexts one at a time
│   └── Heap              ← where objects are stored in memory
└── Web APIs              ← setTimeout, fetch, DOM, etc. (NOT JavaScript)

Event Loop
├── Callback Queue (Task Queue)   ← setTimeout, setInterval callbacks wait here
└── Microtask Queue               ← Promise callbacks, queueMicrotask() wait here
```

**How the Event Loop Works:**

```
1. JS Engine runs all synchronous code (empties the call stack)
2. Event Loop checks: is the call stack empty?
3. If YES → move items from Microtask Queue first, then Callback Queue
4. Repeat forever
```

> 💡 The **Microtask Queue** has higher priority than the Callback Queue — Promises always resolve before `setTimeout` callbacks.

---

### 9. Array Methods

| Method | What It Does | Returns |
|---|---|---|
| `map(cb)` | Transforms every item | New array (same length) |
| `forEach(cb)` | Loops through every item | `undefined` |
| `filter(cb)` | Keeps items where callback returns `true` | New array (shorter or equal) |
| `find(cb)` | Returns the first item where callback returns `true` | Single item or `undefined` |

```javascript
const nums = [1, 2, 3, 4, 5];

nums.map(n => n * 2);        // [2, 4, 6, 8, 10]
nums.filter(n => n > 3);     // [4, 5]
nums.find(n => n > 3);       // 4
nums.forEach(n => console.log(n)); // 1, 2, 3, 4, 5 (returns nothing)
```

---

### 10. Object Destructuring & Spread Operator

**Destructuring** — extract values from objects cleanly:

```javascript
const user = { name: "Habib", age: 25 };
const { name, age } = user;
console.log(name, age); // Habib 25
```

**Spread Operator (`...`)** — copy or merge objects/arrays:

```javascript
const original = { a: 1, b: 2 };
const copy     = { ...original, c: 3 };
console.log(copy); // { a: 1, b: 2, c: 3 }

const arr1 = [1, 2];
const arr2 = [3, 4];
const merged = [...arr1, ...arr2]; // [1, 2, 3, 4]
```

---

### 11. Classes, Objects, Constructor & Inheritance

```javascript
class Animal {
    constructor(name) {
        this.name = name; // 'this' = the new object being created
    }
    speak() {
        console.log(this.name + " makes a sound.");
    }
}

class Dog extends Animal { // Dog inherits from Animal
    constructor(name) {
        super(name); // calls Animal's constructor
    }
    speak() {
        console.log(this.name + " barks.");
    }
}

const dog = new Dog("Rex");
dog.speak(); // Rex barks.
```

| Keyword | Purpose |
|---|---|
| `class` | Defines a blueprint |
| `constructor()` | Runs when `new ClassName()` is called |
| `this` | Refers to the current object instance |
| `extends` | Child class inherits from parent class |
| `super()` | Calls the parent class's constructor |

---

### 12. Hoisting

**Hoisting** = variables and functions are stored in memory during the **creation phase**, before code runs.

| Type | Hoisted As |
|---|---|
| `var` | `undefined` |
| `function` declaration | The full function body |
| `let` / `const` | Hoisted, but placed in the TDZ — not accessible |

---

### 13. Function Types

| Type | Syntax | Key Feature |
|---|---|---|
| **Function Declaration** | `function greet() {}` | Hoisted fully |
| **Function Expression** | `const greet = function() {}` | Not hoisted |
| **Anonymous Function** | `function() {}` | No name |
| **Arrow Function** | `const greet = () => {}` | No own `this` |
| **Callback Function** | Passed as argument to another function | Used in `map`, `filter`, `find`, etc. |
| **First Class Function** | Receives or returns another function | Basis of functional programming |

**Parameter vs Argument:**

| Term | When | Example |
|---|---|---|
| **Parameter** | Defining the function | `function add(x, y)` → `x`, `y` are parameters |
| **Argument** | Calling the function | `add(5, 10)` → `5`, `10` are arguments |

---

### 14. The `this` Keyword

`this` refers to the **object that called the method**.

```javascript
class Car {
    constructor(brand) { this.brand = brand; }
    show() { console.log(this.brand); }
}
const car = new Car("Toyota");
car.show(); // Toyota — 'this' = car
```

> ⚠️ Arrow functions do **not** have their own `this` — they inherit it from the surrounding scope. That's why they're ideal for React event handlers.

---

## Extra JavaScript Topics (For Further Study)

| Topic | Brief Description |
|---|---|
| **Promise** | Represents a value that will be available in the future |
| **Callback Hell** | Deeply nested callbacks that become unreadable |
| **async/await** | Cleaner syntax for working with Promises |
| **Prototyping** | Every object has a prototype it inherits properties from |
| **Prototype Chaining** | The chain of prototypes looked up when accessing a property |
| **call / bind / apply** | Ways to explicitly set what `this` refers to in a function |
| **Garbage Collector** | Automatically frees memory that is no longer referenced |

---

## Operating System Concepts

| Concept | Description |
|---|---|
| **Process** | A running program — loaded into RAM, executed by the CPU (Virtual Computer) |
| **Thread** | A task inside a process (Virtual Process). JavaScript has 1 thread. |
| **Context Switching** | CPU saves a process/thread's state and jumps to another |
| **RAM** | Fast memory — holds running programs and their data |
| **Hard Disk** | Slow storage — holds files and programs when not running |
| **Processor (CPU)** | Executes binary instructions |
| **Register Set** | Ultra-fast CPU memory used during execution |
| **PCB** | Process Control Block — saves the state of a process during context switching |

---

## React Concepts

| Concept | Description |
|---|---|
| **npm** | Node Package Manager — installs and manages JavaScript packages |
| **package.json** | Lists all dependencies and scripts for your project |
| **create-react-app** | Tool that sets up a full React project with zero configuration |
| **import** | Brings code from another file or package into the current file |
| **JSX** | HTML-like syntax that compiles to `React.createElement()` calls |
| **State** | An object holding data that, when changed, causes the component to re-render |
| **Props** | Data passed from a parent component to a child component (read-only) |
| **Parent Component** | A component that renders other components inside it |
| **Child Component** | A component rendered inside another component |
| **onClick** | A React event prop that runs a function when an element is clicked |

---

## How React Renders to the Browser

```
1. App.js (Parent Component)
   └── renders Box.js (Child Component)

2. When root.render(<App />) is called:
   └── React builds a Virtual DOM (a JS object representing the UI)

3. React compares the new Virtual DOM to the previous one (diffing)

4. ReactDOM updates only the parts of the real DOM that changed

5. The browser paints the updated UI
```

**Why Virtual DOM?**

Directly updating the real browser DOM is slow. React builds a lightweight copy (Virtual DOM) in memory, figures out the **minimum** changes needed, and then updates only those parts in the real DOM.

---

## Quick Reference — Everything at a Glance

```
JavaScript
├── Basics: variables, if/else, loops, functions
├── Execution Context: memory component + code component
├── Phases: creation phase + execution phase
├── Hoisting: memory allocated before code runs
├── Scope / Scope Chain / Lexical Environment
├── Closure: function remembers its outer scope
├── let / const: block-scoped, TDZ, no global leaking
├── Array Methods: map, filter, find, forEach
├── Object Destructuring & Spread Operator
├── Classes: blueprint → constructor → extends → super
└── Functions: declaration, expression, arrow, callback, IIFE

Operating System
├── Process (Virtual Computer) → runs in RAM
├── Thread (Virtual Process) → runs inside a process
└── Context Switching → PCB saves state, CPU jumps between tasks

React
├── create-react-app → sets up the project
├── index.html → <div id="root">
├── index.js → connects React to the HTML
├── App.js → main component (extends Component)
├── JSX → compiles to React.createElement()
├── State → data that triggers re-render when changed
├── Props → data passed from parent to child
└── Virtual DOM → fast UI updates via diffing
```
