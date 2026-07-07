# Defining Routes with `useRoutes`

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

---

---
