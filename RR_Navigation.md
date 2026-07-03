# Navigation

- [Navigation](#navigation)
  - [Navigation with `Link`](#navigation-with-link)
  - [More about `<Link/>`](#more-about-link)
    - [`replace`](#replace)
    - [`reloadDocument`](#reloaddocument)
  - [Navigation with `<NavLink/>`](#navigation-with-navlink)
  - [`<Navigate/>`](#navigate)
  - [`useNavigate()`](#usenavigate)

---

---

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

---

---

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
            <Link to="/" replace>
              Home
            </Link>
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

---

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
            <Link to="/" reloadDocument>
              Home
            </Link>
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

---

---

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
            <NavLink to="/">Home</NavLink>
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

---

---

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

---

---

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

---

---
