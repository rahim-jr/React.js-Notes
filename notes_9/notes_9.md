# Browser Architecture & JavaScript Threading — Class 9

---

## 1. Quick Recap — Threads vs Processes

| | Process | Thread |
|---|---|---|
| Also known as | Virtual Computer | Virtual Process |
| Context switching cost | High | Low (only saves current line) |
| Default | Every process starts with 1 thread | All work happens inside threads |

### Important Definitions

- **Process Context Switching** — switching the Pointing Register between different processes
- **Thread Context Switching** — switching between threads *inside* the same process

> 💡 Thread context switching is extremely fast because it saves very little information — just the line it was on.

---

## 2. JavaScript is Single-Threaded

- JavaScript can **never do 2 tasks at the same time**
- It has exactly **1 thread** (1 code component / thread of execution)
- Code runs **line by line**, one line at a time

### How the Thread Works

The thread talks to the processor through the **Pointing Register**.

```
Thread → Pointing Register → Processor → Executes one line
```

Inside JavaScript's **Execution Context**:

| Part | Also Called | Role |
|---|---|---|
| Memory Component | Variable Environment | Stores variables |
| Code Component | Thread of Execution | Runs code, 1 line at a time |

The code component IS the thread — it executes exactly one line at a time, just like the Pointing Register works.

> 💡 Even if there are 5 execution contexts on the call stack, only the **top one** is actively running. The rest are paused and waiting.

---

## 3. What is a Browser?

A **browser** runs on your device (computer, phone, tablet). The device has various hardware sensors and resources that programs can access.

### Device Resources (Examples)
- Camera
- Microphone
- GPS / Location
- Audio output

### Browser's Own Resources (Examples)
- Timer
- GeoLocation API
- URL / History
- Console
- LocalStorage
- And many more...

---

## 4. Web APIs

Browsers have built-in code written to access both browser and device resources. These are called **Web APIs**.

> ⚠️ Web APIs are **NOT JavaScript**. They are written in whatever language the browser is built in (usually C++).

### Common Web APIs

| Web API | What It Does |
|---|---|
| `setTimeout()` | Sets a timer |
| `document.*` | DOM manipulation |
| `fetch()` | Makes network requests |
| `localStorage` | Stores data in the browser |
| `console.log()` | Outputs to the browser console |
| `location` | Accesses the current URL |

> 💡 Web APIs enter the **memory component** before the global execution context's creation phase even starts. That's how they're always available to use.

---

## 5. The JavaScript Engine

The **JS Engine** is the heart of the browser. It is what understands and runs JavaScript code.

```
Browser
└── JS Engine          ← heart of the browser
    └── Call Stack
        └── Execution Contexts
```

### Key Facts

- JS code can **only run inside a browser's JS engine**
- Every browser company builds their own JS engine:
  - Chrome → **V8**
  - Firefox → **SpiderMonkey**
  - Safari → **JavaScriptCore**
- Because each browser has a different engine, the same JS code *may* behave slightly differently across browsers

### How `console.log()` Actually Works

When your JS code runs `console.log("Hello")`:

1. The **code component** (thread) reaches that line
2. It calls the **`console` Web API** provided by the browser
3. The browser's Web API does the actual work of printing to the console

> 💡 `console` is a **Web API**, not native JavaScript!

---

## 6. The Big Question — How Does Single-Threaded JS Do Multiple Things?

JavaScript only has 1 thread, yet it can:
- Fetch data from a server
- Show a loading spinner at the same time
- Handle a button click

**How?** → The answer is the **Event Loop**, **Callback Queue**, and **Microtask Queue** — covered in the next class.

---

## Quick Summary

```
Browser
├── Device Resources (camera, mic, GPS...)
├── Browser Resources (timer, localStorage, console...)
├── Web APIs (code to access those resources — NOT JavaScript)
└── JS Engine
    ├── Call Stack (holds execution contexts)
    ├── Heap (memory allocation)
    └── 1 Thread (executes JS code line by line)
```

> 🔑 JavaScript is single-threaded, but the **browser** is not. The browser handles timers, network requests, and more — on separate threads — and reports back to JS when they're done.