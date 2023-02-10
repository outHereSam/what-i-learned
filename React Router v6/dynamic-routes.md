# Dynamic Routes

To implement dynamic routing with React Router we use **URL parameters**.
URL parameters are dynamic segments of a URL that act as placeholders for more specific resources. They appear with a colon `:` in a dynamic route like this:

```js
const route = createBrowserRouter(
  createRoutesFromElement(
    <Route path="/articles/:title" element={<Article />} />
  )
);
```

- The path prop `'/article/:title'` contains a URL parameter `:title`
- This suggests that when we navigate to pages such as `'/articles/learn-react'` or `'/articles/intro-to-nodejs'`, the `<Article />` component will render.
- When the `<Article />` component is rendered this way, it can access the value of the URL parameter `:title` and based on that determine which article to render
