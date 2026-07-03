# Dynamic Routes

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

---

---
