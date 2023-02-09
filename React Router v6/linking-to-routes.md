# Linking To Routes

In HTML websites, we use the anchor (`<a>`) tag to create links to other pages. The downside to this, however, is that the anchor tag reloads the page. This can be distracting in React applications as the main goal of React is to create fast and immersive application experiences.
React Router offers two solutions to this issue: the `Link` and `NavLink` components, both of which can be imported from the `reac-router-dom` package:

```js
import {
  createBrowserRouter,
  createRoutesFromElement,
  Route,
  Link,
  NavLink,
} from "react-router-dom";
```

Both the `Link` and `NavLink` components work like the anchor tags:

- They have a `to` prop that indicates the location to redirect the user to, similar to the anchor tag's `href` attribute.
- They wrap some HTML to use as the display for the link.

```js
<Link to="/about">About</Link>
<NavLink to="/about">About</NavLink>
```

Both of these links will generate anchor (`<a>`) tags with the text "About", which will take the user to the `/about` view when clicked. However, the default behaviour of the anchor tag, which is refreshing the page, will be disabled.
By prefacing our the value provided to our `to` prop with a forward slash, the path will be treated as an **absolute** path. Meaning, React Router will assume this path is navigating from the root directory.

### What's the difference between `Link` and `NavLink` then?

The difference boils down to basically presentation. When we use `NavLink`, the link will automatically have an `active` class applied to it. This can be useful as we can define custom CSS styles for the `.active` class.
