# React Learning Series — Table of Contents

A complete learning path from JavaScript fundamentals to full React applications.

---

## 📚 How to Use These Notes

Each folder contains a clean `notes_XX.md` file. Read them in order — each class builds on the previous one.

---

## 🟡 Phase 1 — JavaScript Fundamentals (Classes 1–7)

**Folder:** `notes_1-7/`  
**File:** `notes_1-7.md`

| Topic | What You'll Learn |
|---|---|
| Execution Context | Memory component, code component, creation & execution phases |
| Call Stack | How function calls are stacked and popped |
| Hoisting | Memory allocation before code runs |
| Lexical Environment | How scopes are connected to their parents |
| Scope & Scope Chain | Where variables can be accessed, and how JS searches for them |
| Closure | Functions that remember their outer scope |
| Function Types | Declaration, expression, anonymous, arrow, callback, first-class, IIFE |
| `let` & `const` | Block scoping, Temporal Dead Zone, fixing `var` memory leaks |

---

## 🟠 Phase 2 — Operating Systems & Browser Architecture (Classes 8–9)

### Class 8 — `notes_8_os/notes_8.md`

| Topic | What You'll Learn |
|---|---|
| Binary Executables | How programs are stored and loaded into RAM |
| Processor & Registers | How the CPU executes code via the Pointing Register |
| Process | A running program (Virtual Computer) |
| Context Switching | How one CPU can run many programs by rapidly switching |
| PCB | How the CPU saves and restores process state |
| Concurrency vs Parallelism | Single core vs multi-core execution |
| Threads | Lightweight tasks inside a process |

### Class 9 — `notes_9/notes_9.md`

| Topic | What You'll Learn |
|---|---|
| JS is Single-Threaded | One thread, one line at a time |
| Browser Resources | Timers, LocalStorage, GeoLocation, Console, etc. |
| Web APIs | Browser-provided tools (NOT JavaScript) |
| JS Engine | V8, SpiderMonkey — the heart of the browser |
| Event Loop Preview | How single-threaded JS handles async work |

---

## 🔵 Phase 3 — React Foundations (Classes 11–18)

### Class 11 — `notes_11/notes_11.md`
> **React Setup & First App**
- Installing Node.js, Git, and `create-react-app`
- Creating your first React app step by step
- Project structure: `index.html`, `index.js`, `App.js`
- React's core advantage: Single Page Application (SPA)

### Class 12 — `notes_12/notes_12.md`
> **Project Structure & JavaScript Classes**
- `index.html`, `index.js`, `App.js`, `package.json`, `node_modules`
- How `index.js` connects React to the HTML file
- JavaScript classes, objects, constructors
- Inheritance with `extends` and `super()`

### Class 13 — `notes_13/notes_13.md`
> **Classes, Objects & React State**
- Recap of classes & objects with examples
- The `this` keyword
- `onClick` event handling in React
- Arrow functions as event handlers (why `this` works)
- `setState()` — the only correct way to update UI

### Class 14 — `notes_14/notes_14.md`
> **React App Flow Deep Dive & Tooling**
- HTML fundamentals: tags, elements, nesting
- Absolute vs relative paths, `pwd`
- Babel (transpiling ES6 → ES5) and Webpack (bundling)
- `React.Fragment` — avoid unnecessary `<div>` wrappers
- Complete app flow: `index.html` → `index.js` → `App.js`
- Deep dive into `App.js` with increment/decrement counter

### Class 15 — `notes_15/notes_15.md`
> **Props, Deployment & UI Improvements**
- Deploying with Surge (`npm run build` → `surge -p build`)
- Stopping decrement at 0 (early return / guard clause)
- Inline styles in React (camelCase property names)
- Props: passing data from parent → child
- Primitive vs non-primitive props
- Props are read-only

### Class 16 — `notes_16/notes_16.md`
> **React Components, JSX & the Map Function**
- What a component is and why React was created
- Creating a reusable `Box` component
- JSX compiles to `React.createElement()` calls
- Using JavaScript expressions inside JSX with `{}`
- The `map()` function — turning arrays into JSX lists
- The `key` prop — required when rendering lists
- Object destructuring

