# `useSearchParams()`

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

---

---
