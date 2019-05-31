# Redux

#### Table of Contents

- [Redux](#redux)
      - [Table of Contents](#table-of-contents)
  - [What Is Redux](#what-is-redux)
    - [State](#state)
    - [Store](#store)
    - [Installation](#installation)
  - [Data FLow in Redux](#data-flow-in-redux)
    - [Action](#action)
    - [Reducer](#reducer)
    - [Creating a Store](#creating-a-store)
    - [Getting the State](#getting-the-state)
    - [Changing the State](#changing-the-state)
      - [Action Creators](#action-creators)
    - [Listening for Changes](#listening-for-changes)
    - [Data Flow Review](#data-flow-review)
  - [React with Redux](#react-with-redux)
    - [Installation](#installation-1)
    - [Provider](#provider)
    - [Connect: Wrapping a Component and mapStateToProps](#connect-wrapping-a-component-and-mapstatetoprops)
      - [Example with a Stateful Component](#example-with-a-stateful-component)
    - [Connect: Dispatching an Action (mapDispatchToProps)](#connect-dispatching-an-action-mapdispatchtoprops)
      - [Action Creators in React Redux](#action-creators-in-react-redux)
      - [Example with a Stateful Component](#example-with-a-stateful-component-1)
    - [Using Wrapped Components](#using-wrapped-components)
  - [Organizing Redux](#organizing-redux)
    - [Presentational Component](#presentational-component)
    - [Container Component](#container-component)
      - [Higher Order Component](#higher-order-component)
    - [combineReducers](#combinereducers)
      - [Messages Reducer](#messages-reducer)
    - [Redux Directory Structure](#redux-directory-structure)
  - [React Router Redux](#react-router-redux)
    - [App.js](#appjs)
    - [TodoList.js](#todolistjs)
    - [NewTodoForm.js](#newtodoformjs)
  - [React + Backend](#react--backend)
    - [Redux Thunk](#redux-thunk)
      - [Thunk](#thunk)
      - [Usage of Redux Thunk](#usage-of-redux-thunk)
      - [actionCreators.js](#actioncreatorsjs)
      - [rootReducer.js](#rootreducerjs)
      - [TodoList.js](#todolistjs-1)

## What Is Redux

Redux is a popular state management library (inspired by Flux and Elm)

It models the application state as a single JS object which means there's only one place where all our data in the application is stored

Redux isn't necessary for apps and it is independent of React, but it can be a useful tool because it allows us to keep all of our state in a central place called the store

### State

State is any data in the app that's going to be changing; it could refer to a list of todos, some info about users, etc.

Redux state is similar to React state, but it is a bit less for our UI, and more for just the data that's going to be shared in different parts of our application

Whenever we think about apps that we got on the front-end, there's going to be information that changes pretty constantly, and having one centralized place to manage that is really useful (especially when we have a lot of different files and so on)

### Store

A store is one big JS object that represents our state in the entire application

A store is a centralized place where our state lives; once we make the store, we'll be able to actually see data flow throughout the application and make changes to that state


### Installation

```bash
$npm install --save redux
```

## Data FLow in Redux

- Action which is dispatched to cause a change in the state
- Reducer which decides what those state changes should be when an action gets dispatched
- Store which contains our actual JS object of keys and values for the state of our application

### Action

A plain JS object that must have a key called type and a string value (represents some action that we want to take in the application)

The action can have any number of additional keys (id, etc.)

```javascript
{
  type: 'LOGOUT_USER'
}
```

### Reducer

Once we've created actions, the next step is to create a reducer

It is a function that is passed to createStore, it accepts the state and an action, and it returns a new state (entire state object)

It is a specific function that tells our centralized store (which is our state container) what the state looks like and how to make changes to the state
- It decides what state to return given the old state and the current action
- If we make a store without a reducer, we're going to get an error
- It is always a function
- It should be a pure function
- We can have multiple reducers

```javascript
// simple example
// we can also provide a default state
var initialState = {
  count: 0
}

function rootReducer(state = initialState, action) {
  // default condition, this happens the first time we make the store (action {type: '@@redux/INIT'} to figure out what the initial state is)
  return state;
}

// more complex example
function rootReducer(state={}, action) {
  // we swtich on that action.type
  switch(action.type) {
    // if the action is logout user, we change state for login to be false
    // we use spread operator to make a copy of the state
    case 'LOGOUT_USER':
      return {...state, login: false}
    case 'LOGIN_USER':
      return {...state, login: true}
    // if the reducer doesn't know what to do with the action then it's always a good idea just to return the state as it is
    default:
      return state;
  }
}
```

### Creating a Store

Use the Redux createStore function which accepts the root (main) reducer as a parameter

The first time we make a store, Redux actually dispatches an action ``{type: '@@redux/INIT'}`` to see what our initial state is, so it's important that the very first time the reducer is run (which happens when the store is created) that we return some default state (see reducer function)

```javascript
const store = Redux.createStore(rootReducer);
```

### Getting the State

We can get the state of the Redux store using getState()

```javascript
// simple example
var initialState = {
  count: 0
}

function rootReducer(state = initialState, action) {
  return state
}

store.getState(); // {count: 0}

// more complex example
const store = Redux.createStore(rootReducer);
store.dispatch({
  type: 'LOGIN_USER'
});

// getting new state of our entire app
const newState = store.getState(); // {...login: true}
```

### Changing the State

The only way to change the state is by calling dispatch (an action from the store); when we dispatch an action, we go back to our reducer

Unlike React setState (which may take some time to change the applications view), store.dispatch is synchronous, so as soon as that function runs and it's finished, we know that the state has been updated to the new state

```javascript
// simple example:
function rootReducer(state = initialState, action) {
  if (action.type === 'INCREMENT') {
    // making a copy of our state so that we don't mutate it (making a function pure) and can see what our previous state looked like 
    let newState = Object.assign({}, state);
    newState.count++;
    return newState;
  }
  return state;
}

var store = Redux.createStore(rootReducer);
store.dispatch({type: 'INCREMENT'}); // store.getState(); {count: 1}

// more complex example
const store = Redux.createStore(rootReducer);
store.dispatch({
  type: 'LOGIN_USER'
}); // {...login: true}

// Because we pass the root reducer as a parameter to the store, the reducer that we saw earlier will handle this action and change the state of login to be true
```

#### Action Creators

Sometimes we don't want to pass in the entire object, so we can write a function that returns this action object (action creator)

```javascript
function increment() {
  return {
    type: 'INCREMENT'
  }
}

store.dispatch(increment());
```

### Listening for Changes

We can add a listener to see when the state has changed

```javascript
const store = Redux.createStore(rootReducer);
const changeCallback = () => {
  console.log('State has changed', store.getState());
}

const unsubscribe = store.listen(changeCallback);
// any time a state change happens in the entire store, this function will get invoked, and then we can call store.getState from within that function to figure out what the current state is (here we just use console.log)
// if we ever want to cancel that changeCallback, we'll get a function as a return from the listen function, and this unsubscribe function is what we would invoke to unsubscribe
// in setTimeout or setInterval we get an ID that we use to cancel them, but here we have a function that we use
// so whenever we want to stop that callback from happening, we just invoke the unsubscribe function
```

### Data Flow Review

1. Create a reducer with some initial state
2. Create a store
  - The reducer is run and our initial state is defined
3. dispatch(action)
    - Actions are objects and they must always have a type key
4. reducer(currentState, action)
    - Once the action is dispatched, the reducer gets invoked
5. newState
    - Based on that type and whatever the state currently is, the reducer returns a new state object
6. invoke Listeners (UI Changes)
    - After we get that new state, the store invokes all the listeners which tells any listeners that the store has been updated

---

## React with Redux

- React-Redux is a library to facilitate integrating React with Redux
- It exposes a Provider component and a connect function which we use to hook redux up to react
- Provider along with Connect handle setting up listeners and passing state to components

A good rule of thumb is if a component is only using a bit of state for that component specifically, we might not need the Redux state you might just need a little bit of React state

But if we have some data that needs to be shared amongst different components, Redux is a really good
place to put that (one centralized place)

### Installation

```bash
$npm install --save react-redux
# don't forget that we need redux installed as well
```

### Provider

Provider component is what we use from React-Redux to connect our React app with our Redux store

We previously saw that the only whing that could dispatch actions was the Redux store, but when we connect React and Redux, we actually see that any component that we want when connected to the Redux store will be able to dispatch actions on its own

```javascript
// in index.js
import React from 'react';
import ReactDom from 'react-dom';
import {Provider} from 'react-redux';
import {createStore} from 'redux';
import rootReducer from './reducers';

// create a store and pass in a root reducer (we can use createStore directly since we've imported it from redux)
// rootReducer is in a separate file
const store = createStore(rootReducer);

// similar to React Router, we're wrapping our top level app component in a provider
ReactDOM.render(
  // store is a prop to our provider
  <Provider store={store}>
    <App />
  </Provider>
  document.getElementById('root')
);
```

### Connect: Wrapping a Component and mapStateToProps

Once we've set up our store, we need a way of getting the state out of the store into our components

```javascript
// in component
import React from 'react';
import {connect} from 'react-redux';

// component BoldName which puts a strong tag around a prop 'name' (assuming we have a key of name in our state)
const BoldName = ({name}) => (
  <strong>{name}</strong>
);

// in order to pass the state into our component as a prop, we need to implement this function, it is called mapStateToProps because we are turning our Redux state into props on the React component
// the parameter to that function is the entire state of our application
// what we return is the piece of state that just this component cares about (there could be hundreds of keys in that state, but our component cares only about 'name')
// mapStateToProps always has to return an object
const mapStateToProps = state => (
  { name: state.name }
);

// in order to hook that up to our component, we export default not just BoldName, but the connect function
// it takes mapStateToProps as a parameter, and it returns a new function and with that new function we pass our component
export default connect (mapStateToProps, null)(BoldName);
```

#### Example with a Stateful Component

```javascript
class TodoList extends Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.handleChange = this.handleChange.bind(this);
    // we need React state to get that text from input and send it
    this.state = {
      task: ''
    }
  }

  handleSubmit(e) {
    e.preventDefault();
    this.props.dispatch({
      type: 'ADD_TODO',
      task: this.state.task
    });
    e.target.reset();
  }

  handleChange(e) {
    this.setState({
      [e.target.name]: e.target.value
    });
  }

  removeTodo(id) {
    this.props.dispatch({
      type: 'REMOVE_TODO',
      id
    });
  }

  render() {
    let todos = this.props.todos.map((val, index) => (
      // when we remove a todo, we want to make sure that we get the right ID for that todo that's why we are passing val.id to the removeTodo function
      <Todo removeTodo={this.removeTodo.bind(this, val.id)} task={val.task} key={index}></Todo>
    ));
    return (
      <div>
        <form onSubmit = {this.handleSubmit}>
          <label htmlFor="task">Task </label>
          <input type="text" name="task" id="task" onChange={this.handleChange}></input>
          <button>Add a Todo</button>
        </form>
        <div>
          <ul>{todos}</ul>
        </div>
      </div>
    );
  }
}

function mapStateToProps(reduxState) {
  return {
    todos: reduxState.todos
  };
}

export default connect(mapStateToProps)(TodoList);
```

In this example data flow is:

rootReducer on load -> mapStateToProps (we just got an initial state) -> it places on props the Redux state (a key of todo and whatever the value is of our todos array) -> render component -> typing a task -> component is re-rendered (because we're calling setState and changing React state) -> add a todo -> rootReducer (we just dispatched an action) -> once we return the new state we go back to mapStateToProps -> re-render the component (with our new todo)

### Connect: Dispatching an Action (mapDispatchToProps)

In the last example we saw a component that needed some state out of the store, but what about a component that need to dispatch an action

Anytime we connect a component to our Redux store, that component has the ability to dispatch actions of its own

```javascript
import React from 'react';
import {connect} from 'react-redux';

// inside of this component whenever we do 'onClick', we'll use the delName prop which is a function that dispatches an action to delete that name
const DelName = ({delName}) => (
  <button type="button" onClick={delName}>DELETE</button>
);

// it takes dispatch and the props of our component, and it returns an object
const mapDispatchToProps = (dispatch, ownProps) => (
  // the keys of that object are what props we want to pass to the component as dispatch (instead of generalized dispatch), and the values are functions
  {
    // the function typically invokes dispatch with an action
    delName: () => (dispatch({
      type: "DEL_NAME"
    }))

  }
);

// we use second parameter for connect function (mostly we import action creators, and don't write mapDispatchToProps; but don't forget to pu them here so that we could use them in props)
export default connect(null, mapDispatchToProps)(DelName);
```

#### Action Creators in React Redux

So far we've created actions like this:

```javascript
const mapDispatchToProps = dispatch => ({
  onLogout() {
    dispatch({
      type: 'USER_LOGOUT'
    })
  },
});
```

This can get cumbersome after a while because we have lots of different types floating around, and there's no one central place where they live

An alternative approach would just be to create an action creator function, and those actions would live in a folder called actions

The file with types is often called actionTypes

```javascript
const mapDispatchToProps = dispatch => ({
  onLogout() {
    dispatch(actions.userLogout())
  },
});
```

#### Example with a Stateful Component

```javascript
// this is the function that's going to allow us to dispatch actions
// under the hood, we're simply mapping "dispatch" to props by replacing a generic dispatch function with other functions that call dispatch, i.e. we won't have 'dispatch' on this.props anymore, but we will have 'addTodo'
function mapDispatchToProps(dispatch) {
  return {
    addTodo: function(task) {
      dispatch({
        type: 'ADD_TODO',
        task
      });
    }
  }
}

// ****************************
// example with action creators

// actionCreators.js
export const ADD_TODO = 'ADD_TODO';
export const REMOVE_TODO = 'REMOVE_TODO';

export function addTodo(task) {
  return {
    type: ADD_TODO,
    task
  }
}

export function removeTodo(id) {
  return {
    type: REMOVE_TODO,
    id
  }
}

// TodoList.js
import {addTodo, removeTodo} from './actionCreators';

class ... {

...

handleSubmit(e) {
    e.preventDefault();
    this.props.addTodo(this.state.task)
    e.target.reset();
  }

  removeTodo(id) {
    this.props.removeTodo(id);
  }

  ...
  
}

function mapStateToProps(reduxState) {
  return {
    todos: reduxState.todos
  };
}

// mapDispatchToProps has to return an object, so we just put an object with a bunch of keys which are simply functions (don't forget to put it here)
export default connect(mapStateToProps, { addTodo, removeTodo })(TodoList);
```

### Using Wrapped Components

```javascript
// in App.js
import React from 'react';
import BoldName from './BoldName';
import DelName from './DelName';

// we don't need to do 'connect' here since we already export our components with 'connect'
// the components are already hooked up to state and to dispatch, so we don't need to add any additional props
const App = () => (
  <div>
    <BoldName />
    <DelName />
  </div>
);
```

## Organizing Redux

https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0

Note that now Dan Abramov recommends using Hooks (https://reactjs.org/docs/hooks-custom.html)

### Presentational Component

Also known as 'dumb' component, it is a component that is primarily concerned with how things look on the screen

It is often a stateless functional component, but does not have to be one: a presentational component can still be one with state, but typically the state should be concerned with the UI, and not with data in the application

### Container Component

It is usually a stateful component that deals with application data

It is often created using higher order components like 'connect' with 'withRouter'

#### Higher Order Component

Higher Order Components (hocs) are just functions that wrap another components with something
- They are usually called 'withSomething'
- For example hoc is used to handle validation to make sure that a user is logged in before they see a component

### combineReducers

So far we've just had one big reducer function, and that's fine for smaller things, but when the app starts getting more complex, the reducer's file can get very large

combineReducers is a redux function that allows for reducer composition; each reducer is only responsible for its small piece of the entire state object

```javascript
import {combineReducers} from 'redux';
import currentUser from './currentUser';
import messages from './messages';

// we combine two separate reducers (currentUser and messages) to create our rootReducer with combineReducers
// it creates a key in our state called currentUser, and then the currentUser reducer is only concerned with data that's inside of that key (the same with messages)
const rootReducer = combineReducers({
  currentUser,
  messages,
});

export default rootReducer;
```

#### Messages Reducer

```javascript
// notice that the state we're passing in is an array so the value our reducer is concerned with doesn't have to be an object, it could be an array or it could just be a string or any other piece of JS data
const messages = (state=[], action) => {
  switch(action.type) {
    case 'LOAD_MESSAGES':
    // the data we return here is going to also be an array, not an object
      return [...action.messages];
    case 'ADD_MESSAGE':
      return [action.messages, ...state];
    default:
      return state;
  }
};

export default messages;
```

### Redux Directory Structure

In src folder (see example at warbler-client):

- components
  - Presentational components
- containers
  - Container components (connected to redux)
- hocs
  - Higher Order Components
- services
  - logic that relates to communication with our API
- store
  - Redux-related logic:
    - reducers (root reducer and other reducers)
  - actions (actions, action creators)
- index.js
  - Uses ReactDOM.render and kicks off the application
- other folders like images, etc.

---

## React Router Redux

When we use BrowserRouter, make sure it is placed inside of the Provider

For details, see 'react-redux-todos' application folder (App.js, TodoList.js, NewTodoForm.js)

```javascript
// index.js
import { BrowserRouter } from 'react-router-dom';
...
<Provider store={store}>
  <BrowserRouter>
    <App />
  </BrowserRouter>
</Provider>
```

### App.js

```javascript
import { Link, Route, Redirect } from 'react-router-dom';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h1>See Our Todos!</h1>
        <p>
          <Link to="/todos">See my todos!</Link>
        </p>
        <p>
          <Link to="/todos/new">Add a todo!</Link>
        </p>
        <Route path="/todos" component={TodoList}/>
        {/* the reason we use render is so that we can redirect right off the bat */}
        <Route exact path="/" render={() => <Redirect to="/todos" />} />
      </div>
    );
  }
}
```

### TodoList.js

```javascript
import React, { Component } from 'react';
import Todo from './Todo';
import NewTodoForm from './NewTodoForm';
import { connect } from 'react-redux';
import { addTodo, removeTodo } from './actionCreators';
import { Route } from 'react-router-dom';

class TodoList extends Component {
  constructor(props) {
    super(props);
    // adding prop to handleAdd
    this.handleAdd = this.handleAdd.bind(this);
  }

  handleAdd(val) {
    // function that dispatches an action
    this.props.addTodo(val);
  }

  removeTodo(id) {
    this.props.removeTodo(id);
  }

  render() {
    let todos = this.props.todos.map((val, index) => (
      // when we remove a todo, we want to make sure that we get the right ID for that todo that's why we are passing val.id to the removeTodo function
      <Todo removeTodo={this.removeTodo.bind(this, val.id)} task={val.task} key={index}></Todo>
    ));
    return (
      <div>
        {/* {/* when we are on /todos/new, we're going to render some component and pass some kind of props (we want these props to use react router in this component, to be able to access history of this.props) */}
        {/* we render NewTodoForm and we're going to pass in all of these props as well as another prop handleSubmit (it's going to be whatever this handleAdd does) */}
        <Route
          path="/todos/new"
          // From answers:
          // We're simply passing a prop called component which is a reference to the NewTodoForm component. The Route component will use the component prop passed to it to render the NewTodoForm, it will pass history and other props to the NewTodoForm in it's render function
          // because we needed to add his own props he had to wrap everything in a stateless functional component (a function) 
          // so now first the Route component will pass all the props to the stateless functional component above (the arrow function), then the arrow function will return the NewTodoForm component passing in all those props plus also the handleSubmit prop. this.props does not equal props, props is what will be passed to the component from Route
          component={props => (
            <NewTodoForm {...props} handleSubmit={this.handleAdd} />
          )}
        />
        {/* whenever we go to this path /todos, we're going to render a div with all todos */}
        <Route exact path="/todos" component={() => <div>{todos}</div>} />
      </div>
    );
  }
}

function mapStateToProps(reduxState) {
  return {
    todos: reduxState.todos
  };
}

export default connect(mapStateToProps, { addTodo, removeTodo })(TodoList);
```

### NewTodoForm.js

```javascript
import React, { Component } from 'react';

export default class NewTodoForm extends Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.handleChange = this.handleChange.bind(this);
    // we need React state to get that text from input and send it
    this.state = {
      task: ''
    }
  }

  handleSubmit(e) {
    e.preventDefault();
    // we don't want to connect this component to Redux
    // instead of calling: this.props.addTodo(this.state.task) we're going to make sure we pass down a prop from TodoList (called handleSubmit) and pass the task
    this.props.handleSubmit(this.state.task);
    e.target.reset();
    // we make sure we redirect back to the list of todos; this comes from {...props} that we passed with React Router
    this.props.history.push('/todos');
  }

  handleChange(e) {
    this.setState({
      [e.target.name]: e.target.value
    });
  }
  
  render() {
    return (
      <form onSubmit = {this.handleSubmit}>
        <label htmlFor="task">Task </label>
        <input type="text" name="task" id="task" onChange={this.handleChange} />
        <button>Add a Todo</button>
      </form>
    );
  }
}
```

## React + Backend

Instead of storing all the data in memroy, we need to connect our frontend to backend; in order to do that, we need to make AJAX requests from (in this case) from localhost:3000 to localhost:3001 (client -> server)

Redux doesn't have any built-in support for our actions to be asynchronous, actions are always objects

Although there is a way to dispatch async actions, but to do that we need some Redux middleware called Redux Thunk (redux-thunk)

### Redux Thunk

Redux think allows us to write action creators that return a function instead of an action

#### Thunk

A think is a function that wraps an expression to delay its evaluation

```javascript
// calculation is immediate
let x = 1 + 2;

// calculation is delayed; foo can be called later to perform the calculation
// foo is a thunk
let foo = () => 1 + 2;
```

What we're going to do here is wait until the AJAX call has finished to then dispatch an action (we delay the dispatch of an action) as opposed to dispatching it right away

#### Usage of Redux Thunk

```javascript
// index.js
import { createStore, applyMiddleware, compose } from 'redux';
// applyMiddleware is going to allow us to use the redux middleware (specifically thunk)
import thunk from 'redux-thunk';

// the 2nd param to createStore is a combination of middleware that we're using so we're going to use the 'compose' function to bring that all together into one function
const store = createStore(
  rootReducer,
  compose(
    // first thing we want to pass in to compose is applyMiddleware - it is a function that accepts some kind of middlware
    applyMiddleware(thunk),
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
  )
);
```

#### actionCreators.js

```javascript
// before:
export const ADD_TODO = 'ADD_TODO';
export const REMOVE_TODO = 'REMOVE_TODO';

export function addTodo(task) {
  return {
    type: ADD_TODO,
    task
  }
}

export function removeTodo(id) {
  return {
    type: REMOVE_TODO,
    id
  }
}

// now:
export const ADD_TODO = 'ADD_TODO';
export const REMOVE_TODO = 'REMOVE_TODO';
export const GET_TODOS = 'GET_TODOS';

// gets todos; data is some data that we get back from our backend
function handleTodos(data) {
  return {
    type: GET_TODOS,
    data
  }
}

function handleAdd(todo) {
  return {
    type: ADD_TODO,
    todo
  }
}

function handleRemove(id) {
  return {
    type: REMOVE_TODO,
    id
  }
}

// we export async functions that use redux thunk and will be used inside of our react components
// we make a function that is action creator, but it returns a function instead of an object
export function getTodos() {
  // function(dispatch) {}
  // this function we're returning executes async action
  return dispatch => {
    // inside of here we run some king of AJAX call
    return fetch('http://localhost:3001/api/todos')
      // get response and convert it into json
      .then(res => res.json())
      // when async action is completed, we're dispatching a function (here: that simple action creator handleTodos) which accepts some information that we got back and returns an object
      .then(data => dispatch(handleTodos(data)))
      // here we really should dispatch something, but this is simply to handle errors
      .catch(err => console.log('Something went wrong!', err));
  }
}

export function addTodo(task) {
  return dispatch => {
    return fetch('http://localhost:3001/api/todos', {
      method: 'POST',
      headers: new Headers({
        'Content-Type': 'application/json'
      }),
      body: JSON.stringify({ task }) // task: task, this is what we're sending to server
    })
      // when this is done, we're going to get some data back
      .then(res => res.json())
      // once we got the data back and converted it into json, we're dispatching an action (we go to our reducer with that new todo that we made and got back from the server, then go to mapsStateToProps and updating our React component) 
      .then(data => dispatch(handleAdd(data)))
      .catch(err => console.log('Something went wrong!', err));
  }
}

export function removeTodo(id) {
  return dispatch => {
    return fetch(`http://localhost:3001/api/todos/${id}`, {
      method: 'DELETE'
    })
    .then(res => res.json())
    .then(data => dispatch(handleRemove(id)))
    .catch(err => console.log('Something went wrong!', err));
  };
}
```

#### rootReducer.js

```javascript
import { ADD_TODO, REMOVE_TODO, GET_TODOS } from './actionCreators';

const initialState = {
  todos: [],
  // id: 0 - we don't need this since mongoose is keeping track of our ids
}

export default function rootReducer (state = initialState, action) {
  switch (action.type) {
    case GET_TODOS:
      // we're passing whatever we have in the state along with todos which is action.data (since we pass inside of our action creator some kind of data)
      return {...state, todos: action.data}
    case ADD_TODO:
      // we don't need this code anymore
      // let newState = {...state};
      // newState.id++;
      // return {
      //   ...newState,
      //   todos: [...newState.todos, {task: action.task, id: newState.id}]
      // };
      return {...state, todos: [...state.todos, action.todo]}
    case REMOVE_TODO:
      // don't forget to use mongo's _id
      let todos = state.todos.filter(val => val._id !== action.id);
      return {...state, todos};
    default:
      return state;
  }
}
```

#### TodoList.js

```javascript
import { addTodo, removeTodo, getTodos } from './actionCreators';

...

// when this component loads, we want to go to the database and get all of our todos
  componentDidMount() {
    // we don't need .then, etc. since it all is already inside of getTodos thunk
    this.props.getTodos();
  }

...

export default connect(mapStateToProps, { addTodo, removeTodo, getTodos })(TodoList);
```