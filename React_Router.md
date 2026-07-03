# React Router

- [React Router](#react-router)
  - [Some useful links](#some-useful-links)
  - [Getting Started](#getting-started)
  - [Defining Routes](#defining-routes)
  - [Navigation](#navigation)
  - [Dynamic Routes](#dynamic-routes)
  - [General Route](#general-route)
  - [Nesting Routes](#nesting-routes)
    - [Rendering the same component for any nested route and the usage of the component](#rendering-the-same-component-for-any-nested-route-and-the-usage-of-the--component)
    - [Rendering the same component for the routes that don't share the same URL](#rendering-the-same-component-for-the-routes-that-dont-share-the-same-url)
    - [`useOutletContext`](#useoutletcontext)
  - [Defining the same route more than once](#defining-the-same-route-more-than-once)
  - [Using `location` - this one needs to be looked at](#using-location---this-one-needs-to-be-looked-at)
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

If we want a certain component to be rendered, no matter which URL a user goes to (except the ones that we have set), then we can use `path="*"` in the `<Route/>`.

In the below example, we are rendering the `<NotFound/>` component, whenever a user goes to a URL that we have not specified.

```js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";
import Book from "./pages/Book";
import NewBook from "./pages/NewBook";
import NotFound from "./pages/NotFound";

function App() {
  return (
    <>
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
        <Route path="/books" element={<BookList />} />
        <Route path="/books/:id" element={<Book />} />
        <Route path="/books/new" element={<NewBook />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </>
  );
}

export default App;
```

<hr>

## Nesting Routes

Instead of defining routes one after another and specifically indicating the whole path, we can nest routes.

In the below example, we define a route with the `path="/books"`. If we define routes nested inside that route, we won't need to write `/books` again. For example in the below code as `<Route path=":id"...` is nested inside a route with the path `/books`, it is the same as `<Route path="/books/:id"...`.

If we nested a route inside a parent route and want to indicate which component to render when a user goes to the parent route, we can use `index` to refer to the parent route.

```js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";
import Book from "./pages/Book";
import NewBook from "./pages/NewBook";
import NotFound from "./pages/NotFound";

function App() {
  return (
    <>
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

        <Route path="/books">
          <Route index element={<BookList />} />
          <Route path=":id" element={<Book />} />
          <Route path="new" element={<NewBook />} />
        </Route>

        <Route path="*" element={<NotFound />} />
      </Routes>
    </>
  );
}

export default App;
```

<hr>

### Rendering the same component for any nested route and the usage of the <Outlet /> component

We can render a component on a parent route, and that component will be rendered even if a user goes to a nested route. This is not as useful at this point. However, when we add `<Outlet/>` into the component of the parent rout, then the component on the parent route as well as components on the nested routes will be rendered. `<Outlet/>` is a component from React Router and it renders a route's child component.

In this example, we render the `<BookLayout/>` component, when a user goes to the `/books` route or any nested route. Because we have `<Outlet/>` used in the `<BookLayout/>` component, `<BookList />`, `<Book />}`, and `<NewBook />` uses that component.

```js
// App.js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";
import Book from "./pages/Book";
import NewBook from "./pages/NewBook";
import NotFound from "./pages/NotFound";
import BookLayout from "./BookLayout";

function App() {
  return (
    <>
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

        <Route path="/books" element={<BookLayout />}>
          <Route index element={<BookList />} />
          <Route path=":id" element={<Book />} />
          <Route path="new" element={<NewBook />} />
        </Route>

        <Route path="*" element={<NotFound />} />
      </Routes>
    </>
  );
}

export default App;
```

```js
// BookLayout.js
import { Link, Outlet } from "react-router-dom";

export default function BookLayout() {
  return (
    <>
      <Link to="/books/1">Book 1</Link>
      <br />
      <Link to="/books/2">Book 2</Link>
      <br />
      <Link to="/books/new">New Book</Link>

      <Outlet />
    </>
  );
}
```

<hr>

### Rendering the same component for the routes that don't share the same URL

We might want to render the same component even for the routes that don't share the same URL. In this case, we would nest routes inside the parent route, but not specify any path for the parent route.

```js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";
import Book from "./pages/Book";
import NewBook from "./pages/NewBook";
import NotFound from "./pages/NotFound";
import BookLayout from "./BookLayout";

function App() {
  return (
    <>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/unrelated">unrelated</Link>
          </li>
          <li>
            <Link to="/not-related">not-related</Link>
          </li>
        </ul>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />

        <Route element={<BookLayout />}>
          <Route path="/unrelated" element={<Book />} />
          <Route path="/not-related" element={<NewBook />} />
        </Route>

        <Route path="*" element={<NotFound />} />
      </Routes>
    </>
  );
}

export default App;
```

<hr>

### `useOutletContext`

We can also provide a context value to the `<Outlet/>` component and with `useOutletContext()`, we can use that value in any component rendered for nested routes.

```js
// BookLayout
import { Link, Outlet } from "react-router-dom";

export default function BookLayout() {
  return (
    <>
      <Link to="/books/1">Book 1</Link>
      <br />
      <Link to="/books/2">Book 2</Link>
      <br />
      <Link to="/books/new">New Book</Link>
      <Outlet context={{ hello: "World" }} />
    </>
  );
}
```

```js
// Book.js
import { useOutletContext, useParams } from "react-router-dom";

const Book = () => {
  const { id } = useParams();

  const obj = useOutletContext();
  return (
    <h1>
      Book {id} {obj.hello}
    </h1>
  );
};

export default Book;
```

```js
// App.js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";
import Book from "./pages/Book";
import NewBook from "./pages/NewBook";
import NotFound from "./pages/NotFound";
import BookLayout from "./BookLayout";

function App() {
  return (
    <>
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

        <Route path="/books" element={<BookLayout />}>
          <Route index element={<BookList />} />
          <Route path=":id" element={<Book />} />
          <Route path="new" element={<NewBook />} />
        </Route>

        <Route path="*" element={<NotFound />} />
      </Routes>
    </>
  );
}

export default App;
```

<hr>

## Defining the same route more than once

We can define the same route more than once and render different components. All the different components will be rendered one after another.

In this example, we have defined `<Routes>...</Routes>` twice, and we specified `/books` route in both of them. Both of them will be rendered.

```js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";
import Book from "./pages/Book";
import NewBook from "./pages/NewBook";
import NotFound from "./pages/NotFound";
import BookLayout from "./BookLayout";

function App() {
  return (
    <>
      <Routes>
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
        <Route path="/books" element={<BookLayout />}>
          <Route index element={<BookList />} />
          <Route path=":id" element={<Book />} />
          <Route path="new" element={<NewBook />} />
        </Route>
        <Route path="*" element={<NotFound />} />
      </Routes>
    </>
  );
}

export default App;
```

<hr>

## Using `location` - this one needs to be looked at

We can also use the `location` prop with the `Routes` component. When we provide a path to the `location` prop, no matter which URL a user tries to go, browser navigates to the path that we provided to the `location` prop.

In the below example, we set the `location` to `/books`. No matter which URL a user tries to go, browser will render the same components that we specified for the path `/books`.

```js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";
import Book from "./pages/Book";
import NewBook from "./pages/NewBook";
import NotFound from "./pages/NotFound";
import BookLayout from "./BookLayout";

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
        <Route path="/books" element={<BookLayout />}>
          <Route index element={<BookList />} />
          <Route path=":id" element={<Book />} />
          <Route path="new" element={<NewBook />} />
        </Route>
        <Route path="*" element={<NotFound />} />
      </Routes>
    </>
  );
}

export default App;
```

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
