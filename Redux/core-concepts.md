# Redux

## What is Redux?

Redux is a _predictable_ state container for JavaScript apps. What that basically means is that, it helps us create a global state container with data that can be accessed from any where in the application.

## Core Concepts

Let's say your app's state is described as a plain JavaScript object. An example of the state of a todo app might look something like this:

```js
{
  todos: [{
    text: 'Eat food',
    completed: true
  }, {
    text: 'Exercise',
    completed: false
  }],
  visibilityFilter: 'SHOW_COMPLETED'
}
```

This object is like a "model" of the state of the applications except there's no way to change it (in Redux terms, there are no setters)

## Actions

To change something in the state, you need to dispatch an action. An action is basically an object that describes what happened or the action you took to change the state data. Some examples are as follows:

```js
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }
```

To help us understand how and what caused our state to change is why we use actions. They give us a clear understanding of what's going on in our app.

# Reducer

To have our state communicate with our actions, we write a function called a reducer. A reducer is nothing crazy. It's just a function that takes the state of our application and the action as arguments and returns the next state of the app. It is key to note that it is wise to write reducer functions for different actions rather than putting them all in one _BIG_ function. Here's an example:

```js
function visibilityFilter(state = 'SHOW_ALL', action) {
    if (action.type === 'SET_VISIBILITY_FILTER') {
        return action.filter;
    } else {
        return state;
    }
}

function todos(state = [], action) {
    switch(action.type) {
        case 'ADD_TODO':
            return state.concat([{ text: action.text, completed: false }]);
        case 'TOGGLE_TODO':
            return state.map((todo, index) => action.index === index ? { text: todo:text, completed: !todo.completed } : todo);
        default:
            return state;
    }
}
```

We can have another reducer function the manages the complete state of our app by calling all the reducers for the corresponding states:

```js
function todoApp(state = {}, action) {
    return {
        todos: todos(state.todos, action),
        visibilityFilter: visibilityFilter(state.visibilityFilter, action);
    }
}
```

## Conclusion

The main idea of Redux is that you describe how your state is updated over time in response to action objects.
