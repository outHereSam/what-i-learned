# Providing A Router

In React Router paradigm, the different views of your application, also called _routes_ are just React Components which need to be rendered.

The first step is to make the router available to our entire application by using React Router's `RouterProvider`. So inside our `App.js`:

```js
import { RouterProvider, createBrowserRouter } from "react-router-dom";

const router = createBrowserRouter(/* routes are defined here */);

export default function App() {
  return <RouterProvider router={router} />;
}
```

`createBrowserRouter` will define a router that will prevent the page from reloading when the URL changes. Instead, URL changes will allow `router` to determine which of its defined routes to render while passing along information about the current URL's path as props. We then make our router available everywhere in our application by providing it using `RouterProvider` at the root of our application.
