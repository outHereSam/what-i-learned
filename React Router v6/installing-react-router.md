# Installing React Router

To use React Router, you will need to include the `react-router-dom` package (the version of React Router built specifically for web browsers as there is another for React Native) in your project by using `npm` like so:

```console
npm install --save react-router-dom@6
```

Once you add `react-router-dom` to your project, you now have access to all the routers that come with the package. The most common one, however is `createBrowserRouter`. To add that to your project, we can import it at the top of our file just like every other package, like so:

```js
import { createBrowserRouter } from "react-router-dom";
```

Next, we'll initialize our router by calling `createBrowserRouter`. This method accepts a list of JSX components.

```js
import { createBrowserRouter } from "react-router-dom";

const router = createBrowserRouter(/* routes are defined in here */);
```

The `router` serves as the basis for all the React Router logic. Without it, we'd get errors.
