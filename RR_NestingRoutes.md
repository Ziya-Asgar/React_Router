# Nesting Routes

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

---

---

## Rendering the same component for any nested route and the usage of the <Outlet /> component

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

---

---

## Rendering the same component for the routes that don't share the same URL

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

---

---

## `useOutletContext`

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

---

---