### Class 17 — `notes_17/notes_17.md`
> **Arrays, Objects & Map Deep Dive**
- React goals recap (SPA, bandwidth, speed)
- `node_modules` — deleting and restoring with `npm install`
- Arrays are objects — proof and explanation
- `map()` internals — how it works under the hood
- Using `map()` to render components dynamically
- Passing props through `map()`
- `map()` vs `forEach()` — which to use in JSX

### Class 18 — `notes_18/notes_18.md`
> **Array Methods & Props Deep Dive**
- `map()`, `filter()`, and `find()` — side by side
- Internal implementation of all three methods
- Real-world React use cases for each method
- Defining and receiving props
- `super(props)` in the constructor — why it's needed
- Props + `map()` for dynamic component lists

---

## 🟢 Phase 4 — React Advanced & Projects (Classes 19–27)

### Class 19 — `notes_19/notes_19.md`
> **Full JavaScript & React Summary**
- Master reference sheet covering every topic so far
- JavaScript: basics, execution context, scope, closure, let/const, TDZ, array methods, classes, hoisting, function types
- Operating system concepts summary
- React concepts summary
- How React renders to the browser (Virtual DOM)

### Class 22 — `notes_22/notes_22.md`
> **React Component Lifecycle**
- The 3 phases: Mounting, Updating, Unmounting
- `constructor()` — sets up initial state
- `render()` — builds the UI (required)
- `componentDidMount()` — best place for API calls
- `componentDidUpdate()` — responds to state/prop changes
- `componentWillUnmount()` — cleanup (timers, subscriptions)
- Full example with all lifecycle methods together

### Class 23 — `notes_23/notes_23.md`
> **YouTube API Integration**
- Getting a YouTube Data API v3 key from Google
- Testing the API in Postman before writing code
- REST methods: GET, POST, PUT, PATCH, DELETE
- Installing and using Axios for HTTP requests
- Template literals with `${}` for dynamic URLs
- Full project flow: search → API call → render results

### Class 24 — `notes_24/notes_24.md`
> **YouTube Project Practice & Axios Deep Dive**
- Hands-on practice building the YouTube project
- Full Axios reference: install, import, GET request
- Understanding the YouTube API JSON response
- Passing API data through props (App → VideoList → VideoItem)
- Common bugs and how to fix them
- Protecting your API key with `.env` files
- Debugging tips: DevTools Console & Network tabs

### Class 25 — `notes_25/notes_25.md`
> **Git & GitHub**
- What Git is (version control) vs what GitHub is (cloud storage)
- Installing Git, first-time configuration
- Key terms: repository, commit, branch, push, pull, fetch, merge
- Creating a repo on GitHub
- The basic Git workflow: change → stage → commit → push
- Branches: creating, switching, merging
- `.gitignore` — what NOT to track (`node_modules`, `.env`)

### Class 26 — `notes_26/notes_26.md`
> **IMDb Project**
- Getting an IMDb API key from imdb-api.com
- Using Swagger to explore API endpoints
- Yarn vs npm — command comparison
- Fetching and rendering top 250 movies
- `MovieList.js` and `MovieItem.js` component structure
- Understanding the IMDb API JSON response
- Handling loading states and protecting API keys

### Class 27 — `notes_27/notes_27.md`
> **Functional Components & React Hooks**
- Class components vs functional components
- Props in functional components (no `this`)
- `useState` hook — replacing `this.state` and `setState()`
- `useEffect` hook — replacing all lifecycle methods
- Dependency array: `[]`, `[value]`, or none
- Converting the YouTube project to functional components
- Rules of hooks (top-level only, React functions only)
- Common hooks reference: `useState`, `useEffect`, `useRef`, `useContext`

---

## 📎 Additional Files

| File | Contents |
|---|---|
| `extra.md` | Generator functions vs Constructor functions |

---

## 🗺️ Learning Path at a Glance

```
JavaScript Core
    ↓
How the Browser & OS Works
    ↓
React Setup & Project Structure
    ↓
Components, JSX & State
    ↓
Props, Lists & Array Methods
    ↓
Component Lifecycle
    ↓
API Integration (YouTube, IMDb)
    ↓
Version Control (Git & GitHub)
    ↓
Modern React (Functional Components & Hooks)
```
