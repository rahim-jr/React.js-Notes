# React Setup & First App — Class 11

---

## 1. Prerequisites

Before creating a React app, make sure you have the following installed:

### Install Node.js
Download and install from [nodejs.org](https://nodejs.org)

After installing, verify in your terminal:
```bash
node --version
npm --version
```

### Install Git Bash
Download from [git-scm.com](https://git-scm.com)

---

## 2. Install Create React App

`create-react-app` is an official tool that sets up a React project for you automatically — no manual configuration needed.

Install it **globally** so you can use it from anywhere:

```bash
npm install -g create-react-app
```

Verify the installation:
```bash
create-react-app --version
```

> 💡 The `-g` flag means **global** — the package is available everywhere on your computer, not just in one project folder.

---

## 3. Create Your First React App

Follow these steps one by one in your terminal:

### Step 1 — Check where you are
```bash
pwd
```
This prints your **present working directory**.

### Step 2 — Navigate to Desktop
```bash
cd Desktop/
```

### Step 3 — Create a folder for React projects
```bash
mkdir react
```

### Step 4 — Go into that folder
```bash
cd react
```

### Step 5 — Create the React app
```bash
create-react-app first-app
```

> ⏳ This will take a minute — it downloads and sets up everything you need.

---

## 4. Open the App in VS Code

Navigate into your new app folder and run:
```bash
cd first-app
code .
```

> 💡 `code .` opens the **current folder** directly in VS Code.

---

## 5. Start the Development Server

```bash
npm run start
```

Your app will automatically open in the browser at:
```
http://localhost:3000
```

---

## 6. Clean Up the Project (Remove Unnecessary Files)

The default Create React App comes with extra files. Clean it down to just what you need:

### Keep only these files:

```
public/
└── index.html          ← the one HTML file for your app

src/
├── index.js            ← entry point, mounts React to the HTML
└── App.js              ← your main component
```

Delete everything else inside `public/` and `src/`.

---

## 7. How a React App Works (Project Flow)

```
index.html
└── <div id="root">     ← empty container

index.js
└── Takes your React app and injects it into <div id="root">

App.js
└── Contains your UI (what actually shows on the page)
```

### Result in the browser:

```html
<div id="root">
    <h1>Hello World</h1>   ← React dynamically puts content here
</div>
```

> 🔑 React is a **Single Page Application (SPA)** — there is only ever **1 HTML file**. React dynamically updates the content inside `<div id="root">` without reloading the page.

---

## 8. React's Key Advantage

> Instead of reloading the whole page, React only **fetches and updates the part of the page that changed**.

This saves bandwidth and makes the app feel much faster for the user.

---

## Quick Summary

| Step | Command | Purpose |
|---|---|---|
| Install CRA | `npm install -g create-react-app` | Installs the tool globally |
| Create app | `create-react-app first-app` | Sets up a new React project |
| Open in VS Code | `code .` | Opens project in editor |
| Run the app | `npm run start` | Starts the dev server at localhost:3000 |