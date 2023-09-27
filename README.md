# React + Vite

# React-Redux-Demo

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

# Redux

## Redux is a predictable state container for JavaScript apps.

```
It is for JavaScript apps
It is a state container
It is predictable
```

## Redux is for JavaScript applications

Redux is not tied to React.
Can be used with React, Angular, Vue or even vanilla JavaScript.
Redux is a library for JavaScript applications.

## Redux is a state container

Redux stores the state of your application.
Consider a React app - state of a component.
State of an app is the state represented by all the individual components of that app.
`Redux will store and manage the application state.`

For Example:
State can be -

> LoginFormComponent:

```
state = {
    username: '',
    password: '',
    submitting: false,
}
```

> UserListComponent:

```
state = {
    users: []
}
```

For medium to large scale applications, the state will be like

> Application:

```
state = {
    isUserLoggedIn: true,
    username: 'Asadulla',
    profileUrl: '',
    onlineUsers: [],
    isModalOpened: false,
}
```

## Redux is predictable

Predictable in what way? <br>
Redux is a state container<br>
The state of the application can change.

Ex: todo list app - item (pending) --> item (completed)

In redux, all state transitions are explicit and it is possible to keep track of them. The changes to your application's state become predictable.

            Parent Component
                    | name
                    V

    Component A     Component B     Component C
                        | name          | name
                        V               V
                    Component D     Component E
                                        | name
                                        V
                                    Component F

## React + Redux

In the medium to large scale applications, due to the props drilling, lifting state to the parent component or using the other ways like render props, etc, the state management could be a troublesome.

**So, Redux state management will help us out.**

With `Redux`, the state is contained outside the components.
If the last child component in the component tree wants to update the state, it will communicate with the `Redux state container`.

The state container updates the state in a predictable manner and then sends this value to only those components that are in need of that value.

**Redux 1.0 was released on August 2015.**

At that time there is no hooks and context API available. Hence, the props drilling issue was resolved with this Redux solution.

Later, when hooks were introduced, the `useContext + useReducer` and `context API` hooks solved this problem.

**React-Redux is the official Redux UI binding library for React.**

```
    React <---->  React-Redux <----> Redux
```

**_When should We use redux in our react application?_**

- It totally depends on the application requirements.
- The Redux store can be used in the applications which contains a couple of different routes and considerable number of components that need to share state.
- It is not mandatory to use Redux but several companies still using the Redux library to maintain the state management.
- May be it might not a relevant solution also. So, totally, it depends on the application requirements whether it requires the Redux store or any other methodology.
- In a particular period of time, if you struggle to maintain the applications state, then it's time to use the Redux state management.

## Installations:

`node used - 20.7.0`

```
npm install redux react-redux
```

```
npm install axios redux-thunk
```

## Three Core Concepts

A **store** that holds the state of your application.<br>
An **action** that describes the changes in the state of the application.<br>
A **reducer** which actually carries out the state transition depending on the action.

### Three principles

**_First Principle_**<br>
`The state of your whole application is stored in an object tree within a single store`<br>
Maintain our application state in a single object which would be managed by the Redux store.

State:

```
{
    numOfCakes: 10
}
```

**_Second Principle_**<br>
`The only way to change the state is to emit an action, an object describing what happened`<br>
To update the state of your app, you need to let Redux know about that with an action.<br>
Not allowed to directly update the state object.<br>
**Ex:** action - **BUY_CAKE**

```
{
    type: BUY_CAKE
}
```

**_Third Principle_**<br>
`To specify how the state tree is transformed by actions, you write pure reducers.`

Reducer - (previousState, action) => newState

```
const reducer = (state, action) => {
    switch (action.type) {
        case BUY_CAKE: return {
            numOfCakes: state.numOfCakes - 1
        }
    }
}
```

        subscribed
        |----------> JavaScript App ------------>|
        |                 |                      |
        ^                 V X                    V dispatch
        |                 |                      |
    ReduxStore  <---------|                     Action
        (state)                                  |
        ^                                        |
        |                                        V
        |                                        |
        |-----------> Reducer <------------------|

## Redux Store:

One store for the entire application.

Responsibilities:<br>

- Holds application state.
- Allows access to state via **_getState_**
- Allows state to be updated via **_dispatch(action)_**
- Registers listeners via **_subscribe(listener)_**
- Handles unregister of listeners via the function returned by **_subscribe(listener)_**
- At the end we can unsubscribe from the store by **_unsubscribe()_**

**Redux without react**

> **Example:**

```
import {createStore} from 'redux';

const store = createStore(reducer);
console.log('Initial state', store.getState());
const unsubscribe = store.subscribe(() => console.log('Updated state', store.getState()));
store.dispatch(buyCake());
store.dispatch(buyCake());
store.dispatch(buyCake());
```

> **Output:**

```
Initial state { numOfCakes: 10 }
Updated state { numOfCakes: 9 }
Updated state { numOfCakes: 8 }
Updated state { numOfCakes: 7 }
```

> **Example:**

```
----
import {combineReducers} from 'redux';
----

const rootReducer  = combineReducers({
    cake: cakeReducer,
    iceCream: iceCreamReducer
});
const store = createStore(rootReducer);
---
--- same as previous example
---
```

### MiddleWare:

It is the suggested way to extend Redux with custom functionality.

Provides a third-party extension point between dispatching an action, and the moment it reaches the reducer.

User middleware for logging, crash reporting, performing asynchronous tasks, etc.

> **Example:**
> Lets use redux-logger middleware to log the redux state on console.

```
----
import {applyMiddleware} from 'redux';
import * as reduxLogger from 'redux-logger';

const logger = reduxLogger.createLogger();
----

const rootReducer  = combineReducers({
    cake: cakeReducer,
    iceCream: iceCreamReducer
});
const store = createStore(rootReducer, applyMiddleware(logger));
---
--- same as previous example and no need to add console logs while subscribing.
---
```

### Redux-Thunk:

Its the standard way to define async action creators.

Its a **middleware** we need to apply for redux store.

**Ex:**

```
---
--- importing lib's
const applyMiddleware = redux.applyMiddleware;
const thunkMiddleware = require('redux-thunk').default;
const axios = require('axios');
---
---
---

const fetchUsers = () => {
    return function(dispatch) {
        dispatch(fetchUsersRequest());
        axios.get('<https://Users-URL>')
            .then(response => {
                const users = response.data.map(user => user.id);
                dispatch(fetchUsersSuccess(users));
            })
            .catch(error => {
                dispatch(fetchUsersFailure(error.message))
            })
    }
}

const store = createStore(reducer, applyMiddleware(thunkMiddleware));
--Subscribe---
store.dispatch(fetchUsers());
-----
```

## Thinks to Remember:

React is a library used to build user interfaces.<br>
Redux is a library for managing state in a predictable way in JavaScript applications.<br>
React-redux is a library that provides bindings to use React and Redux together in an application.
