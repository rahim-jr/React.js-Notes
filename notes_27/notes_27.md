# Functional Components & React Hooks — Class 27

---

## 1. What Are Functional Components?

So far, every component we've written has been a **class component** — a JavaScript class that extends `Component`.

A **functional component** is simply a **JavaScript function** that returns JSX. No class, no `constructor`, no `this`.

### Class Component vs Functional Component

```jsx
// ── Class Component (old way) ──────────────────────────────
import React, { Component } from 'react';

class Greeting extends Component {
    render() {
        return <h1>Hello, {this.props.name}!</h1>;
    }
}

export default Greeting;


// ── Functional Component (new way) ────────────────────────
import React from 'react';

function Greeting(props) {
    return <h1>Hello, {props.name}!</h1>;
}

export default Greeting;


// ── Functional Component with Arrow Function ───────────────
const Greeting = (props) => {
    return <h1>Hello, {props.name}!</h1>;
};

export default Greeting;
```

> 💡 Functional components are the **modern, preferred** way to write React components. They are simpler, shorter, and easier to read.

---

## 2. Why Functional Components?

| Feature | Class Component | Functional Component |
|---|---|---|
| Syntax | Verbose (class, constructor, render, this) | Simple (just a function) |
| `this` keyword | Required everywhere | ❌ Not needed |
| State | `this.state` + `this.setState()` | `useState()` hook |
| Lifecycle methods | `componentDidMount`, etc. | `useEffect()` hook |
| Code length | Longer | Much shorter |
| Readability | Harder to follow | Easier to read |
| Performance | Slightly heavier | Slightly lighter |

> 🔑 **React hooks** (introduced in React 16.8) gave functional components the ability to use state and lifecycle features — making class components largely unnecessary.

---

## 3. Props in Functional Components

Props work the same way, but instead of `this.props`, you just receive `props` as a function argument.

```jsx
// Class component
class Welcome extends Component {
    render() {
        return <h1>Welcome, {this.props.name}!</h1>;
    }
}

// Functional component — much cleaner
function Welcome(props) {
    return <h1>Welcome, {props.name}!</h1>;
}

// With destructuring — even cleaner
function Welcome({ name }) {
    return <h1>Welcome, {name}!</h1>;
}
```

### Using the Component

```jsx
<Welcome name="Habib" />
// Output: Welcome, Habib!
```

---

## 4. The `useState` Hook — Adding State to Functional Components

In a class component, state is managed with `this.state` and `this.setState()`.

In a functional component, you use the **`useState` hook**.

### Syntax

```javascript
const [value, setValue] = useState(initialValue);
```

| Part | What It Is |
|---|---|
| `value` | The current state value (like `this.state.count`) |
| `setValue` | A function to update the value (like `this.setState()`) |
| `initialValue` | The starting value of the state |

### Counter Example — Class vs Functional

```jsx
// ── Class Component ────────────────────────────────────────
class Counter extends Component {
    state = { count: 0 };

    increment = () => {
        this.setState({ count: this.state.count + 1 });
    }

    render() {
        return (
            <div>
                <p>{this.state.count}</p>
                <button onClick={this.increment}>+</button>
            </div>
        );
    }
}


// ── Functional Component ───────────────────────────────────
import React, { useState } from 'react';

function Counter() {
    const [count, setCount] = useState(0); // initial value = 0

    const increment = () => {
        setCount(count + 1); // update state
    };

    return (
        <div>
            <p>{count}</p>
            <button onClick={increment}>+</button>
        </div>
    );
}
```

> 💡 Notice how much shorter the functional version is — no `this`, no `setState`, no `render()`.

### Multiple State Variables

```jsx
function UserForm() {
    const [name, setName]   = useState('');
    const [age, setAge]     = useState(0);
    const [email, setEmail] = useState('');

    return (
        <div>
            <input value={name}  onChange={(e) => setName(e.target.value)}  placeholder="Name" />
            <input value={age}   onChange={(e) => setAge(e.target.value)}   placeholder="Age" />
            <input value={email} onChange={(e) => setEmail(e.target.value)} placeholder="Email" />
        </div>
    );
}
```

---

## 5. The `useEffect` Hook — Lifecycle in Functional Components

In a class component, you use lifecycle methods like `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.

In a functional component, all three are replaced by **`useEffect`**.

### Syntax

```javascript
useEffect(() => {
    // code to run

    return () => {
        // cleanup code (equivalent to componentWillUnmount)
    };
}, [dependencies]);
```

### The Dependency Array

The second argument to `useEffect` (the `[]`) controls **when** the effect runs:

| Dependency Array | When the Effect Runs |
|---|---|
| Not provided | After **every** render |
| `[]` (empty) | **Once** — after the first render only (like `componentDidMount`) |
| `[value]` | After the first render AND whenever `value` changes (like `componentDidUpdate`) |

---

### `componentDidMount` Equivalent

```jsx
// Class Component
componentDidMount() {
    console.log("Component mounted!");
    this.fetchData();
}

// Functional Component
useEffect(() => {
    console.log("Component mounted!");
    fetchData();
}, []); // ← empty array = run once on mount
```

---

### `componentDidUpdate` Equivalent

```jsx
// Class Component
componentDidUpdate(prevProps) {
    if (prevProps.searchTerm !== this.props.searchTerm) {
        this.fetchResults();
    }
}

