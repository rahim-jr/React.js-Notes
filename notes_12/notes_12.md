# React Project Structure & JavaScript Classes — Class 12

---

## 1. React Project Structure

After cleaning up your project (from Class 11), you should have this structure:

```
my-app/
├── public/
│   └── index.html        ← the one and only HTML file
├── src/
│   ├── index.js          ← entry point of the app
│   └── App.js            ← your main component
├── node_modules/         ← all installed packages live here
└── package.json          ← lists all dependencies
```

---

## 2. Key Files Explained

### `index.html` (inside `public/`)

This file has one important thing — a `<div>` with the id of `root`:

```html
<div id="root"></div>
```

React will inject your entire app inside this div. You never need to touch this file.

---

### `package.json`

This file lists all the libraries your project depends on. You can open it to check which React packages are installed.

> 💡 **React** and **React DOM** are two **separate libraries**:
> - `react` — the core library (components, state, logic)
> - `react-dom` — responsible for rendering React to the browser's DOM

---

### `node_modules/`

When you run `npm install`, all the packages listed in `package.json` are downloaded and stored here.

> ⚠️ Never manually edit files inside `node_modules/`. It is auto-generated.

---

### `index.js`

This is where React connects to your HTML file. It:
1. Imports React and ReactDOM
2. Imports the `App` component
3. Renders the app into `<div id="root">`

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const div = document.getElementById('root'); // grabs the <div id="root"> element
const root = ReactDOM.createRoot(div);       // creates a React root from that div

root.render(<App />);                        // renders your App component into it
```

### Step-by-step breakdown:

| Step | Code | What It Does |
|---|---|---|
| 1 | `document.getElementById('root')` | Finds the `<div id="root">` and returns it as an object |
| 2 | `ReactDOM.createRoot(div)` | Creates a React "root" attached to that div |
| 3 | `root.render(<App />)` | Tells React to render the App component inside the root |

> 💡 `root.render(<App />)` calls the `render()` method of the App component, which returns HTML elements that get injected into the page.

---

### `App.js`

This is your main component. In a **class-based** setup:

1. Import the `Component` class from `react`
2. Create an `App` class that **extends** `Component`
3. Define a `render()` method that returns JSX (the HTML-like UI)
4. Export the `App` class so `index.js` can use it

```javascript
import React, { Component } from 'react';

class App extends Component {
    render() {
        return (
            <div>
                <h1>Hello World!</h1>
            </div>
        );
    }
}

export default App;
```

---

## 3. JavaScript Classes (The Foundation of React Class Components)

Before understanding React class components, you need to understand JavaScript classes.

### What is a Class?

A **class** is a **blueprint** for creating objects. It defines what properties and methods an object will have.

```javascript
class User {
    // blueprint for a User object
}
```

### What is an Object?

An **object** is a collection of **properties** (data) and **methods** (functions).

```javascript
const user = {
    firstName: "Habibur",
    lastName: "Rahman",
    age: 25
};
```

> 💡 The `{}` on the right side of a variable assignment creates an **object**, not a block of code.

---

## 4. Constructor Function

The **constructor** is a special method that runs **automatically** when a new object is created from a class. It sets up the initial values.

```javascript
class User {
    constructor(name, age) {
        this.name = name;   // 'this' refers to the new object being created
        this.age = age;
    }

    print() {
        console.log(this.name, this.age);
    }
}

const user1 = new User("Arif", 30);
user1.print(); // Output: Arif 30

const user2 = new User("Habib", 25);
user2.print(); // Output: Habib 25
```

### Breaking It Down

| Keyword | What It Means |
|---|---|
| `class` | Defines the blueprint |
| `constructor()` | Runs when `new ClassName()` is called |
| `this` | Refers to the specific object being created |
| `new` | Creates a new object from the class |

---

## 5. Inheritance — `extends` and `super()`

A class can **inherit** from another class using the `extends` keyword. This means the child class gets all the properties and methods of the parent class.

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        console.log(this.name + " makes a sound.");
    }
}

class Dog extends Animal {
    constructor(name) {
        super(name); // calls the parent class (Animal) constructor
    }

    speak() {
        console.log(this.name + " barks.");
    }
}

const dog = new Dog("Rex");
dog.speak(); // Output: Rex barks.
```

> 💡 `super()` calls the **parent class's constructor**. In React, `App extends Component` — so `super()` calls `Component`'s constructor, giving App all of React's built-in functionality.

---

## 6. How `App extends Component` Works in React

```javascript
class App extends Component {
    // App now has access to everything Component has:
    // - render()
    // - setState()
    // - this.state
    // - lifecycle methods
    // - and more...
}
```

This is why React class components always extend `Component` — it gives them all the React superpowers.

---

## Quick Summary

```
index.html          → has <div id="root">
index.js            → connects React to that div
App.js              → your UI, exported as a class that extends Component
node_modules/       → all installed packages
package.json        → list of all dependencies

Class               → blueprint for objects
Object              → instance of a class (has properties & methods)
constructor()       → runs automatically when new object is created
extends             → child class inherits from parent class
super()             → calls the parent class constructor
```
