# IMDb Project — Class 26

---

## 1. What We're Building

A React app that fetches and displays a list of movies from the **IMDb API**, showing details like:

| Field | Description |
|---|---|
| **Poster** | Movie cover image |
| **Id** | Unique movie identifier |
| **Rank** | IMDb ranking position |
| **Full Title** | Movie title + release year |
| **IMDb Rating** | Score out of 10 |
| **IMDb Rating Count** | Number of people who rated it |
| **Your Review** | A custom field for personal notes |

---

## 2. Getting Your IMDb API Key

### Step 1 — Register & Get a Key

1. Go to the IMDb API site: [imdb-api.com/api](https://imdb-api.com/api)
2. Create an account if you don't have one
3. Go to your **Profile** page: [imdb-api.com/Identity/Account/Manage](https://imdb-api.com/Identity/Account/Manage)
4. Copy your **API key** from the profile page

> ⚠️ Keep your API key private. Never hardcode it in a file that gets pushed to GitHub.

### Step 2 — Check Your Tickets / Usage

Visit: [imdb-api.com/Account/Tickets](https://imdb-api.com/Account/Tickets)

This shows how many API requests you've used. The free tier has a daily limit.

### Step 3 — Explore the API with Swagger

Visit: [imdb-api.com/swagger/index.html](https://imdb-api.com/swagger/index.html)

Swagger is an interactive documentation tool — you can **test API endpoints directly in the browser** without writing any code first.

---

## 3. Yarn — An Alternative to npm

This project uses **Yarn** instead of npm. Yarn is a faster, more reliable package manager that is fully compatible with npm packages.

### Install Yarn Globally

```bash
npm install -g yarn
```

### Yarn vs npm — Command Comparison

| Purpose | npm | Yarn |
|---|---|---|
| Install all packages | `npm install` | `yarn install` |
| Add a package | `npm install axios` | `yarn add axios` |
| Remove a package | `npm uninstall axios` | `yarn remove axios` |
| Run a script | `npm run start` | `yarn start` |
| Global install | `npm install -g <pkg>` | `yarn global add <pkg>` |

> 💡 Both npm and Yarn use the same `package.json` file. You can switch between them at any time on the same project.

---

## 4. Setting Up the Project

### Step 1 — Create a New React App

```bash
create-react-app imdb-project
cd imdb-project
```

### Step 2 — Install Dependencies with Yarn

```bash
yarn install
```

### Step 3 — Install Axios

```bash
yarn add axios
```

### Step 4 — Start the Dev Server

```bash
yarn start
```

---

## 5. Project Structure

```
src/
├── index.js                   ← entry point
├── App.js                     ← root component, holds state & API call
└── components/
    ├── MovieList.js            ← renders the full list of movies
    └── MovieItem.js            ← renders a single movie card
```

---

## 6. Fetching Data from the IMDb API

Use `componentDidMount()` to fetch the movie list when the app first loads.

```jsx
import React, { Component } from 'react';
import axios from 'axios';
import MovieList from './components/MovieList';

class App extends Component {

    state = {
        movies: [],
    };

    componentDidMount() {
        this.fetchTopMovies();
    }

    fetchTopMovies = () => {
        const API_KEY = process.env.REACT_APP_IMDB_KEY; // from .env file

        axios.get(`https://imdb-api.com/en/API/Top250Movies/${API_KEY}`)
            .then((response) => {
                this.setState({ movies: response.data.items });
            })
            .catch((error) => {
                console.error("API Error:", error);
            });
    }

    render() {
        return (
            <div>
                <h1>Top 250 Movies</h1>
                <MovieList movies={this.state.movies} />
            </div>
        );
    }
}

export default App;
```

> 💡 Store your API key in a `.env` file at the root of your project:
> ```
> REACT_APP_IMDB_KEY=your_key_here
> ```
> Then access it with `process.env.REACT_APP_IMDB_KEY`.

---

## 7. Rendering the Movie List

### `MovieList.js`

```jsx
import React, { Component } from 'react';
import MovieItem from './MovieItem';

class MovieList extends Component {
    render() {
        return (
            <div>
                {this.props.movies.map((movie) => (
                    <MovieItem key={movie.id} movie={movie} />
                ))}
            </div>
        );
    }
}

export default MovieList;
```

---

### `MovieItem.js`

```jsx
import React, { Component } from 'react';

class MovieItem extends Component {
    render() {
        const { image, rank, fullTitle, imDbRating, imDbRatingCount } = this.props.movie;

        return (
            <div style={{ display: 'flex', marginBottom: '20px', border: '1px solid #ccc', padding: '10px' }}>

                {/* Poster */}
                <img
                    src={image}
                    alt={fullTitle}
                    style={{ width: '100px', marginRight: '15px' }}
                />

                {/* Details */}
                <div>
                    <h3>#{rank} — {fullTitle}</h3>
                    <p>⭐ IMDb Rating: {imDbRating} / 10</p>
                    <p>👥 Rated by: {imDbRatingCount} people</p>
                </div>

            </div>
        );
    }
}

export default MovieItem;
```

---

## 8. Understanding the IMDb API Response

When the API responds, `response.data.items` is an array. Each item looks like this:

```json
{
    "id": "tt0111161",
    "rank": "1",
    "title": "The Shawshank Redemption",
    "fullTitle": "The Shawshank Redemption (1994)",
    "year": "1994",
    "image": "https://m.media-amazon.com/images/...",
    "crew": "Frank Darabont (dir.), Tim Robbins, Morgan Freeman",
    "imDbRating": "9.2",
    "imDbRatingCount": "2700000"
}
```

### Accessing the Fields

```javascript
const movie = response.data.items[0];

console.log(movie.id);              // "tt0111161"
console.log(movie.rank);            // "1"
console.log(movie.fullTitle);       // "The Shawshank Redemption (1994)"
console.log(movie.image);           // poster URL
console.log(movie.imDbRating);      // "9.2"
console.log(movie.imDbRatingCount); // "2700000"
```

---

## 9. Handling Loading & Empty States

Always handle the case where data hasn't loaded yet:

```jsx
render() {
    if (this.state.movies.length === 0) {
        return <p>Loading movies...</p>;
    }

    return (
        <div>
            <h1>Top 250 Movies</h1>
            <MovieList movies={this.state.movies} />
        </div>
    );
}
```

---

## 10. Protecting Your API Key

```
# .env  (at the root of your project — NEVER commit this to GitHub)
REACT_APP_IMDB_KEY=your_api_key_here
```

```
# .gitignore  (add this line)
.env
```

In your code:
```javascript
const API_KEY = process.env.REACT_APP_IMDB_KEY;
```

> ⚠️ React environment variables **must** start with `REACT_APP_` to be accessible in your code.

---

## Quick Summary

| Topic | Key Point |
|---|---|
| **IMDb API** | Free API — get your key from imdb-api.com/api |
| **Swagger** | Use it to explore and test API endpoints before coding |
| **Yarn** | Alternative to npm — `yarn add` instead of `npm install` |
| **API call** | Use Axios in `componentDidMount()` for the initial data fetch |
| **Response data** | Video/movie results are in `response.data.items` |
| **`.env` file** | Store your API key here — never hardcode it in source files |
| **`REACT_APP_`** | All React env variables must start with this prefix |
| **Loading state** | Always check if data exists before trying to render it |