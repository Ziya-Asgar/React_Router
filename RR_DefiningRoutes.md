# Defining Routes

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

---

---
