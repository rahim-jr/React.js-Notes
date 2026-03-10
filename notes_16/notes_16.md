# React Components, JSX & the Map Function — Class 16

---

## 1. What is a Component?

A **component** is a self-contained, reusable piece of UI. Think of it like a **custom HTML element** that you build yourself.

Instead of writing the same code over and over, you write it once as a component and **reuse it** as many times as you need.

### The Box Example

Imagine you need 5 identical boxes on a page. Without components, you'd copy-paste the same HTML 5 times:

```html
<!-- ❌ Without components — repetitive and hard to maintain -->
<div class="box"><button>Click</button></div>
<div class="box"><button>Click</button></div>
<div class="box"><button>Click</button></div>
<div class="box"><button>Click</button></div>
<div class="box"><button>Click</button></div>
```

With a component, you write it **once** and reuse it:

```jsx
// ✅ With components — clean and reusable
<Box />
<Box />
<Box />
<Box />
<Box />
```

> 💡 **This is the entire reason React was created** — to make UI reusable and easier to manage.

---

## 2. Creating a Component — `Box.js`

Create a new file called `Box.js` in your `src/` folder:

```jsx
import React, { Component } from 'react';

class Box extends Component {
    render() {
        return (
            <div style={{ marginLeft: '10px' }}>
                <button>Click Me</button>
            </div>
        );
    }
}

export default Box;
```

### Important CSS Rule in React

When using inline styles, CSS property names use **camelCase** (not kebab-case):

| CSS (normal) | React inline style |
|---|---|
| `margin-left` | `marginLeft` |
| `background-color` | `backgroundColor` |
| `font-size` | `fontSize` |

---

## 3. Using a Component in `App.js`

Import your `Box` component and use it multiple times:

```jsx
import React, { Component } from 'react';
import Box from './Box'; // import the Box component

class App extends Component {
    render() {
        return (
            <div>
                <Box />
                <Box />
                <Box />
                <Box />
                <Box />
            </div>
        );
    }
}

export default App;
```

Each `<Box />` is an **independent instance** of the Box component.

---

## 4. JSX — What It Really Is

JSX looks like HTML, but it is actually **JavaScript**. Babel converts every JSX element into a `React.createElement()` call.

### The Conversion

```jsx
// What you write (JSX):
<h1>Hello World</h1>

// What Babel converts it to (JavaScript):
React.createElement('h1', null, 'Hello World')
```

### `React.createElement` Syntax

```javascript
React.createElement(element, props, children)
```

| Argument | What It Is | Example |
|---|---|---|
| `element` | Tag name or component | `'h1'` or `Box` |
| `props` | Attributes/properties object | `{ className: 'title' }` or `null` |
| `children` | Content inside the element | `'Hello World'` |

### More Examples

```jsx
// JSX
<Box className="my-box" />
// Becomes:
React.createElement(Box, { className: 'my-box' }, null)

// JSX
<div>
    <h1>Hello</h1>
</div>
// Becomes:
React.createElement('div', null,
    React.createElement('h1', null, 'Hello')
)
```

> 💡 This is why you must `import React from 'react'` at the top of every JSX file — because JSX *compiles* to `React.createElement(...)` calls, which need React to be in scope.

---

## 5. Using JavaScript Inside JSX

To use a JavaScript expression inside JSX, wrap it in **curly braces `{}`**.

```jsx
const name = "Habib";

render() {
    return (
        <h1>Hello, {name}!</h1>        // ✅ variable
        <p>{2 + 2}</p>                  // ✅ expression → 4
        <p>{isActive ? "On" : "Off"}</p> // ✅ ternary
    );
}
```

> 💡 Think of `{}` as an **"escape hatch"** from JSX back into JavaScript.

---

## 6. Rendering Lists — The `map()` Function

When you have an **array** of items to display, use the `map()` function to turn each item into a JSX element.

### What is `map()`?

`map()` is a method on JavaScript arrays. It takes a **callback function**, runs it for every item in the array, and **returns a new array** of whatever the callback returned.

```javascript
const numbers = [1, 2, 3];

const doubled = numbers.map((num) => {
    return num * 2;
});

console.log(doubled); // [2, 4, 6]
```

### `map()` Signature

```javascript
array.map((item, index) => {
    // item  = the current element in the array
    // index = the position of that element (0, 1, 2...)
    return something;
})
```

> 💡 `map()` is a **first-class function** — it accepts a **callback function** as its argument.

### `map()` Always Returns a New Array

The original array is **never modified**. `map()` always creates and returns a **brand new array**.

---

## 7. Rendering a List in React

```jsx
class App extends Component {
    render() {
        const names = ["Habib", "Arif", "Iqbal"];

        return (
            <ul>
                {names.map((name, index) => (
                    <li key={index}>{name}</li>
                ))}
            </ul>
        );
    }
}
```

**Output:**
```
• Habib
• Arif
• Iqbal
```

### The `key` Prop

When rendering a list, React requires a unique **`key`** prop on each element. This helps React track which items changed.

```jsx
<li key={index}>{name}</li>
```

> ⚠️ Using `index` as a key works fine for static lists. For dynamic lists (items that can be added/removed/reordered), use a unique ID from the data instead.

---

## 8. How Arrays Work Inside JSX

In React, an **array of JSX elements** renders just like a list of elements side by side.

```jsx
const items = [<h1>One</h1>, <h1>Two</h1>, <h1>Three</h1>];

render() {
    return <div>{items}</div>;
}
```

> 💡 When you call `.map()` on an array inside JSX, React internally wraps each element in an object and renders them one by one. **An array = a list in React.**

---

## 9. Object Destructuring

**Destructuring** is a clean way to extract values from an object into separate variables.

### Without Destructuring (verbose)
```javascript
const user = { name: "Habib", age: 25 };

const name = user.name;
const age  = user.age;

console.log(name, age); // Habib 25
```

### With Destructuring (clean)
```javascript
const user = { name: "Habib", age: 25 };

const { name, age } = user; // extract in one line

console.log(name, age); // Habib 25
```

### Destructuring in Function Parameters

```javascript
const users = [
    { name: "Habib", age: 25 },
    { name: "Arif",  age: 30 },
];

users.map(({ name, age }) => {
    console.log(name, age);
});
// Habib 25
// Arif  30
```

> 💡 Destructuring props in React components is very common — it makes your code much cleaner.

---

## Quick Summary

| Concept | Key Point |
|---|---|
| **Component** | A reusable, self-contained piece of UI |
| **JSX** | Looks like HTML but compiles to `React.createElement()` calls |
| **`{}`** in JSX | Use curly braces to write JavaScript expressions inside JSX |
| **`map()`** | Transforms an array — used to render lists in React |
| **`key` prop** | Required when rendering lists — helps React track items |
| **Object destructuring** | A clean way to extract values from objects |
| **Array in JSX** | An array of JSX elements renders as a list |