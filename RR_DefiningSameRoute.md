# Defining the Same Route Multiple Times

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

---

---
