# React App Flow Deep Dive & Tooling — Class 14

---

## 1. HTML Basics (Quick Reference)

Before diving into React/JSX, let's make sure HTML fundamentals are clear.

| Term | Example | Explanation |
|---|---|---|
| **Tag** | `h1`, `p`, `img` | The name of the element type |
| **Start Tag** | `<h1>` | Opens the element |
| **End Tag** | `</h1>` | Closes the element |
| **Element** | `<h1>Hello</h1>` | Start tag + content + end tag |
| **Self-closing Tag** | `<img />` | Elements with no content |
| **Attribute / Property** | `<img src="photo.jpg" />` | Extra info added to a tag |

### Nesting — Parent, Child, Sibling

```html
<div>              <!-- parent -->
    <p>            <!-- child of div, parent of h1 and p below -->
        <h1>Hello</h1>   <!-- child of p, sibling of the next p -->
        <p>World</p>     <!-- child of p, sibling of h1 -->
    </p>
</div>
```

---

## 2. Paths — Absolute vs Relative

When importing files in JavaScript, you use paths.

| Type | Example | When to Use |
|---|---|---|
| **Absolute Path** | `/home/user/project/src/App.js` | Full path from the root of the system |
| **Relative Path** | `./App.js` or `../components/Box.js` | Path relative to the current file's location |

### Special Path Symbols

| Symbol | Meaning |
|---|---|
| `.` | Current directory |
| `..` | One directory up (parent folder) |
| `pwd` | Terminal command: prints your Present Working Directory |

> 💡 In React imports, you almost always use **relative paths**:
> ```javascript
> import App from './App';
> ```

---

## 3. Webpack & Babel

When you use `create-react-app`, two powerful tools are working behind the scenes automatically.

### Babel

**Babel** is a JavaScript **transpiler** — it converts modern JavaScript (ES6+) into older JavaScript (ES5) that all browsers can understand.

| ES6+ Feature | Babel Converts It |
|---|---|
| `const` / `let` | → `var` |
| Arrow functions `() =>` | → regular `function` |
| `async/await` | → Promise-based code |
| JSX `<App />` | → `React.createElement(App, null)` |

> 💡 Without Babel, older browsers would not understand your modern React code.

### Webpack

**Webpack** is a **module bundler** — it takes all your files (JS, CSS, images) and bundles them into a few optimized files for the browser.

> 💡 `create-react-app` sets up both Webpack and Babel for you automatically. You don't need to configure them manually.

---

## 4. `React.Fragment`

In React, a component's `render()` method can only return **one root element**. But sometimes you don't want to add an unnecessary `<div>` wrapper.

**The problem:**
```jsx
// ❌ Error — two root elements
render() {
    return (
        <h1>Title</h1>
        <p>Paragraph</p>
    );
}
```

**The solution — use `React.Fragment`:**
```jsx
// ✅ Works — Fragment wraps without adding a DOM element
render() {
    return (
        <React.Fragment>
            <h1>Title</h1>
            <p>Paragraph</p>
        </React.Fragment>
    );
}

// ✅ Shorthand version
render() {
    return (
        <>
            <h1>Title</h1>
            <p>Paragraph</p>
        </>
    );
}
```

---

## 5. React App Flow — Step by Step

Here is the complete flow of how a React app starts up:

```
npm run start
    ↓
index.html is loaded
    ↓
index.js runs (React's entry point)
    ↓
index.js finds <div id="root"> in index.html
    ↓
index.js creates a React root and calls root.render(<App />)
    ↓
App.js runs → render() returns JSX
    ↓
React injects the JSX into <div id="root">
    ↓
The user sees the UI in the browser
```

> 🔑 **React always starts from `index.js`**. Everything flows from there.

---

## 6. `index.js` — Deep Dive

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

// Step 1: Find the <div id="root"> in index.html
const div = document.getElementById('root');

// Step 2: Create a React root attached to that div
const root = ReactDOM.createRoot(div);

// Step 3: Render the App component inside the root
root.render(<App />);
```

### What Each Step Does

| Step | Code | Explanation |
|---|---|---|
| 1 | `document.getElementById('root')` | Finds the div in index.html and returns it as a JavaScript object |
| 2 | `ReactDOM.createRoot(div)` | Creates a React "root" and returns a `root` object |
| 3 | `root.render(<App />)` | Calls App's `render()` method and injects the result into the div |

> 💡 Think of `root` as an object:
> ```javascript
> root = {
>     render: function() { /* puts your UI into the DOM */ }
> }
> ```

---

## 7. `App.js` — Deep Dive

```javascript
import React, { Component } from 'react';

class App extends Component {

    // ── State ────────────────────────────────────────────────
    state = { number: 10 }; // initial state object

    // ── Methods ──────────────────────────────────────────────

    // Arrow function so 'this' always refers to the App object
    increment = () => {
        this.state.number = this.state.number + 1;
        this.setState(this.state); // tells React to re-render
    }

    decrement = () => {
        this.state.number = this.state.number - 1;
        this.setState(this.state);
    }

    // ── Render ───────────────────────────────────────────────
    render() {
        return (
            <div>
                <button onClick={this.decrement}> - </button>
                {this.state.number}
                <button onClick={this.increment}> + </button>
            </div>
        );
    }
}

export default App;
```

### How `setState` Works Internally

When you call `this.setState(updatedState)`, React does this:

```javascript
setState(updatedState) {
    this.state = updatedState; // 1. Updates the state value
    this.render();             // 2. Re-runs render() to update the UI
}
```

> ⚠️ **Never update state directly** like `this.state.number = 11` without calling `setState`. React won't know the state changed and the UI will not update.

---

## 8. Why Arrow Functions for Event Handlers?

When you pass `this.increment` to `onClick`, React calls it later — at that point, `this` can lose its reference.

Arrow functions **do not have their own `this`** — they inherit it from the class. So `this` always correctly points to the `App` object.

```jsx
// ✅ Arrow function — 'this' is always the App object
increment = () => {
    this.setState(...);  // works perfectly
}

// ❌ Regular method — 'this' can be undefined when called by React
increment() {
    this.setState(...);  // may cause: "Cannot read properties of undefined"
}
```

---

## Quick Summary

| Concept | Key Point |
|---|---|
| **Babel** | Converts ES6+ to ES5 so all browsers can run it |
| **Webpack** | Bundles all your files into optimized output |
| **`React.Fragment`** | Wraps multiple elements without adding a real DOM node |
| **App Flow** | `index.html` → `index.js` → `App.js` |
| **`state`** | An object that holds the component's data |
| **`setState()`** | The only correct way to update state — triggers a re-render |
| **Arrow functions** | Required for event handlers so `this` works correctly |