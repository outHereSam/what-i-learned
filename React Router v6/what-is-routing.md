# What Is Routing?

_Routing_ is the process by which a web application uses the current browser URL to determine what content to display to a user.
So for a user visitin `/wiki/Node.js` page would expect to see something different from `/wiki/React/learn-react` page.

Taking a look at an example URL:

![An example URL](https://static-assets.codecademy.com/Courses/Learn-Node/http/url-dark.png)

URLs consist of different components, some of which are mandatory and others optional:

1. The scheme (eg. `HTTP`, `HTTPS`, `mailto`, etc) specifies which protocol to be used to access the resource.
2. The domain (eg. `codecademy.com`), specifies the website that hosts the resource. It mainly serves as the entry point of your application.
3. The path (eg. `/articles`), identifies the specific resource or page to be loaded and displayed to the user. This is essentially where the routing begins.
4. The optional query string (eg. `?search=node`), which appears after a `?` and assigns values to parameters.
