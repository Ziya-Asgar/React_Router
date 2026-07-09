# React Router

- [React Router](#react-router)
  - [Some useful links](#some-useful-links)
  - [Getting Started](#getting-started)
  - [Defining Routes](#defining-routes)
  - [Navigation](#navigation)
  - [Dynamic Routes](#dynamic-routes)
  - [General Route](#general-route)
  - [Nesting Routes](#nesting-routes)
  - [Defining the Same Route Multiple Times](#defining-the-same-route-multiple-times)
  - [Using `location`](#using-location)
  - [Defining Routes with `useRoutes`](#defining-routes-with-useroutes)
  - [`useSearchParams`](#usesearchparams)
  - [`useLocation()`](#uselocation)

<hr>

## Some useful links

- https://www.youtube.com/watch?v=Ul3y1LXxzdU
- https://blog.webdevsimplified.com/2022-07/react-router/

<hr>

## Getting Started

[Getting Started](./RR_GettingStarted.md)

<hr>

## Defining Routes

[Defining Routes](./RR_DefiningRoutes.md)

<hr>

## Navigation

[Navigation](./RR_Navigation.md)

<hr>

## Dynamic Routes

[Dynamic Routes](./RR_DynamicRoutes.md)

<hr>

## General Route

[General Route](./RR_GeneralRoute.md)

<hr>

## Nesting Routes

[Nesting Routes](./RR_NestingRoutes.md)

<hr>

## Defining the Same Route Multiple Times

[Defining the Same Route Multiple Times](./RR_DefiningSameRoute.md)

<hr>

## Using `location`

[Using `location`](./RR_UsingLocation.md)

<hr>

## Defining Routes with `useRoutes`

[Defining Routes with `useRoutes`](./RR_DefiningRoutesWith_useRoutes.md)

<hr>

## `useSearchParams`

[`useSearchParams`](./RR_useSearchParams.md)

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
