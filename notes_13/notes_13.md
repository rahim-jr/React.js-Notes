# Classes, Objects & React State — Class 13

---

## 1. Recap — Classes & Objects in JavaScript

A **class** is a blueprint. An **object** is a real instance created from that blueprint.

```javascript
class Arif {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    print() {
        console.log(this.name, this.age);
    }
}

const arifObject = new Arif("Arif", 30);
arifObject.print(); // Output: Arif 30
```

### What's happening here?

| Part | Explanation |
|---|---|
| `class Arif` | Defines the blueprint called `Arif` |
| `constructor(name, age)` | Runs automatically when `new Arif(...)` is called |
| `this.name = name` | Saves the argument onto the new object |
| `new Arif("Arif", 30)` | Creates a real object from the class |
| `arifObject.print()` | Calls the `print` method on that object |

---

## 2. Multiple Classes & Objects

You can have multiple classes, each with their own objects.

```javascript
class Arif {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    print() {
        console.log(this.name, this.age);
    }
}

class AnotherClass {
    constructor(message) {
        this.message = message;
    }

    print() {
        console.log(this.message);
    }
}

const arifObject = new Arif("Arif", 30);
arifObject.print(); // Output: Arif 30

const anotherObject = new AnotherClass("I am another object from a different class");
anotherObject.print(); // Output: I am another object from a different class
```

> 💡 Each class has its **own objects**. Objects from one class are completely separate from objects of another class.

---

## 3. `this` Keyword

`this` always refers to the **object that called the method**.

```javascript
class Car {
    constructor(brand) {
        this.brand = brand; // 'this' = the new Car object being created
    }

    showBrand() {
        console.log(this.brand); // 'this' = the object that called showBrand()
    }
}

const myCar = new Car("Toyota");
myCar.showBrand(); // Output: Toyota
```

> ⚠️ **Important:** Arrow functions do NOT have their own `this`. They inherit `this` from the surrounding scope. This is why arrow functions are often used for event handlers inside React class components.

---

## 4. React — `onClick` and Event Handling

In React, you can attach events (like clicks) to HTML elements using special **props** like `onClick`.

```jsx
class App extends Component {
    handleClick() {
        console.log("Button was clicked!");
    }

    render() {
        return (
            <button onClick={this.handleClick}>Click Me</button>
        );
    }
}
```

### Rules for `onClick`

| ✅ Correct | ❌ Wrong | Why |
|---|---|---|
| `onClick={this.handleClick}` | `onClick={this.handleClick()}` | Without `()` you pass the function. With `()` you call it immediately on render! |

---

## 5. Arrow Functions as Event Handlers

Regular methods inside a class **lose their `this`** when passed as a callback. The fix is to use **arrow functions**.

```jsx
class App extends Component {
    // ✅ Arrow function — 'this' always refers to the App object
    handleClick = () => {
        console.log("Clicked!", this); // 'this' works correctly
    }

    render() {
        return (
            <button onClick={this.handleClick}>Click Me</button>
        );
    }
}
```

> 💡 Because arrow functions don't have their own `this`, they borrow it from the class — which is exactly what we want.

---

## 6. `setState` — Updating the UI

In React, you **cannot** just update a variable and expect the UI to refresh. You must use `setState()`.

### Why?

React only re-renders (updates the screen) when it is **told** to. `setState()` does two things:
1. Updates the state value
2. Triggers a re-render so the UI reflects the new value

### How `setState` Works Internally

```javascript
setState(updatedState) {
    this.state = updatedState; // updates the state
    this.render();             // re-renders the component
}
```

---

## 7. Putting It All Together — Counter Example

```jsx
class App extends Component {

    state = { number: 10 }; // initial state — an object with a 'number' property

    increment = () => {
        this.state.number = this.state.number + 1; // update the value
        this.setState(this.state);                 // tell React to re-render
    }

    decrement = () => {
        this.state.number = this.state.number - 1;
        this.setState(this.state);
    }

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
```

### Step-by-Step Walkthrough

```
1. Component renders → shows: [ - ]  10  [ + ]
2. User clicks [ + ]
3. onClick fires → calls this.increment (arrow function)
4. increment updates this.state.number to 11
5. this.setState(this.state) is called
6. React re-renders the component
7. render() runs again → shows: [ - ]  11  [ + ]
```

---

## Quick Summary

| Concept | Key Point |
|---|---|
| `class` | Blueprint for creating objects |
| `constructor()` | Runs when a new object is created with `new` |
| `this` | Refers to the object that called the method |
| Arrow function | Inherits `this` from the surrounding scope — great for event handlers |
| `onClick` | Attach a function to run when a button is clicked |
| `state` | An object that holds data the component needs to display |
| `setState()` | The ONLY correct way to update state — triggers a re-render |