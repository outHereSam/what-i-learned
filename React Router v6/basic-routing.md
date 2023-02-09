# Basic Routing with `<Route>`

Now that we have our `router` in place, we can begin defining the different views or routes that our application will render for various URL paths. For example we might want to render an `About` component for the `/about` path and a `SignIn` component for the `/sign-in` path. React Router gives us two options to define routes: JSX or objects.

The `.createBrowserRouter` method accepts an array of `<Route>` objects so we'll need to use React Router's `.createRoutesFromElement` to configure our routes with JSX. We'll also need to use React Router's `<Route>` component to define our routes. These components can be imported from the `react-router-dom` package like so:

```js
import {
  RouterProvider,
  createBrowserRouter,
  createRoutesFromElement,
  Route,
} from "react-router-dom";
```

The `<Route>` component is designed to render (or not render) a component based on the current URL path. Each `<Route>` component should include:

1. A `path` prop indicating the _exact_ URL path that will cause the route to render.

2. An `element` prop specifying which component to render for the given URL path.

We can use the `.createRoutesFromElement` to configure a `<Route>` that renders the `<About>` component when the URL path matches `/about`:

```js
import {
  RouterProvider,
  createBrowserRouter,
  createRoutesFromElement,
  Route,
} from "react-router-dom";

const router = createBrowserRouter(
  createRoutesFromElement(<Route path="/about" element={<About />} />)
);
```
