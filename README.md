# Intro to React

Open your existing Codespace (the one we used in the early part of the course). 

First, we need to install into the Codespace. Follow these instructions: [node.js](nodejs.md)

Once node.js is installed.

Create a new react project

```bash
npm create vite@latest example-react-app -- --template react
```

You may be asked to install the following package:

```text
create-vite@9.3.4

Ok to proceed? (y)
```

Enter `y` to continue.

It doesn't matter which linter you choose. ESLint is commonly used.

> A linter is a tool that analyses your code and warns you about potential problems before you run it.

Think of it as a spellchecker and grammar checker for code.

When asked whether to install the packages and start the server, answer `y`.

The packages will be installed and the Vite development server will start.

Under **Ports**, you should see the development server on port `5173`. You may need to make the port visible before opening it.

You should see a **Get Started** page. Vite creates a simple React application by default. Click the button and check that the count increases.

## Starting and Stopping the Vite Server

If you ever need to stop the Vite server enter Ctrl+C in the terminal

To start the server again and run your React app:

Make sure you are in the example-react-app folder (use `cd`)

Then enter:

```
npm run dev
```

## Components, Props, State and Interactivity

This walkthrough provides a practical introduction to React. For more complete information, refer to the [React documentation](https://react.dev/learn).

Open `index.html`. Apart from a single `<div id="root"></div>` element, it is an almost empty HTML page. It simply loads a JSX file:

```html
<script type="module" src="/src/main.jsx"></script>
```

Open `main.jsx`.

This file imports the React libraries and the application component.

It uses the Document Object Model (DOM) to find the `div` in `index.html` and render the React application inside it.

```js
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import "./index.css";
import App from "./App.jsx";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <App />
  </StrictMode>,
);
```

We'll leave these files unchanged and build the application in `App.jsx`.

Open `App.jsx`. It already contains the code for a simple React application.

Delete the existing code and replace it with the following:

```js
const initialFilms = [
  { id: 1, title: "The Incredibles", year: 2004, duration: 115 },
  { id: 2, title: "Spirited Away", year: 2001, duration: 125 },
  { id: 3, title: "Mean Girls", year: 2004, duration: 97 },
  { id: 4, title: "Finding Nemo", year: 2003, duration: 100 },
  { id: 5, title: "The Dark Knight", year: 2008, duration: 152 },
];

const FilmDetails = () => {
  return (
    <div>
      <h2>Jaws</h2>
      <div>Jaws was released in 1975.</div>
      <button>Delete</button>
    </div>
  );
};

function App() {
  return (
    <div className="page">
      <FilmDetails />
    </div>
  );
}

export default App;
```

This creates a simple `<FilmDetails />` component.

Components should be named using PascalCase.

Test the page in a browser.

Inspect the HTML. Notice how the component has been rendered as ordinary HTML in the page.

## Adding some CSS

Open `App.css`.

Delete its contents.

Paste in the following CSS:

```css
body {
  font-family: Arial, sans-serif;
  font-size: 16px;
  line-height: 1.5;
}

h2 {
  margin-bottom: 15px;
}

a {
  padding: 1rem;
  color: #2563a6;
}

form div {
  margin-bottom: 15px;
}

label {
  display: inline-block;
  width: 80px;
}

input {
  padding: 6px;
  font-size: 16px;
}

button {
  padding: 7px 12px;
  font-size: 15px;
  cursor: pointer;
}

.page {
  border: 1px solid black;
  padding: 1rem;
  margin: 1rem;
}
```

Add a link to the CSS file in `index.html`:

```html
<link href="/src/App.css" rel="stylesheet" type="text/css" />
```

In React, we use `className` to apply CSS classes. Notice how the `page` class has been applied to the `FilmDetails` component.

## Passing Data to a Component — [Passing props](https://react.dev/learn/passing-props-to-a-component)

Next, we'll make the component dynamic by using data from the array. Modify your code as follows:

```js
const initialFilms = [
  { id: 1, title: "The Incredibles", year: 2004, duration: 115 },
  { id: 2, title: "Spirited Away", year: 2001, duration: 125 },
  { id: 3, title: "Mean Girls", year: 2004, duration: 97 },
  { id: 4, title: "Finding Nemo", year: 2003, duration: 100 },
  { id: 5, title: "The Dark Knight", year: 2008, duration: 152 },
];

const FilmDetails = ({ film }) => {
  return (
    <div className="page">
      <h2>{film.title}</h2>
      <div>
        {film.title} was released in {film.year}.
      </div>
      <button>Delete</button>
    </div>
  );
};

function App() {
  const films = initialFilms;
  const film = films.find((film) => film.id === 1);
  return (
    <div>
      <FilmDetails film={film} />
    </div>
  );
}

export default App;
```

Test the page in the browser. The `FilmDetails` component should now display data from the first film in the array.

We call this data passed into a component a **prop**.

### A List of Films

- Add a new component called `FilmLink` (after the array has been declared):

```js
const FilmLink = ({ film }) => {
  return <a href="#">{film.title}</a>;
};
```

Modify the App component to display some film links:

```js
function App() {
  const films = initialFilms;
  const film = films.find((film) => film.id === 1);
  return (
    <div>
      <FilmLink film={films[0]} />
      <FilmLink film={films[1]} />
      <FilmLink film={films[2]} />
      <FilmDetails film={film} />
    </div>
  );
}
```

Test that this works.

It would be better to generate the `FilmLink` components in a loop.

Add a new component called `FilmList`.

[https://react.dev/learn/rendering-lists](https://react.dev/learn/rendering-lists)

```js
const FilmList = ({ films }) => {
  return (
    <div>
      <h2>Film List</h2>
      {films.map((film) => {
        return <FilmLink key={film.id} film={film} />;
      })}
    </div>
  );
};
```

This component accepts an array of films as a prop.

We use JavaScript’s `map()` method to iterate over the array and create a `<FilmLink />` component for each film.

Modify the `<App />` component to use `<FilmList />`:

```js
function App() {
  const films = initialFilms;
  const film = films.find((film) => film.id === 1);
  return (
    <div>
      <FilmList films={films} />
      <FilmDetails film={film} />
    </div>
  );
}
```

Test the app, you should now have a list of films

### Adding Interactivity — [Responding to events](https://react.dev/learn/responding-to-events)

Let's make the links interactive. Modify `FilmLink` as follows:

```js
const FilmLink = ({ film }) => {
  return (
    <a
      href="#"
      onClick={(event) => {
        event.preventDefault();
        console.log(film.id);
      }}
    >
      {film.title}
    </a>
  );
};
```

This is standard JavaScript: we've added an `onClick` event handler.

- We prevent the default hyperlink action being executed (event.preventDefault())

- Display the film ID in the console.

Often, we want the parent component to respond to an event. To do this, we pass an event handler using props.

First, modify `<App>` to add a `selectFilm` function:

This function is passed as a prop to the `<FilmList />` component.

```js
function App() {
  const films = initialFilms;
  const selectFilm = (id) => {
    console.log(id);
  };
  const film = films.find((film) => film.id === 1);
  return (
    <div>
      <FilmList films={films} onFilmSelect={selectFilm} />
      <FilmDetails film={film} />
    </div>
  );
}
```

Modify `<FilmList>` to accept the `onFilmSelect` prop and pass it on to the child `<FilmLink />` component:

```js
const FilmList = ({ films, onFilmSelect }) => {
  return (
    <div>
      <h2>Film List</h2>
      {films.map((film) => {
        return (
          <FilmLink key={film.id} film={film} onFilmSelect={onFilmSelect} />
        );
      })}
    </div>
  );
};
```

Modify `<FilmLink>` to accept the `onFilmSelect` prop and call the function when the link is clicked:

```js
const FilmLink = ({ film, onFilmSelect }) => {
  return (
    <a
      href="#"
      onClick={(event) => {
        event.preventDefault();
        onFilmSelect(film.id);
      }}
    >
      {film.title}
    </a>
  );
};
```

Test the application. When the user clicks a link, the console should display the ID of the selected film.

The `selectFilm` function has been passed from `App` to `FilmList` and then to `FilmLink`. When a link is clicked, the function is called.

We call this **prop drilling**.

### Using State — [State: a component’s memory](https://react.dev/learn/state-a-components-memory)

Import `useState` at the top of App.jsx

```js
import { useState } from "react";
```

Modify `App` as follows:

- Declare a state variable `const [chosenFilm, setChosenFilm] = useState(1);`
- Set the value of this variable when a <FilmLink> is clicked
- Use the chosenFilm state variable in the `find` method.

```js
function App() {
  const films = initialFilms;
  const [chosenFilm, setChosenFilm] = useState(1);

  const selectFilm = (id) => {
    setChosenFilm(id);
  };

  const film = films.find((film) => film.id === chosenFilm);

  return (
    <div>
      <FilmList films={films} onFilmSelect={selectFilm} />
      <FilmDetails film={film} />
    </div>
  );
}
```

- The user should now be able to click on different films and the FilmDetails component should update.

## Adding a Create Component

Add a new component called `CreateFilm` (after the `FilmDetails` component):

```js
const CreateFilm = () => {
  const handleSubmit = (event) => {
    event.preventDefault();
    const formData = new FormData(event.target);
    const newFilm = {
      title: formData.get("title"),
      year: Number(formData.get("year")),
      duration: Number(formData.get("duration")),
    };
    console.log(newFilm);
  };
  return (
    <div className="page">
      <h2>Add a new film</h2>
      <form onSubmit={handleSubmit}>
        <div>
          <label htmlFor="title">Title:</label>
          <input type="text" id="title" name="title" />
        </div>
        <div>
          <label htmlFor="year">Year:</label>
          <input type="number" id="year" name="year" />
        </div>
        <div>
          <label htmlFor="duration">Duration:</label>
          <input type="number" id="duration" name="duration" />
        </div>
        <div>
          <button type="submit">SAVE THE FILM</button>
        </div>
      </form>
    </div>
  );
};
```

Modify the `App` component to make use of `CreateFilm`:

```js
function App() {
  const films = initialFilms;
  const [chosenFilm, setChosenFilm] = useState(1);

  const selectFilm = (id) => {
    setChosenFilm(id);
  };

  const film = films.find((film) => film.id === chosenFilm);

  return (
    <div>
      <FilmList films={films} onFilmSelect={selectFilm} />
      <FilmDetails film={film} />
      <CreateFilm />
    </div>
  );
}
```

Test that this works. A form for adding a new film should now appear on the page.

Enter the details of a new film and click **Submit**. The new film's details should appear in the console.

The `CreateFilm` component uses a standard HTML form with a React `onSubmit` event handler. For more information, see the [React form documentation](https://react.dev/reference/react-dom/components/form).

When the form is submitted, the `handleSubmit` event handler is called. We read the data from the form and use it to create a new JavaScript object.

### Updating the `films` Array

This works, but the `CreateFilm` component needs to pass the new film object up to the `App` component, where it can be added to the array of films.

Modify the `<App>` component as follows:

```js
function App() {
  const [films, setFilms] = useState(initialFilms);
  const [chosenFilm, setChosenFilm] = useState(1);
  const selectFilm = (id) => {
    setChosenFilm(id);
  };
  const addFilm = (newFilm) => {
    // Map the films objects to ID numbers
    const filmIds = films.map((film) => film.id);
    // ... this turns the array into individual arguments e.g. [1,2,3,4,5]
    const highestID = Math.max(0, ...filmIds);
    const filmWithId = { ...newFilm, id: highestID + 1 };
    setFilms([...films, filmWithId]);
  };

  const film = films.find((film) => film.id === chosenFilm);

  return (
    <div>
      <FilmList films={films} onFilmSelect={selectFilm} />
      <FilmDetails film={film} />
      <CreateFilm onFilmAdd={addFilm} />
    </div>
  );
}
```

- We have made films a state variable `  const [films, setFilms] = useState(initialFilms);`
  - It is initialised with `initialFilms` when the page loads.
- When the user submits the form, the CreateFilm component will create the film object and passes it as an argument to the addFilm function in App .
- addFilm finds the highest id number of the existing films and uses this to assign an id value for the new film.
  ```js
  // Map the films objects to ID numbers
  const filmIds = films.map((film) => film.id);
  // ... this turns the array into individual arguments e.g. [1,2,3,4,5]
  const highestID = Math.max(0, ...filmIds);
  const filmWithId = { ...newFilm, id: highestID + 1 };
  ```
- We then update the films state variable with the new film.
- `setFilms([...films, filmWithId]);`

- Update `CreateFilm` so that it calls the `addFilm` function from `App`

```js
const CreateFilm = ({ onFilmAdd }) => {
  const handleSubmit = (event) => {
    event.preventDefault();
    const formData = new FormData(event.target);
    const newFilm = {
      title: formData.get("title"),
      year: Number(formData.get("year")),
      duration: Number(formData.get("duration")),
    };
    onFilmAdd(newFilm);
  };
  return (
    <div className="page">
      <h2>Add a new film</h2>
      <form onSubmit={handleSubmit}>
        <div>
          <label htmlFor="title">Title:</label>
          <input type="text" id="title" name="title" />
        </div>
        <div>
          <label htmlFor="year">Year:</label>
          <input type="number" id="year" name="year" />
        </div>
        <div>
          <label htmlFor="duration">Duration:</label>
          <input type="number" id="duration" name="duration" />
        </div>
        <div>
          <button type="submit">SAVE THE FILM</button>
        </div>
      </form>
    </div>
  );
};
```

Test that this works. The user should be able to add a new film. It should appear in the list, and the user should be able to click it to view its details.

## Deleting a fFlm

Finally, we'll allow the user to delete a film.

Modify `App` as follows:

```js
function App() {
  const [films, setFilms] = useState(initialFilms);
  const [chosenFilm, setChosenFilm] = useState(1);
  const selectFilm = (id) => {
    setChosenFilm(id);
  };
  const addFilm = (newFilm) => {
    const filmIds = films.map((film) => film.id);
    console.log(filmIds);
    // ... this turns the array into individual arguments: Math.max(1, 2, 3)
    const highestID = Math.max(0, ...filmIds);
    const filmWithId = { ...newFilm, id: highestID + 1 };
    setFilms([...films, filmWithId]);
  };
  const deleteFilm = (deleteId) => {
    const newFilmList = films.filter((film) => film.id !== deleteId);
    setFilms(newFilmList);
  };

  const film = films.find((film) => film.id === chosenFilm);

  return (
    <div>
      <FilmList films={films} onFilmSelect={selectFilm} />
      <FilmDetails film={film} onFilmDelete={deleteFilm} />
      <CreateFilm onFilmAdd={addFilm} />
    </div>
  );
}
```

We have:

- Added a new `deleteFilm` function that filters the array of films to remove the selected film.

```js
const deleteFilm = (deleteId) => {
  const newFilmList = films.filter((film) => film.id !== deleteId);
  setFilms(newFilmList);
};
```

- Passed this function as a prop to the `FilmDetails` component `<FilmDetails film={film} onFilmDelete={deleteFilm} />`

We also need to make some changes to `FilmDetails`:

```js
const FilmDetails = ({ film, onFilmDelete }) => {
  if (film === undefined) {
    return <p>No film selected.</p>;
  }
  return (
    <div className="page">
      <h2>{film.title}</h2>
      <div>
        {film.title} was released in {film.year}. It is {film.duration} minutes
        in length.
      </div>
      <button
        onClick={() => {
          onFilmDelete(film.id);
        }}
      >
        Delete
      </button>
    </div>
  );
};
```

We added an `onClick` handler to the button so that the `deleteFilm` function is called in the `App` component.

Because films can now be deleted, we also add an `if` statement to check whether the film exists. If it doesn't, we display a simple message.

## A Single-Page Application

The application works, but everything is displayed on a single page. We can use React Router (https://reactrouter.com/) to create separate screens or views. The application will still be a **single-page application (SPA)**: navigation happens within the existing application rather than loading a new HTML page each time.

Make sure you are in the example-react-app folder

First, install react router

```
npm install react-router-dom
```

Add the following import statement at the top of `App.jsx`:

```js
import {
  BrowserRouter as Router,
  Routes,
  Route,
  Link,
  useParams,
  useNavigate,
} from "react-router-dom";
```

Change the `return` statement in the `App` component to declare links and routes for the different components:

```js
return (
  <div>
    <div>
      <Link to="/">home</Link>
      <Link to="/create">create</Link>
    </div>
    <Routes>
      <Route
        path="/"
        element={<FilmList films={films} onFilmSelect={selectFilm} />}
      />
      <Route
        path="/films/:id"
        element={<FilmDetails films={films} onFilmDelete={deleteFilm} />}
      />
      <Route path="/create" element={<CreateFilm onFilmAdd={addFilm} />} />
    </Routes>
  </div>
);
```

- React Router provides the <Link/> component. This will create a hyperlink that will allow users to navigate between different components.
- We declare routes by specifying a path (URL) e.g. /create and the component we want to navigate to e.g. element={<CreateFilm onFilmAdd={addFilm} />}
- Note, we are now passing the films array to FilmDetails. It will become clear why we are doing this shortly.

To use React Router, we need to wrap our `<App />` component in a router component.

After the `App` component, create a new `AppWrapper` component.

Change the `export` statement so that it exports `<AppWrapper />` instead of `<App />`.

```js
function AppWrapper() {
  return (
    <Router>
      <App />
    </Router>
  );
}

export default AppWrapper;
```

Save your changes and test the application.

You should be able to navigate between the home and create views. The film links won't work yet.

### Getting the Film Links to work — [React Router: Navigating](https://reactrouter.com/start/declarative/navigating)

To make the `FilmLink` components work with React Router, we need to replace the HTML hyperlinks with React Router's `<Link>` components.

Change the `FilmLink` component as follows:

```js
const FilmLink = ({ film }) => {
  return <Link to={`/films/${film.id}`}>{film.title}</Link>;
};
```

Test the application.

The links should now work, and the user should be taken to the `FilmDetails` component. However, the film details don't yet update.

Look at the generated URL. The film ID is passed as a route parameter, for example, `/films/4`.

We can read this ID and use it to display the correct film.

Change `FilmDetails` as follows:

We can extract the route parameter using `useParams()`.

We then use `find()` to locate the matching film. The rest of `FilmDetails` is the same as before.

```js
const FilmDetails = ({ films, onFilmDelete }) => {
  const id = Number(useParams().id);
  const film = films.find((film) => film.id === id);

  if (film === undefined) {
    return <p>Film not found.</p>;
  }
  return (
    <div className="page">
      <h2>{film.title}</h2>
      <div>
        {film.title} was released in {film.year}. It is {film.duration} minutes
        in length.
      </div>
      <button
        onClick={() => {
          onFilmDelete(film.id);
        }}
      >
        Delete
      </button>
    </div>
  );
};
```

Test the application. Users should now be able to browse, create and delete films.

### Improving the Navigation

Although the app works, the app is confusing e.g. the user adds a new film and there isn't any feedback.
To improve the user experience we can use the `useNavigate` hook to 'redirect' the user.

Modify `App` to redirect the user following the create and delete actions:

```js
function App() {
  const navigate = useNavigate();
  const [films, setFilms] = useState(initialFilms);

  const addFilm = (newFilm) => {
    const filmIds = films.map((film) => film.id);
    console.log(filmIds);
    // ... this turns the array into individual arguments: Math.max(1, 2, 3)
    const highestID = Math.max(0, ...filmIds);
    const filmWithId = { ...newFilm, id: highestID + 1 };
    setFilms([...films, filmWithId]);
    navigate("/");
  };
  const deleteFilm = (deleteId) => {
    const newFilmList = films.filter((film) => film.id !== deleteId);
    setFilms(newFilmList);
    navigate("/");
  };

  return (
    <div>
      <div>
        <Link to="/">home</Link>
        <Link to="/create">create</Link>
      </div>
      <Routes>
        <Route path="/" element={<FilmList films={films} />} />
        <Route
          path="/films/:id"
          element={<FilmDetails films={films} onFilmDelete={deleteFilm} />}
        />
        <Route path="/create" element={<CreateFilm onFilmAdd={addFilm} />} />
      </Routes>
    </div>
  );
}
```

Because `FilmDetails` now selects the correct film to display, we have removed this functionality from `App` (including the `chosenFilm` state variable and the `selectFilm` function).

The `App` component is now much simpler:

Also, we no longer need to pass a selection function to `FilmList` using props. Update `FilmList` to remove `onFilmSelect`:

```js
const FilmList = ({ films }) => {
  return (
    <div className="page">
      <h2>Film List</h2>
      {films.map((film) => {
        return <FilmLink key={film.id} film={film} />;
      })}
    </div>
  );
};
```

Test the app again to make sure it still works.

## Limitations

This app shows many key features of React. However, it suffers from a major weaknesses. The data isn't persisted. As soon as the user closes the browser any new films we added are lost.

In the next session we will look at externalising the data in this app.
