# `useLocation()`

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

---

---
