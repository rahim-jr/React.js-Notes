# YouTube Project Practice & Axios Deep Dive — Class 24

---

## 1. What This Class Is About

This class is focused on **hands-on practice** with the YouTube project. The goals are:

- Reinforce everything learned so far by building
- Identify gaps in your understanding by finding your own questions
- Solve those problems yourself (with research)
- Get comfortable with the **Axios** library

> 💡 The best way to learn is to **build, break, and fix**. This class is all about that process.

---

## 2. The YouTube Project — Recap

By now, the YouTube project should have these core features:

```
User types a search query
        ↓
App sends a GET request to the YouTube API using Axios
        ↓
Results come back as JSON
        ↓
App stores results in state
        ↓
Results render as a list of video cards
```

### Project Structure

```
src/
├── index.js                  ← entry point
├── App.js                    ← root component, holds state & API call
└── components/
    ├── SearchBar.js          ← input field for search
    ├── VideoList.js          ← renders the list of videos
    └── VideoItem.js          ← renders a single video card
```

---

## 3. Axios — Full Reference

### Installation

```bash
npm install axios
```

### Import

```javascript
import axios from 'axios';
```

---

### Making a GET Request

```javascript
axios.get(url, { params: { key: value } })
    .then((response) => {
        // response.data = the actual data returned by the API
        console.log(response.data);
    })
    .catch((error) => {
        console.log("Something went wrong:", error);
    });
```

---

### Axios with the YouTube API

```javascript
import axios from 'axios';

const API_KEY = 'YOUR_API_KEY_HERE';

axios.get('https://www.googleapis.com/youtube/v3/search', {
    params: {
        type:    'video',
        part:    'snippet',
        key:     API_KEY,
        q:       'your search term',
        maxResults: 10,
    }
})
.then((response) => {
    console.log(response.data.items); // array of video results
})
.catch((error) => {
    console.error("API Error:", error);
});
```

---

### Understanding the Response

When the YouTube API responds, it gives you a JSON object. The part you care about is `response.data.items` — an array of video objects.

Each item looks like this:

```json
{
    "id": {
        "videoId": "abc123"
    },
    "snippet": {
        "title": "My Awesome Video",
        "description": "A short description of the video.",
        "thumbnails": {
            "medium": {
                "url": "https://i.ytimg.com/vi/abc123/mqdefault.jpg"
            }
        },
        "channelTitle": "My Channel"
    }
}
```

### Accessing the Data

```javascript
const videos = response.data.items;

videos.map((video) => {
    const id           = video.id.videoId;
    const title        = video.snippet.title;
    const description  = video.snippet.description;
    const thumbnail    = video.snippet.thumbnails.medium.url;
    const channel      = video.snippet.channelTitle;

    console.log(id, title, thumbnail);
});
```

---

## 4. Putting the API Call in `componentDidMount`

The best place to make the **initial** API call is inside `componentDidMount` — it runs once, right after the component appears on the screen.

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
            params: {
                type:       'video',
                part:       'snippet',
                key:        'YOUR_API_KEY_HERE',
                q:          term,
                maxResults: 10,
            }
        })
        .then((response) => {
            this.setState({ videos: response.data.items });
        })
        .catch((error) => {
            console.error("API Error:", error);
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

## 5. Passing API Data Down with Props

Once you have the video data in `App`'s state, pass it down to child components via **props**.

### `VideoList.js`

```jsx
class VideoList extends Component {
    render() {
        return (
            <div>
                {this.props.videos.map((video) => (
                    <VideoItem key={video.id.videoId} video={video} />
                ))}
            </div>
        );
    }
}
```

### `VideoItem.js`

```jsx
class VideoItem extends Component {
    render() {
        const { title, thumbnails, channelTitle } = this.props.video.snippet;
        const videoId = this.props.video.id.videoId;

        return (
            <div style={{ display: 'flex', marginBottom: '10px' }}>
                <img
                    src={thumbnails.medium.url}
                    alt={title}
                    style={{ width: '200px', marginRight: '10px' }}
                />
                <div>
                    <h3>{title}</h3>
                    <p>{channelTitle}</p>
                    <a
                        href={`https://www.youtube.com/watch?v=${videoId}`}
                        target="_blank"
                        rel="noreferrer"
                    >
                        Watch on YouTube
                    </a>
                </div>
            </div>
        );
    }
}
```

---

## 6. Common Problems & How to Fix Them

### Problem 1 — `Cannot read properties of undefined`

This usually means the data hasn't loaded yet but the component is trying to render it.

```jsx
// ❌ Problem
render() {
    return <h1>{this.state.videos[0].snippet.title}</h1>; // crashes if videos is empty
}

// ✅ Fix — check if data exists first
render() {
    if (this.state.videos.length === 0) {
        return <p>Loading...</p>;
    }
    return <h1>{this.state.videos[0].snippet.title}</h1>;
}
```

---

### Problem 2 — API key exposed in code

Never hardcode your API key directly in a file you'll push to GitHub.

```javascript
// ❌ Never do this in a public repo
const API_KEY = 'AIzaSyBXXXXXXXXXXXXXXXXXX';
```

```javascript
// ✅ Use an environment variable
// Create a .env file at the root of your project:
// REACT_APP_YOUTUBE_KEY=AIzaSyBXXXXXXXXXXXXXXXXXX

const API_KEY = process.env.REACT_APP_YOUTUBE_KEY;
```

> ⚠️ Add `.env` to your `.gitignore` file so it never gets committed.

---

### Problem 3 — `key` prop warning in the console

```
Warning: Each child in a list should have a unique "key" prop.
```

Always add a `key` prop when rendering a list:

```jsx
// ❌ Missing key
{videos.map((video) => <VideoItem video={video} />)}

// ✅ With key
{videos.map((video) => <VideoItem key={video.id.videoId} video={video} />)}
```

---

## 7. Debugging Tips

When something isn't working, follow this checklist:

```
1. Open the browser DevTools (F12)
2. Check the Console tab for errors
3. Check the Network tab — did the API request go out?
   → Did it return 200 OK or an error code?
4. Add console.log() statements to track the data flow:
   - Log the raw API response
   - Log props received in child components
   - Log state after setState
5. Check that your API key is correct and has quota remaining
```

---

## Quick Summary

| Topic | Key Point |
|---|---|
| **Axios** | `npm install axios` — cleaner HTTP requests than `fetch()` |
| **GET request** | `axios.get(url, { params: {} })` |
| **Response data** | Access with `response.data` — YouTube results are in `response.data.items` |
| **`componentDidMount`** | Best place for the initial API call |
| **Props** | Pass video data from `App` → `VideoList` → `VideoItem` |
| **`key` prop** | Use `video.id.videoId` as the unique key when mapping |
| **Debugging** | Use `console.log()`, DevTools Console, and Network tabs |
| **API key safety** | Use `.env` files — never hardcode keys in your source code |