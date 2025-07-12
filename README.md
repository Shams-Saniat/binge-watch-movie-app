-----

# Movie Finder App

This is a React-based movie search application that allows users to discover movies and see trending searches. It leverages the TMDB (The Movie Database) API for movie data and Appwrite for tracking popular search terms.

**Live Demo:** [https://binge-watch-movie-app.vercel.app/](https://binge-watch-movie-app.vercel.app/)

## Features

  * **Search Movies**: Find movies by typing in a search term. The search is debounced to optimize API calls.
  * **Trending Movies**: See a list of the most searched movies, powered by Appwrite.
  * **Movie Details**: Each movie card displays essential information like title, rating, language, and release year.
  * **Loading Spinner**: A visual spinner indicates when movie data is being fetched.
  * **Error Handling**: Provides user-friendly messages for API call failures.

-----

## Screenshots

<img width="1919" height="973" alt="image" src="https://github.com/user-attachments/assets/7411b726-0feb-49cb-82fa-208bb5d1e93d" />


### Homepage with Trending Movies

<img width="1210" height="838" alt="image" src="https://github.com/user-attachments/assets/573f9312-ab63-49fd-93f8-174b0f565c58" />


### Search Results

<img width="1919" height="976" alt="image" src="https://github.com/user-attachments/assets/5c152a1f-fafb-4f7d-995b-bfe674b89319" />


### Loading State

<img width="1293" height="538" alt="image" src="https://github.com/user-attachments/assets/9d0410b1-d73a-413f-bcb1-f75f6700c769" />


-----

## Technologies Used

  * **React**: A JavaScript library for building user interfaces.
  * **TMDB API**: Provides comprehensive movie data.
  * **Appwrite**: A self-hosted backend-as-a-service platform used for tracking search counts.
  * **`react-use`**: A collection of useful React Hooks, specifically `useDebounce` for optimizing search input.
  * **Vite**: A fast build tool for modern web projects.

-----

## Setup and Installation

Follow these steps to get the project up and running on your local machine.

### 1\. Clone the repository

```bash
git clone [<your-repository-url>](https://github.com/Shams-Saniat/binge-watch-movie-app.git)
cd movie-finder-app
```

### 2\. Install dependencies

```bash
npm install
# or
yarn install
```

### 3\. Set up environment variables

Create a `.env.local` file in the root of your project and add the following environment variables:

```
VITE_TMDB_API_KEY=YOUR_TMDB_API_KEY
VITE_APPWRITE_PROJECT_ID=YOUR_APPWRITE_PROJECT_ID
VITE_APPWRITE_DATABASE_ID=YOUR_APPWRITE_DATABASE_ID
VITE_APPWRITE_COLLECTION_ID=YOUR_APPWRITE_COLLECTION_ID
```

  * **`VITE_TMDB_API_KEY`**: Obtain this from [The Movie Database (TMDB) API](https://www.themoviedb.org/documentation/api). You'll need to register for an account and generate an API key. The example key you provided starts with `eyJhbGciOiJIUzI1NiJ9...`, which looks like a bearer token. Ensure you're using the correct format for your API key as required by the TMDB API.

  * **Appwrite Variables**:

      * **`VITE_APPWRITE_PROJECT_ID`**: Your Appwrite project ID.
      * **`VITE_APPWRITE_DATABASE_ID`**: The ID of your Appwrite database.
      * **`VITE_APPWRITE_COLLECTION_ID`**: The ID of the collection where search terms and counts will be stored.

    To get these, you'll need to:

    1.  **Set up an Appwrite instance**: You can host it yourself or use Appwrite Cloud.
    2.  **Create a Project**: In your Appwrite console, create a new project.
    3.  **Create a Database**: Within your project, create a new database.
    4.  **Create a Collection**: Inside your database, create a new collection with the following attributes:
          * `searchTerm` (String)
          * `count` (Integer)
          * `movie_id` (String)
          * `poster_url` (String)
    5.  **Set Permissions**: Ensure your collection has appropriate read and write permissions for `any` or `users` based on your authentication setup.

### 4\. Run the development server

```bash
npm run dev
# or
yarn dev
```

This will start the development server, usually at `http://localhost:5173`.

-----

## Project Structure

```
├── public/
│   ├── hero.png
│   ├── no-movie.png
│   ├── search.svg
│   └── star.svg
├── src/
│   ├── appwrite.js              # Appwrite SDK configuration and functions for database interaction
│   ├── App.jsx                 # Main application component
│   ├── components/
│   │   ├── MovieCard.jsx       # Displays individual movie information
│   │   ├── Search.jsx          # Search input component
│   │   └── Spinner.jsx         # Loading spinner component
│   ├── index.css
│   └── main.jsx
├── .env.local                  # Environment variables
├── .gitignore
├── index.html
├── package.json
└── README.md
```

-----

## How it Works

1.  **Movie Search**: When a user types into the search bar, the `searchTerm` state updates.
2.  **Debouncing**: The `useDebounce` hook from `react-use` delays the `setDebouncedSearchTerm` call by 500ms, ensuring `fetchMovies` is not called on every keystroke.
3.  **`fetchMovies`**: This asynchronous function fetches movie data from the TMDB API.
      * If a `query` is provided (i.e., `debouncedSearchTerm` is not empty), it searches for movies.
      * Otherwise, it fetches popular movies.
      * It handles loading states and displays error messages.
      * If a search query yields results, `updateSearchCount` is called to log the search.
4.  **`updateSearchCount` (Appwrite)**:
      * Checks if the `searchTerm` already exists in the Appwrite database.
      * If it exists, increments the `count` for that term.
      * If it doesn't exist, creates a new document with the `searchTerm`, `count` (initialized to 1), `movie_id`, and `poster_url`.
5.  **`getTrendingMovies` (Appwrite)**: Fetches the top 5 most searched movie terms from Appwrite, ordered by their `count` in descending order.
6.  **`useEffect` Hooks**:
      * The first `useEffect` triggers `fetchMovies` whenever `debouncedSearchTerm` changes, initiating the movie search.
      * The second `useEffect` calls `loadTrendingMovies` once when the component mounts to display trending searches.
7.  **Rendering**:
      * The `App` component renders the header, search bar, and conditionally displays trending movies, a loading spinner, error messages, or the list of movies.
      * `MovieCard` components are used to display individual movie details.

-----

## Contributing

Feel free to fork the repository and contribute\!

-----
