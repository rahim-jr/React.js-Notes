# Props, Deployment & UI Improvements — Class 15

---

## 1. Deploying a React App with Surge

**Surge** is a free and simple tool for deploying static websites (including React apps) directly from the terminal.

### Step 1 — Install Surge Globally

```bash
npm install -g surge
```

### Step 2 — Build Your React App

Before deploying, you need to create an optimized production build:

```bash
npm run build
```

This creates a `build/` folder with all your files compiled and optimized for production.

### Step 3 — Deploy to Surge

```bash
surge -p build
```

- `-p build` tells Surge to deploy the contents of the `build/` folder
- Follow the prompts (create an account or log in, confirm the domain)
- Your app will be live at a `.surge.sh` URL!

---

## 2. Exploring npm Packages

You can explore all available Node packages at:

🔗 [npmjs.com](https://npmjs.com)

Search for any package to see:
- Installation command
- Documentation
- Number of weekly downloads
- Version history

---

## 3. Stopping the Decrement Button at 0

A common improvement for the counter project: **prevent the number from going below 0**.

```jsx
decrement = () => {
    if (this.state.number === 0) return; // stop at 0, do nothing

    this.state.number = this.state.number - 1;
    this.setState(this.state);
}
```

### How It Works

```
User clicks [ - ]
    ↓
decrement() is called
    ↓
Is number === 0?
    → YES → return early, nothing happens, UI stays the same
    → NO  → subtract 1, call setState, UI updates
```

> 💡 Using `return` inside a function stops it from continuing. This is called an **early return** or **guard clause** — a clean way to handle edge cases.

---

## 4. Modifying Button Color with Inline CSS

In React, you can apply inline styles using the `style` prop. Instead of a string (like in HTML), you pass a **JavaScript object**.

```jsx
render() {
    return (
        <div>
            <button
                onClick={this.decrement}
                style={{ backgroundColor: 'red', color: 'white' }}
            >
                -
            </button>

            {this.state.number}

            <button
                onClick={this.increment}
                style={{ backgroundColor: 'green', color: 'white' }}
            >
                +
            </button>
        </div>
    );
}
```

### CSS Property Naming in React

In HTML, CSS properties use **kebab-case** (`background-color`).
In React inline styles, they use **camelCase** (`backgroundColor`).

| HTML/CSS | React Inline Style |
|---|---|
| `background-color` | `backgroundColor` |
| `font-size` | `fontSize` |
| `margin-left` | `marginLeft` |
| `border-radius` | `borderRadius` |

> 💡 The double curly braces `{{ }}` are not special syntax — the outer `{}` means "this is JavaScript", and the inner `{}` is the JavaScript object for the styles.

---

## 5. Props — Passing Data from Parent to Child

**Props** (short for *properties*) are how you pass data **from a parent component down to a child component**.

Think of props like **arguments you pass to a function** — the child receives them and uses them.

### Basic Example

**Parent — `App.js`**
```jsx
class App extends Component {
    render() {
        return (
            <div>
                <Greeting name="Habib" age={25} />
                <Greeting name="Arif" age={30} />
            </div>
        );
    }
}
```

**Child — `Greeting.js`**
```jsx
class Greeting extends Component {
    render() {
        return (
            <h1>Hello, {this.props.name}! You are {this.props.age} years old.</h1>
        );
    }
}
```

**Output:**
```
Hello, Habib! You are 25 years old.
Hello, Arif! You are 30 years old.
```

---

## 6. Primitive vs Non-Primitive Props

### Primitive Props (strings, numbers, booleans)

Primitive values are **copied** when passed. Changing them in the child does NOT affect the parent.

```jsx
<Counter startValue={10} label="Score" isActive={true} />
```

### Non-Primitive Props (arrays, objects)

Arrays and objects are passed **by reference** — both parent and child point to the same data in memory.

```jsx
const scores = [10, 20, 30];

<ScoreList scores={scores} />
```

Inside the child:
```jsx
class ScoreList extends Component {
    render() {
        return (
            <ul>
                {this.props.scores.map((score, index) => (
                    <li key={index}>{score}</li>
                ))}
            </ul>
        );
    }
}
```

---

## 7. Props are Read-Only

> ⚠️ **Never modify props inside a child component.** Props flow one way — from parent to child. They are read-only.

```jsx
// ❌ Wrong — never do this
this.props.name = "Someone else";

// ✅ Correct — just read them
console.log(this.props.name);
```

If the child needs to "change" something, it should tell the **parent** to update its state by calling a function passed down as a prop.

---

## Quick Summary

| Concept | Key Point |
|---|---|
| **Surge** | Free deployment tool — `npm run build` then `surge -p build` |
| **Early Return** | Use `if (condition) return;` to stop a function early |
| **Inline Styles** | Pass a JS object to `style={{ }}` using camelCase property names |
| **Props** | Data passed from parent → child. Read-only inside the child. |
| **Primitive props** | Strings, numbers, booleans — passed by value |
| **Non-primitive props** | Arrays, objects — passed by reference |