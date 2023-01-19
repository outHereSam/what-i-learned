# Three Principles

Redux can be best described by three fundamental principles

## Single Source of truth

The global state or the _single source of truth_ of your app is stored in an object within a single store. This makes it easier to manage the state of your application since there is only one store.

```js
console.log(store.getState());

/* Prints
{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
*/
```

## State is read-only

The only way to change the state is to _dispatch_ an action, a JavaScript object describing what happened. This prevents other sources from writing directly to the state. It also to track which actions did what because they follow a strict order.

```js
store.dispatch({
  type: "COMPLETE_TODO",
  index: 1,
});

store.dispatch({
  type: "SET_VISIBILITY_FILTER",
  filter: "SHOW_COMPLETED",
});
```

## Changes are made with _pure functions_

Reducers, as we learnt earlier are pure functions that specify how the state tree is transformed by actions. The reducer takes in the previous state and an action and returns a new state.  
**NB**: Make sure to return a new state object instead of mutating/changing the previous state.

```js
// Visibility Reducer
function visibilityFilter(state = "SHOW_ALL", action) {
  switch (action.type) {
    case "SET_VISIBILITY_FILTER":
      return action.filter;
    default:
      return state;
  }
}

// Todos Reducer
function todos(state = [], action) {
  switch (action.type) {
    case "ADD_TODO":
      return [
        ...state,
        {
          text: action.text,
          completed: false,
        },
      ];
    case "COMPLETE_TODO":
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true,
          });
        }
        return todo;
      });
    default:
      return state;
  }
}

// In another file, usually the index.js or app.js
import { combineReducers, createStore } from "redux";
const reducer = combineReducers({ visibilityFilter, todos });
const store = createStore(reducer);
```
