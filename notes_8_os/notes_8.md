# Operating Systems & Browser Architecture — Class 8

---

## 1. How Programs Run — Binary Executables

When you run any program on your computer, here's what happens step by step:

1. Programs (executable files) are stored in **binary form** on your hard drive, line by line. Each line is either **32-bit** or **64-bit**.
2. When you run the program, its binary lines are loaded into **RAM** one by one.
3. RAM has many memory cells (just like a hard drive, but much faster). Each cell holds a 32-bit or 64-bit value.
4. The **first portion of RAM** is always reserved for system processes. Your program loads after that.

---

## 2. The Processor

The **processor (CPU)** has two main parts:

| Part | What It Does |
|---|---|
| **Processing Unit** | Actually executes the instructions (add, subtract, multiply, etc.) |
| **Register Set** | A small set of ultra-fast memory cells used during execution |

### Basic Operations the Processor Can Do
- Add, Subtract, Multiply, Divide
- AND, OR, NOT (logical operations)

### Registers

The register set contains multiple registers such as `AX`, `BX`, `CX`, `PX`, etc.

The most important register is the **Pointing Register (Program Counter)**:

- It stores the **address of the next RAM cell** to execute
- It can only hold **one address at a time**
- The Processing Unit only looks at the Pointing Register — it knows nothing else
- If the register set's cells are **32-bit**, it's a 32-bit computer. If they're **64-bit**, it's a 64-bit computer.

### Step-by-Step Execution

```
1. Program is loaded into RAM
2. Pointing Register → points to the first line in RAM
3. Processing Unit → reads and executes that line
4. Pointing Register → moves to the next line
5. Repeat until the program ends
```

> 💡 In early computers, only **one program could run at a time**. The processor finished one program before starting another.

---

## 3. What is a Process?

A **Process** is created when:
- A program is loaded into RAM
- The Pointing Register points to its starting address
- The Processing Unit begins executing it line by line

### Key Facts About Processes

- **Every program = one process** (line-by-line execution)
- **Process = Virtual Computer** — each process believes it is the only thing running on the computer
- Processes are **isolated** from each other — one process ends with a marker so the Pointing Register knows to stop
- Process lives in **RAM**

---

## 4. Running Multiple Processes — Context Switching

Modern computers can run many programs at the same time. This is done through **Context Switching**.

### How It Works

1. The Pointing Register is executing **Process A**
2. It **saves** Process A's current register state into the **PCB (Process Control Block)**
3. It **jumps** to **Process B** and starts executing it
4. Later, it saves Process B's state and jumps back to Process A
5. It reads the PCB to find out **where Process A left off** and continues from there

> 💡 Humans can't perceive anything faster than 1/10th of a second. So when the processor switches between processes rapidly, it *feels* like everything is running at the same time!

### PCB — Process Control Block

| PCB Stores | Details |
|---|---|
| Unique Process ID | Identifies each process |
| Last Register Set State | Where the process was when it was paused |

> ⚠️ The PCB saves the **register set information**, NOT the code itself.

---

## 5. Concurrency vs Parallelism

| Concept | Meaning | Example |
|---|---|---|
| **Concurrency** | One core rapidly switches between multiple processes | 1 CPU running 3 programs by taking turns |
| **Parallelism (Multi-programming)** | Multiple cores each run their own process simultaneously | 6 CPUs each running 1 program at the same time |
| **Mixed** | Some cores do concurrency, others do parallelism | 6 CPUs running 7 processes |

### Performance Example

```
Core i3 (5th gen): 6 processes × 10 crore ops/sec  = 60 crore ops/sec
Core i3 (6th gen): 6 processes × 100 crore ops/sec = 600 crore ops/sec
Core i4 (5th gen): 8 processes × 10 crore ops/sec  = 80 crore ops/sec
```

---

## 6. Threads

A **Thread** is like a lightweight process that lives *inside* a process.

- When a process is created, it gets **1 thread by default**
- If a process needs to do **multiple tasks at once**, it can create **multiple threads**
- Thread context switching is **much faster** than process context switching because less information needs to be saved

| | Process | Thread |
|---|---|---|
| Also called | Virtual Computer | Virtual Process |
| Context switch cost | High (saves full register set) | Low (saves only current line) |
| Isolation | Fully isolated | Shares memory with other threads in the same process |

### JavaScript and Threads

> 🔑 **JavaScript is a single-threaded language** — it can only do one thing at a time.

Even though it's single-threaded, JavaScript is incredibly fast because of how modern browsers handle asynchronous work (covered in Class 9).

---

## Quick Summary

```
Hard Drive → stores binary program
RAM        → loads the program to run it
Processor  → reads RAM via Pointing Register and executes code
PCB        → saves where each process left off (for context switching)
Process    → a running program (Virtual Computer)
Thread     → a task inside a process (Virtual Process)
```
