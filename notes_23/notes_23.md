# YouTube API Integration — Class 23

---

## 1. Revisit — React Component Lifecycle

Before starting the project, make sure you're comfortable with the 3 lifecycle phases:

| Phase | Key Methods | When It Runs |
|---|---|---|
| **Mounting** | `constructor()` → `render()` → `componentDidMount()` | Component first appears |
| **Updating** | `render()` → `componentDidUpdate()` | State or props change |
| **Unmounting** | `componentWillUnmount()` | Component is removed |

> 💡 For this project, `componentDidMount()` is where we will make our YouTube API call — right after the component loads for the first time.

---

## 2. Setting Up the YouTube Data API v3

Follow these steps to get your free YouTube API key from Google.

### Step 1 — Open Google Developer Console

Go to: [console.developers.google.com](https://console.developers.google.com)

### Step 2 — Create a New Project

1. Click **"Enable APIs & Services"**
2. Click **"Create Project"** (top right)
3. Give your project a name (e.g., `youtube-browser`)
4. Click **"Create"**

### Step 3 — Enable the YouTube API

1. In the search bar, search for: `YouTube Data API v3`
2. Click on the result
3. Click **"Enable"**

### Step 4 — Create an API Key

1. Go to **"Credentials"** in the left sidebar
2. Click **"Create Credentials"** → select **"API Key"**
3. A popup will show your key — **copy it immediately**
4. Click **"Done"**

> ⚠️ Keep your API key private. Never commit it directly into your code on GitHub.

### Step 5 — (Optional) Edit or Regenerate the Key

- In the Credentials page, click the **three dots (...)** next to your key
- You can restrict, edit, or regenerate the key from there

---

## 3. Finding the API Endpoint

### From the YouTube API Documentation

1. Search for **"YouTube Data API v3 Docs"** and click the official Google link
2. In the docs, find **"Search: list"**
3. The base URL for searching videos is:

```
https://www.googleapis.com/youtube/v3/search
```

This is the endpoint we'll use to search for YouTube videos.

---

## 4. Testing the API with Postman

Before writing any code, test the API directly in **Postman** to make sure it works.

### Setup in Postman

1. Open Postman
2. Create a new **Collection** — name it after your project (e.g., `YouTube Browser`)
3. Inside the collection, create a new **GET request**
4. Name the request (e.g., `Search Videos`)
5. Paste the endpoint URL:
   ```
   https://www.googleapis.com/youtube/v3/search
   ```
6. Click the **"Params"** tab and add these key-value pairs:

| Key | Value | Purpose |
|---|---|---|
| `type` | `video` | Only return video results |
| `part` | `snippet` | Include basic video info (title, thumbnail, etc.) |
| `key` | `<your API key>` | Authenticate your request |
| `q` | `tahsan` | The search query (change to anything you want) |

7. Click **"Send"**

You should receive a JSON response with a list of YouTube videos! 🎉

---

## 5. REST Methods (RESTful API)

When working with APIs, you communicate using standard **HTTP methods**. These are sometimes called **REST methods**, **RESTful services**, or **REST APIs**.

| Method | Purpose | Example Use Case |
|---|---|---|
| **GET** | Retrieve data | Fetch a list of videos from YouTube |
| **POST** | Create new data | Submit a new form, create a new user |
| **PUT** | Replace/update data | Update an entire user profile |
| **PATCH** | Partially update data | Update only a user's email address |
| **DELETE** | Remove data | Delete a post or comment |

> 💡 In this project, we only use **GET** — we are just *reading* data from YouTube, not creating or modifying anything.

---

## 6. Installing Axios

**Axios** is a popular library for making HTTP requests in JavaScript. It is much simpler to use than the native `fetch()` API.

### Install Axios

```bash
npm install axios
```

### Basic Axios GET Request

```javascript
import axios from 'axios';

axios.get('https://www.googleapis.com/youtube/v3/search', {
    params: {
        type: 'video',
        part: 'snippet',
        key: '<your API key>',
        q: 'tahsan',
    }
})
.then((response) => {
    console.log(response.data); // the YouTube search results
})
.catch((error) => {
    console.log("Error:", error);
});
```

### Axios vs `fetch()`

| | `axios` | `fetch()` |
|---|---|---|
| **Import needed** | `import axios from 'axios'` | Built-in, no import |
| **Auto JSON parsing** | ✅ Automatic | ❌ Manual (`.json()`) |
| **Request params** | ✅ Pass as `params: {}` object | ❌ Must manually build URL |
| **Error handling** | ✅ Rejects on 4xx/5xx errors | ❌ Only rejects on network failure |

> 💡 **Axios is recommended for this project** — it makes API calls much cleaner and easier to work with.

---

## 7. Template Literals

**Template literals** use backticks (`` ` ``) instead of quotes. They let you embed variables and expressions directly inside a string.

### Regular String (old way)

```javascript
const name = "Habib";
const message = "Hello, " + name + "! Welcome.";
console.log(message); // Hello, Habib! Welcome.
```

### Template Literal (new way)

```javascript
const name = "Habib";
const message = `Hello, ${name}! Welcome.`;
console.log(message); // Hello, Habib! Welcome.
```

### Multi-line Strings

```javascript
// ❌ Old way — awkward
const html = "<div>" +
             "<h1>Hello</h1>" +
             "</div>";

// ✅ Template literal — clean
const html = `
    <div>
        <h1>Hello</h1>
    </div>
`;
```

### Dynamic API URLs

Template literals are especially useful for building dynamic API URLs:

```javascript
const searchTerm = "taylor swift";
const apiKey = "YOUR_KEY_HERE";

const url = `https://www.googleapis.com/youtube/v3/search?type=video&part=snippet&key=${apiKey}&q=${searchTerm}`;
```

> 💡 The `${}` syntax inside a template literal evaluates any JavaScript expression — variables, function calls, math, etc.

---

## 8. Project Flow — Putting It All Together

```
User types a search term
        ↓
Component state updates (re-render triggered)
        ↓
componentDidUpdate() detects the state change
        ↓
axios.get() sends a GET request to YouTube API
        ↓
YouTube API returns JSON data
        ↓
setState() saves the results
        ↓
render() shows the video list on screen
```

---

## Quick Summary

| Topic | Key Point |
|---|---|
| **YouTube API** | Get a free API key from Google Developer Console |
| **Endpoint** | `https://www.googleapis.com/youtube/v3/search` |
| **Postman** | Test your API before writing code |
| **GET request** | Used to *fetch* data — the only method we need here |
| **Axios** | `npm install axios` — cleaner than `fetch()` for API calls |
| **Template literals** | Use backticks + `${}` to embed variables inside strings |
| **`componentDidMount`** | Best place to make your initial API call |