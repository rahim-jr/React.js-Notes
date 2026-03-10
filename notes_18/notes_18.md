# Array Methods & Props Deep Dive — Class 18

---

## 1. The Three Core Array Methods

JavaScript's `Array` class has three powerful methods you'll use constantly in React:

| Method | What It Does | Returns |
|---|---|---|
| `map()` | Transforms every item in the array | A new array (same length) |
| `filter()` | Keeps only items that pass a test | A new array (shorter or equal length) |
| `find()` | Finds the first item that passes a test | A single item (or `undefined`) |

> 💡 All three methods accept a **callback function** and **never modify the original array**.

---

## 2. The `map()` Method

`map()` runs a callback on every item and returns a **new array** of the results.

### How It Works Internally

```javascript
map(callback) {
    const resultArray = [];

    for (let i = 0; i < this.length; i++) {
        const result = callback(this[i], i); // call callback with (element, index)
        resultArray.push(result);
    }

    return resultArray;
}
```

### Example

```javascript
const numbers = [10, 20, 30];

function mapCallback(number, index) {
    return number * 2;
}

const result = numbers.map(mapCallback);
console.log(result); // [20, 40, 60]
```

### In React — Rendering JSX from an Array

```javascript
const names = ["Habib", "Arif", "Iqbal"];

names.map((name, index) => {
    return <h1 key={index}>{name} is Awesome!</h1>;
});
// Returns: [<h1>Habib is Awesome!</h1>, <h1>Arif is Awesome!</h1>, <h1>Iqbal is Awesome!</h1>]
```

> 🔑 `map()` is the go-to method for rendering lists in React because it **returns a new array** of JSX elements.

---

## 3. The `filter()` Method

`filter()` runs a callback on every item. If the callback returns `true`, the item is **kept**. If it returns `false`, the item is **removed**.

### How It Works Internally

```javascript
filter(callback) {
    const resultArray = [];

    for (let i = 0; i < this.length; i++) {
        const shouldKeep = callback(this[i], i); // returns true or false

        if (shouldKeep === true) {
            resultArray.push(this[i]); // only keep items where callback returned true
        }
    }

    return resultArray;
}
```

### Example

```javascript
const numbers = [10, 20, 30, 40, 50];

function keepBigNumbers(number, index) {
    if (number > 25) return true;
    else return false;
}

const bigNumbers = numbers.filter(keepBigNumbers);
console.log(bigNumbers); // [30, 40, 50]
```

### Filtering by Index

```javascript
const numbers = [10, 20, 30, 40, 50];

// Remove the item at index 2
function removeThird(number, index) {
    if (index === 2) return false; // skip index 2
    else return true;              // keep everything else
}

const result = numbers.filter(removeThird);
console.log(result); // [10, 20, 40, 50]
```

### Real-World React Use Case

```jsx
// Remove a box from a list when the user clicks "Delete"
deleteBox = (idToRemove) => {
    const updatedBoxes = this.state.boxes.filter((box) => {
        return box.id !== idToRemove; // keep all boxes EXCEPT the one to delete
    });
    this.setState({ boxes: updatedBoxes });
}
```

---

## 4. The `find()` Method

`find()` runs a callback on every item. It returns the **first item** where the callback returns `true`. If nothing matches, it returns `undefined`.

### How It Works Internally

```javascript
find(callback) {
    for (let i = 0; i < this.length; i++) {
        const isMatch = callback(this[i], i);

        if (isMatch === true) {
            return this[i]; // return immediately — stops looking after first match
        }
    }
    // returns undefined if nothing matched
}
```

### Example

```javascript
const numbers = [10, 20, 30, 40, 50];

function findThirdItem(number, index) {
    if (index === 2) return true;
    else return false;
}

const found = numbers.find(findThirdItem);
console.log(found); // 30
```

### Finding by Value

```javascript
const users = [
    { id: 1, name: "Habib" },
    { id: 2, name: "Arif" },
    { id: 3, name: "Iqbal" },
];

const user = users.find((user) => user.id === 2);
console.log(user); // { id: 2, name: "Arif" }
```

---

## 5. Comparing All Three Methods Side by Side

```javascript
const numbers = [10, 20, 30, 40, 50];

// map() — transforms every item
const doubled = numbers.map((n) => n * 2);
console.log(doubled); // [20, 40, 60, 80, 100]

// filter() — keeps items that pass the test
const bigOnes = numbers.filter((n) => n > 25);
console.log(bigOnes); // [30, 40, 50]

// find() — returns the first item that passes the test
const firstBig = numbers.find((n) => n > 25);
console.log(firstBig); // 30 (just the first match, not an array)
```

---

## 6. Props — Passing Data Between Components

**Props** let a parent component pass data down to a child component.

### Step 1 — Define Props in the Parent (`App.js`)

```jsx
class App extends Component {
    render() {
        return (
            <div>
                <Box label="Box One"   color="red"   />
                <Box label="Box Two"   color="blue"  />
                <Box label="Box Three" color="green" />
            </div>
        );
    }
}
```

### Step 2 — Receive Props in the Child (`Box.js`)

```jsx
class Box extends Component {

    constructor(props) {
        super(props); // always pass props to super() in the constructor
    }

    render() {
        return (
            <div style={{ backgroundColor: this.props.color, padding: '10px' }}>
                <p>{this.props.label}</p>
            </div>
        );
    }
}
```

### Why `super(props)`?

When you write a `constructor` in a class component, you **must** call `super(props)` first. This calls the `Component` class's constructor and properly sets up `this.props`.

```javascript
constructor(props) {
    super(props); // ← without this, this.props would be undefined inside the constructor
}
```

> 💡 If you don't write a `constructor` at all, React handles this automatically. You only need `super(props)` if you explicitly define a constructor.

---

## 7. Props with `map()` — Dynamic Lists

The real power comes from combining `map()` with props to render lists of components dynamically:

```jsx
class App extends Component {

    state = {
        boxes: [
            { id: 1, label: "Box One",   color: "red"   },
            { id: 2, label: "Box Two",   color: "blue"  },
            { id: 3, label: "Box Three", color: "green" },
        ]
    };

    render() {
        return (
            <div>
                {this.state.boxes.map((box) => (
                    <Box
                        key={box.id}
                        label={box.label}
                        color={box.color}
                    />
                ))}
            </div>
        );
    }
}
```

### What Happens Step by Step

```
state.boxes = [item1, item2, item3]
    ↓
.map() runs once per item
    ↓
Each item becomes: <Box key={...} label={...} color={...} />
    ↓
React renders all three <Box /> components on the page
```

---

## Quick Summary

| Method | Input | Output | Use When |
|---|---|---|---|
| `map()` | Array | New array (same length) | Transform every item |
| `filter()` | Array | New array (≤ original length) | Keep only items that pass a test |
| `find()` | Array | Single item or `undefined` | Get the first item that matches |

| Props Concept | Key Point |
|---|---|
| Define props | Add them as attributes on the JSX element in the parent |
| Receive props | Access them via `this.props` in the child |
| `super(props)` | Required in constructor to properly set up `this.props` |
| Props + `map()` | The most common pattern for rendering dynamic lists in React |