// Functional Component
useEffect(() => {
    fetchResults();
}, [searchTerm]); // ← runs whenever 'searchTerm' changes
```

---

### `componentWillUnmount` Equivalent

```jsx
// Class Component
componentWillUnmount() {
    clearInterval(this.timer);
}

// Functional Component
useEffect(() => {
    const timer = setInterval(() => {
        console.log("tick");
    }, 1000);

    return () => {
        clearInterval(timer); // cleanup — runs when component is removed
    };
}, []);
```

---

## 6. Converting the YouTube Project to Functional Components

Here is how to convert a class component to a functional component step by step.

### Class Component Version

```jsx
class App extends Component {

    state = {
        videos: [],
        searchTerm: 'react tutorial',
    };

    componentDidMount() {
        this.searchYouTube(this.state.searchTerm);
    }

    searchYouTube = (term) => {
        axios.get('https://www.googleapis.com/youtube/v3/search', {
            params: { type: 'video', part: 'snippet', key: API_KEY, q: term }
        })
        .then((response) => {
            this.setState({ videos: response.data.items });
        });
    }

    render() {
        return (
            <div>
                <SearchBar onSearch={this.searchYouTube} />
                <VideoList videos={this.state.videos} />
            </div>
        );
    }
}
```

---

### Functional Component Version

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

const App = () => {
    const [videos,     setVideos]     = useState([]);
    const [searchTerm, setSearchTerm] = useState('react tutorial');

    // Runs once when the component mounts (like componentDidMount)
    useEffect(() => {
        searchYouTube(searchTerm);
    }, []);

    const searchYouTube = (term) => {
        axios.get('https://www.googleapis.com/youtube/v3/search', {
            params: { type: 'video', part: 'snippet', key: API_KEY, q: term }
        })
        .then((response) => {
            setVideos(response.data.items); // update state
        });
    };

    return (
        <div>
            <SearchBar onSearch={searchYouTube} />
            <VideoList videos={videos} />
        </div>
    );
};

export default App;
```

### What Changed?

| Class Component | Functional Component |
|---|---|
| `class App extends Component` | `const App = () =>` |
| `this.state = { videos: [] }` | `const [videos, setVideos] = useState([])` |
| `componentDidMount()` | `useEffect(() => { ... }, [])` |
| `this.setState({ videos })` | `setVideos(videos)` |
| `this.props.something` | `props.something` or destructured `{ something }` |
| `render() { return ... }` | Just `return ...` directly |

---

## 7. Converting a Child Component

### Class Version

```jsx
class VideoItem extends Component {
    render() {
        const { title, thumbnails } = this.props.video.snippet;
        const videoId = this.props.video.id.videoId;

        return (
            <div>
                <img src={thumbnails.medium.url} alt={title} />
                <h3>{title}</h3>
            </div>
        );
    }
}
```

### Functional Version

```jsx
const VideoItem = ({ video }) => {
    const { title, thumbnails } = video.snippet;
    const videoId = video.id.videoId;

    return (
        <div>
            <img src={thumbnails.medium.url} alt={title} />
            <h3>{title}</h3>
        </div>
    );
};
```

> 💡 Stateless components (components with no state, only props) benefit the most from being converted to functional components — they become very small and clean.

---

## 8. Rules of Hooks

When using hooks, there are two important rules you must always follow:

### Rule 1 — Only Call Hooks at the Top Level

Never call hooks inside loops, conditions, or nested functions.

```jsx
// ❌ Wrong
if (isLoggedIn) {
    const [name, setName] = useState(''); // inside a condition
}

// ✅ Correct
const [name, setName] = useState(''); // always at the top level
```

### Rule 2 — Only Call Hooks Inside React Functions

Hooks must be called inside functional components or custom hooks — not in regular JavaScript functions.

```javascript
// ❌ Wrong
function regularFunction() {
    const [count, setCount] = useState(0); // not a React component!
}

// ✅ Correct
function MyComponent() {
    const [count, setCount] = useState(0); // inside a functional component ✅
}
```

---

## 9. Common Hooks — Quick Reference

| Hook | Purpose | Replaces |
|---|---|---|
| `useState` | Add state to a functional component | `this.state` + `this.setState()` |
| `useEffect` | Handle side effects (API calls, timers, subscriptions) | `componentDidMount` + `componentDidUpdate` + `componentWillUnmount` |
| `useRef` | Access a DOM element directly | `React.createRef()` |
| `useContext` | Access shared data without prop drilling | Context API with class components |

---

## Quick Summary

```
Class Component → Functional Component

class App extends Component   →  const App = () =>
this.state                    →  useState()
this.setState()               →  the setter from useState()
componentDidMount             →  useEffect(() => { ... }, [])
componentDidUpdate            →  useEffect(() => { ... }, [dependency])
componentWillUnmount          →  useEffect(() => { return () => cleanup }, [])
this.props                    →  props (or destructured directly)
render() { return ... }       →  just return ...
```

> 🔑 **The goal:** Every class component in the YouTube project should be converted to a functional component using `useState` and `useEffect`. The result will be cleaner, shorter, and more modern code.