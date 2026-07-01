# React Router

- [React Router](#react-router)
  - [Some useful links](#some-useful-links)
  - [Install and use `react-router-dom`](#install-and-use-react-router-dom)
  - [Define routes](#define-routes)
  - [Navigation with `Link`](#navigation-with-link)
  - [Dynamic Routes with `useParams`](#dynamic-routes-with-useparams)
  - [General Route](#general-route)
  - [Nesting Routes](#nesting-routes)
    - [Rendering the same component for any nested route and the usage of the  component](#rendering-the-same-component-for-any-nested-route-and-the-usage-of-the--component)
    - [Rendering the same component for the routes that don't share the same URL](#rendering-the-same-component-for-the-routes-that-dont-share-the-same-url)
    - [`useOutletContext`](#useoutletcontext)
  - [Defining the same route more than once](#defining-the-same-route-more-than-once)
  - [Using `location` - this one needs to be looked at](#using-location---this-one-needs-to-be-looked-at)
  - [Nesting Routes using different files - this one needs to be looked at](#nesting-routes-using-different-files---this-one-needs-to-be-looked-at)
  - [Defining Routes with `useRoutes`](#defining-routes-with-useroutes)
  - [More about `<Link/>`](#more-about-link)
    - [`replace`](#replace)
    - [`reloadDocument`](#reloaddocument)
  - [Navigation with `<NavLink/>`](#navigation-with-navlink)
  - [`<Navigate/>`](#navigate)
  - [`useNavigate()`](#usenavigate)
  - [`useSearchParams()`](#usesearchparams)
  - [`useLocation()`](#uselocation)

<hr>

## Some useful links

- https://www.youtube.com/watch?v=Ul3y1LXxzdU
- https://blog.webdevsimplified.com/2022-07/react-router/

<hr>

## Install and use `react-router-dom`

To use the **React Router**, you need to install it:

```
npm i react-router-dom
```

After installing the `react-router-dom`, we can import `BrowserRouter`. After importing it, we will need to wrap our `<App/>` inside `<BrowserRouter>...</BrowserRouter` component.

```js
// index.js
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { BrowserRouter } from "react-router-dom";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

After we have our router setup in the _index.js_ file, we need to define the routes for our application.

## Define routes

To define the routes, in our `<App/>` component, we need to use `Route` component inside of `<Routes>...</Routes>`. Here is an example:

```js
// App.js
import { Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/books" element={<BookList />} />
    </Routes>
  );
}

export default App;
```

In the above example, when a user goes to the root URL, the `<Home/>` component is rendered. When a user goes to the `/books` URL, the `<BookList/>` component is rendered.

## Navigation with `Link`

To add navigation to our App, we can also use the `Link` component from the `react-router-dom`.

```js
// App.js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";

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
      </Routes>
    </>
  );
}

export default App;
```

<hr>

## Dynamic Routes with `useParams`

We can define dynamic routes using React Router. To do this, we will use `:` and `useParams` hook.

First, we define the URl with `:`:

```js
// App.js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";
import Book from "./pages/Book";

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
      </Routes>
    </>
  );
}

export default App;
```

The above example says that if there is anything after `./books/` URL, store it as `id` (which is now a URL parameter) and render the `<Book/>` component. Inside the `<Book/>` component, we have below code:

```js
// Book.js
import { useParams } from "react-router-dom";

const Book = () => {
  const { id } = useParams();
  return <h1>Book {id}</h1>;
};

export default Book;
```

The above code imports `useParams`, because we need it to access the URL parameters. Then, we destructure it and store it with the `id` variable. After that, we have a simple code to render a heading with the word "Book" and the URL parameter that we received. If a user goes to `./books/1`, the `<Book/>` component will render `<h1>Book 1</h1>`. If a user goes to `./books/abc`, the `<Book/>` component will render `<h1>Book abc</h1>`.

<hr>

Let's say we set another route, such as `/books/new` and render a different component for this route. Because we have `/books/:id` and `/books/new` routes, would React not confuse which component to render?! The answer is **no**. if we set a specific route, even if there is a match with another route that has a URL parameter, the specific route would be used.

For example, below we have `/books/:id` and `/books/new` routes and we are rendering different components for each route. If a user goes to `/books/new` route, we will render `<NewBook />` component, because the route is more specific.

```js
// App.js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import BookList from "./pages/BookList";
import Book from "./pages/Book";
import NewBook from "./pages/NewBook";

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
      </Routes>
    </>
  );
}

export default App;
```

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

## More about `<Link/>`

### `replace`

Let's have more info about the `<Link/>`. In addition to having the `to` prop to set a path, this component also has a `replace` prop. If we have `replace` in a `<Link/>`, then the new page will replace the old page in the history of the browser. Thus, if we click _back_ on a browser, we won't go to that page that we moved from. This might be useful for a login page.

In this example, we have `replace` in the `<Link/>` that goes to the _Home_ page. So, if we click that link on any page, the _Home_ page will replace the previous page in the history of the browser.

```js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import NotFound from "./pages/NotFound";
import { BookRoutes } from "./BookRoutes";

function App() {
  return (
    <>
      <nav>
        <ul>
          <li>
            <Link to="/" replace>Home</Link>
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

### `reloadDocument`

Another prop that can be used with the `<Link/>` is the `reloadDocument`. Usually, React just changes the content inside a route, when we go to that route. However, `reloadDocument` reloads the page entirely, when we click on `<Link/>`.

```js
import { Link, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import NotFound from "./pages/NotFound";
import { BookRoutes } from "./BookRoutes";

function App() {
  return (
    <>
      <nav>
        <ul>
          <li>
            <Link to="/" reloadDocument>Home</Link>
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

## Navigation with `<NavLink/>`

Another useful component related to React Router is `<NavLink/>`. It provides the same functionality as `<Link/>` but has some additional benefits. It mainly helps to do something with an active link.

- When a `<NavLink/>` is set to a certain URL and a user goes to that URL, that `<NavLink/>` receives the `active` CSS class. So, we can style this class in CSS, and when a user navigates to a URL using `<NavLink/>`, the style that we prepared for the `active` class will be aplied to that `<NavLink/>`.

- `<NavLink/>` can be used with `style` and `className` props. These props, while using with `<NavLink/>`, accept a function and using that function we can access the `isActive` property. This could help to either set an inline style or use a CSS class.

- We can also use `isActive` to have the `<NavLink/>` conditionally return something.

```js
import { Link, NavLink, Route, Routes } from "react-router-dom";
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
            <NavLink
              style={({ isActive }) => {
                return isActive ? { color: "red" } : {};
              }}
              to="/"
            >
              {({ isActive }) => {
                return isActive ? "Active Home" : "Home";
              }}
            </NavLink>
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

- If we have nested routes specified with the `<NavLink/>`, then when a user goes to a child route, the parent route is considered as activated and also receives the `active` CSS class. So, the style prepared for that class is applied to the parent route. To stop this, we just need to use the `end` prop.

```js
import { Link, NavLink, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import NotFound from "./pages/NotFound";
import { BookRoutes } from "./BookRoutes";

function App() {
  return (
    <>
      <nav>
        <ul>
          <li>
            <NavLink to="/">
              Home
            </NavLink>
          </li>
          <li>
            <NavLink
              to="/books"
              end
              style={({ isActive }) => {
                return isActive ? { color: "red" } : {};
              }}
            >
              Books
            </NavLink>
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

## `<Navigate/>`

Another navigation-related component is `<Navigate/>`. It automatically redirects a user to a certain page. It has the same props as `<Link/>`.

For example, if we use `<Navigate to="/" />` in the _NotFound_ component, then whenever a user is directed to this component it will be redirected to the _Home_ page.

```js
// NotFound.js
import { Navigate } from "react-router-dom";

const NotFound = () => {
  return <Navigate to="/" />;
};

export default NotFound;
```

<hr>

## `useNavigate()`

Another useful navigation tool is `useNavigate()` hook. It takes no parameters and returns a single function which you can use to redirect a user to specific pages. This navigate function takes two parameters. The first parameter is the location you want to redirect the user to and the second parameter is an object where you can have keys for `replace`, and `state`.

The below example uses `setTimeout` to wait for a second and then redirect the user to the _Home_ page.

```js
// NotFound.js
import { useEffect } from "react";
import { useNavigate } from "react-router-dom";

const NotFound = () => {
  const navigate = useNavigate();

  useEffect(() => {
    setTimeout(() => {
      navigate("/");
    }, 1000);
  }, []);

  return <h1>NotFound</h1>;
};

export default NotFound;
```

The below example will redirect the user to the `/books` route. It will also replace the current route in history and pass along some state information as well.

```js
// NotFound.js
import { useEffect } from "react";
import { useNavigate } from "react-router-dom";

const NotFound = () => {
  const navigate = useNavigate();

  useEffect(() => {
    setTimeout(() => {
      navigate("/books", { replace: true, state: { bookName: "Some Title" } });
    }, 1000);
  }, []);

  return <h1>NotFound</h1>;
};

export default NotFound;
```

Another way you can use the navigate function (the function returned from `useNavigate`) is to pass to it a number. This will allow you to simulate hitting the forward/back button.

```js
navigate(-1); // Go back one page in history
navigate(-3); // Go back three pages in history
navigate(1); // Go forward one page in history
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
