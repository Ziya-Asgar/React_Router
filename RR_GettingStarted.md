# Getting Started with `react-router-dom`

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
  </React.StrictMode>,
);
```

After we have our router setup in the _index.js_ file, we need to define the routes for our application.

---

---
