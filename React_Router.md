# React Router

- [React Router](#react-router)
  - [Some useful links](#some-useful-links)
  - [Getting Started](#getting-started)
  - [Defining Routes](#defining-routes)
  - [Navigation](#navigation)
  - [Dynamic Routes](#dynamic-routes)
  - [General Route](#general-route)
  - [Nesting Routes](#nesting-routes)
  - [Defining the Same Route Multiple Times](#defining-the-same-route-multiple-times)
  - [Using `location`](#using-location)
  - [Nesting Routes using different files - this one needs to be looked at](#nesting-routes-using-different-files---this-one-needs-to-be-looked-at)
  - [Defining Routes with `useRoutes`](#defining-routes-with-useroutes)
  - [`useSearchParams()`](#usesearchparams)
  - [`useLocation()`](#uselocation)

<hr>

## Some useful links

- https://www.youtube.com/watch?v=Ul3y1LXxzdU
- https://blog.webdevsimplified.com/2022-07/react-router/

<hr>

## Getting Started

[Getting Started](./RR_GettingStarted.md)

<hr>

## Defining Routes

[Defining Routes](./RR_DefiningRoutes.md)

<hr>

## Navigation

[Navigation](./RR_Navigation.md)

<hr>

## Dynamic Routes

[Dynamic Routes](./RR_DynamicRoutes.md)

<hr>

## General Route

[General Route](./RR_GeneralRoute.md)

<hr>

## Nesting Routes

[Nesting Routes](./RR_NestingRoutes.md)

<hr>

## Defining the Same Route Multiple Times

[Defining the Same Route Multiple Times](./RR_DefiningSameRoute.md)

<hr>

## Using `location`

[Using `location`](./RR_UsingLocation.md)

<hr>

## Nesting Routes using different files - this one needs to be looked at

We can also nest routes in a different way. Instead of nesting everything in one file, we can split our code. We can create a different file, and part of our `<Routes>...</Routes>` could be there. Then, we can import that file, use that file as a component.

First, we create `<BookRoutes/>` component and move our nested routes there.

```js
// BookRoutes.js
import { Route, Routes } from "react-router-dom";
import BookList from "./pages/BookList";
import Book from "./pages/Book";
import NewBook from "./pages/NewBook";
import BookLayout from "./BookLayout";

export function BookRoutes() {
  return (
    <>
      <BookLayout />
      <Routes>
        <Route index element={<BookList />} />
        <Route path=":id" element={<Book />} />
        <Route path="new" element={<NewBook />} />
      </Routes>
    </>
  );
}
```

Then, we import `<BookRoutes/>` and when we use it, we also have to provide star sign - `/*` to the `path` prop. When one route renders other routes, we have to specify `/*`. Adding this sign means, render this component for a route that matches `/books/` and any character after that.

```js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import NotFound from "./pages/NotFound";
import { BookRoutes } from "./BookRoutes";

function App() {
  return (
    <>
      <Routes location="/books">
        <Route path="/books" element={<h1>Extra Content</h1>} />
      </Routes>

      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/books">Books</Link>
          </li>
        </ul>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/books/*" element={<BookRoutes />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </>
  );
}

export default App;
```

<hr>

## Defining Routes with `useRoutes`

Another way to define a React Route is to use a hook called `useRoutes`. We will provide an array of objects to it, and inside each object, we can specify our routes.

```js
// App.js
import { Route, Routes } from "react-router-dom";
import { Home } from "./Home";
import { NotFound } from "./NotFound";

export function App() {
  const myRoutes = useRoutes([
    {
      path: "/",
      element: <Home />,
    },
    {
      path: "*",
      element: <NotFound />,
    },
  ]);

  return myRoutes;
}
```

If we want to indicate nested routes with `useRoutes`, we can use the `children` property.

```js
// App.js
import { Route, Routes } from "react-router-dom";
import { Home } from "./Home";
import { BookList } from "./BookList";
import { Book } from "./Book";

export function App() {
  const myRoutes = useRoutes([
    {
      path: "/",
      element: <Home />,
    },
    {
      path: "/books",
      children: [
        { index: true, element: <BookList /> },
        { path: ":id", element: <Book /> },
      ],
    },
  ]);

  return myRoutes;
}
```

<hr>

## `useSearchParams()`

To be able to access the search parameters in a URL, we can use the `useSearchParams` hook. When we initialize it, we can use 2 variables in an array, just like we are doing when we initialize `useState`. During the initialization, we can either provide `useSearchParams` with an empty object or a key-value pair in an object. Then, we can use `get` method to access a specific key.

Here we set the key `n` and the value `3` for that key during initialization. Then, we `get` the value for the key `n` and use it in the `Link` component.

```js
import { Link, useSearchParams } from "react-router-dom";

export function BookLayout() {
  const [searchParams, setSearchParams] = useSearchParams({ n: 3 });
  const number = searchParams.get("n");

  return (
    <>
      <Link to="/books/1">Book 1</Link>
      <br />
      <Link to="/books/2">Book 2</Link>
      <br />

      <Link to={`/books/${number}`}>Book {number}</Link>
      <br />

      <Link to="/books/new">New Book</Link>

      <input
        type="number"
        value={number}
        onChange={(e) => setSearchParams({ n: e.target.value })}
      />
    </>
  );
}
```

Here, we initialize the `useSearchParams` without any initial key-value pair. But we `get` the values for the keys `dog` and `food`. So, when a user goes to a url that looks like this `./books?food=someFood&dog=dogName` our conditional components relying on these keys will render.

```js
import { Link, useSearchParams } from "react-router-dom";

export function BookLayout() {
  const [searchParams, setSearchParams] = useSearchParams();
  const food = searchParams.get("food");
  const dog = searchParams.get("dog");

  return (
    <>
      <Link to="/books/1">Book 1</Link>
      <br />
      <Link to="/books/2">Book 2</Link>
      <br />

      <Link to="/books/new">New Book</Link>

      <div>
        {food && <p>`My favorite food is ${food}`</p>}
        {dog && <p>`My dog's name is ${dog}`</p>}
      </div>
    </>
  );
}
```

<hr>

## `useLocation()`

We can get an access to a state and location data using the `useLocation` hook. Using this hook is very simple as it returns one value and takes no parameters.

```js
const location = useLocation();
```

If we have the following URL `http://localhost/books?n=32#id` then the return value of `useLocation` would look like this.

```js
{
  pathname: "/books",
  search: "?n=32",
  hash: "#id",
  key: "2JH3G3S",
  state: null
}
```

You can notice that we have a `state` property being returned from `useLocation` as well. This `state` data can be anything and is passed between pages without being stored in the URL. For example if you click on a `Link` that looks like this:

```js
<Link to="/books" state={{ name: "Kyle" }}>
```

then the `state` value in the `location` object will be set to `{ name: "Kyle" }`. This can be really useful, if for example you want to send across simple messages between pages that shouldn’t be stored in the URL.
