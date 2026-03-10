# React Component Lifecycle — Class 22

---

## What is a Component Lifecycle?

Every React class component goes through a series of phases from the moment it appears on the screen to the moment it disappears. This is called the **Component Lifecycle**.

Think of it like a human life:

| Human Life Stage | React Equivalent |
|---|---|
| Born | **Mounting** — component appears on the screen |
| Living / Growing | **Updating** — component changes due to new state or props |
| Death | **Unmounting** — component is removed from the screen |

---

## The 3 Phases of a React Component

```
┌─────────────────────────────────────────────────────┐
│                  COMPONENT LIFECYCLE                │
│                                                     │
│   1. MOUNTING     2. UPDATING      3. UNMOUNTING    │
│   (Born)          (Changed)        (Removed)        │
│                                                     │
│   constructor()   render()         componentWill-   │
│   render()        componentDid-    Unmount()        │
│   componentDid-   Update()                          │
│   Mount()                                           │
└─────────────────────────────────────────────────────┘
```

---

## Phase 1 — Mounting (Born)

The **Mounting phase** happens when a component is **first added to the screen**.

These lifecycle methods run in this exact order:

### 1. `constructor(props)`

- The very first thing that runs when a component is created
- Used to set up the initial **state** and bind event handlers
- You must call `super(props)` first

```jsx
constructor(props) {
    super(props);
    this.state = { count: 0 }; // set initial state here
}
```

---

### 2. `render()`

- Runs after the constructor
- Returns the JSX that describes the UI
- React uses the returned JSX to **update the Virtual DOM**
- This is the only **required** lifecycle method

```jsx
render() {
    return (
        <div>
            <h1>Count: {this.state.count}</h1>
        </div>
    );
}
```

> ⚠️ Never call `setState()` inside `render()` — it would cause an infinite loop.

---

### 3. `componentDidMount()`

- Runs **after** the component has been rendered and added to the real DOM
- The best place to:
  - Fetch data from an API
  - Set up subscriptions or event listeners
  - Start timers

```jsx
componentDidMount() {
    console.log("Component is now on the screen!");

    // Great place for API calls
    fetch("https://api.example.com/data")
        .then(res => res.json())
        .then(data => this.setState({ data }));
}
```

> 💡 `componentDidMount` only runs **once** — right after the component first appears.

---

### Mounting Phase — Full Flow

```
new Component created
        ↓
constructor() runs  →  sets up initial state
        ↓
render() runs       →  Virtual DOM updated
        ↓
Real DOM updated    →  component visible on screen
        ↓
componentDidMount() →  safe to fetch data, start timers
```

---

## Phase 2 — Updating (Changed)

The **Updating phase** happens when a component **re-renders** because its **state** or **props** changed.

> 🔑 The updating phase is **only triggered** by:
> - `this.setState()` being called
> - The parent passing new **props** to the component

### 1. `render()`

- Runs again every time state or props change
- Produces the new JSX with the updated values
- React diffs the new Virtual DOM against the previous one and updates only what changed

```jsx
render() {
    return (
        <div>
            <h1>Count: {this.state.count}</h1> {/* shows updated count */}
        </div>
    );
}
```

---

### 2. `componentDidUpdate(prevProps, prevState)`

- Runs **after** the component has re-rendered due to a state or props change
- Receives the **previous** props and state as arguments, so you can compare them
- Good for responding to changes — e.g., fetching new data when a prop changes

```jsx
componentDidUpdate(prevProps, prevState) {
    // Only fetch if the search term actually changed
    if (prevProps.searchTerm !== this.props.searchTerm) {
        fetch(`https://api.example.com/search?q=${this.props.searchTerm}`)
            .then(res => res.json())
            .then(data => this.setState({ results: data }));
    }
}
```

> ⚠️ If you call `setState()` inside `componentDidUpdate`, always wrap it in a condition — otherwise you'll trigger an infinite update loop.

---

### Updating Phase — Full Flow

```
this.setState() called   OR   new props received
            ↓
render() runs again  →  new Virtual DOM created
            ↓
React diffs old vs new Virtual DOM
            ↓
Only changed parts updated in the real DOM
            ↓
componentDidUpdate(prevProps, prevState) runs
```

---

## Phase 3 — Unmounting (Removed)

The **Unmounting phase** happens when a component is **removed from the screen** (e.g., when you navigate away or conditionally hide it).

### 1. `componentWillUnmount()`

- Runs just **before** the component is removed from the DOM
- Used for **cleanup** — clearing timers, cancelling API calls, removing event listeners

```jsx
componentWillUnmount() {
    console.log("Component is being removed!");

    clearInterval(this.timer);       // clear any timers
    this.subscription.unsubscribe(); // cancel subscriptions
}
```

> 💡 Always clean up in `componentWillUnmount` to prevent **memory leaks**.

---

## All Three Phases — Side by Side

| Phase | Trigger | Methods (in order) | Common Use |
|---|---|---|---|
| **Mounting** | Component first appears | `constructor()` → `render()` → `componentDidMount()` | Fetch initial data, set up timers |
| **Updating** | `setState()` or new props | `render()` → `componentDidUpdate()` | Respond to changes, fetch new data |
| **Unmounting** | Component removed | `componentWillUnmount()` | Cleanup — clear timers, cancel requests |

---

## Full Example — All Lifecycle Methods Together

```jsx
import React, { Component } from 'react';

class Counter extends Component {

    // ── MOUNTING ──────────────────────────────────
    constructor(props) {
        super(props);
        this.state = { count: 0 };
        console.log("1. constructor() — component is being created");
    }

    render() {
        console.log("2. render() — building the UI");
        return (
            <div>
                <h1>Count: {this.state.count}</h1>
                <button onClick={() => this.setState({ count: this.state.count + 1 })}>
                    Increment
                </button>
            </div>
        );
    }

    componentDidMount() {
        console.log("3. componentDidMount() — component is on the screen");
        // Safe to fetch data here
    }

    // ── UPDATING ──────────────────────────────────
    componentDidUpdate(prevProps, prevState) {
        if (prevState.count !== this.state.count) {
            console.log("4. componentDidUpdate() — count changed to:", this.state.count);
        }
    }

    // ── UNMOUNTING ────────────────────────────────
    componentWillUnmount() {
        console.log("5. componentWillUnmount() — component is being removed");
        // Clean up timers, subscriptions, etc.
    }
}

export default Counter;
```

### Console Output (on first load, then one click):

```
1. constructor()        — component is being created
2. render()             — building the UI
3. componentDidMount()  — component is on the screen
--- user clicks the button ---
2. render()             — building the UI (re-render)
4. componentDidUpdate() — count changed to: 1
```

---

## Quick Summary

```
MOUNTING (Born)
    constructor()       → set up initial state
    render()            → build the UI
    componentDidMount() → fetch data, start timers

UPDATING (Changed)
    render()              → rebuild the UI with new data
    componentDidUpdate()  → respond to what changed

UNMOUNTING (Removed)
    componentWillUnmount() → cleanup (clear timers, cancel requests)
```

> 🔑 **Key Rule:** Use `componentDidMount` for setup, `componentDidUpdate` for reactions to changes, and `componentWillUnmount` for cleanup.