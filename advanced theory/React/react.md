# React: a Front-end Framework

#### Table of Contents

- [React: a Front-end Framework](#react-a-front-end-framework)
      - [Table of Contents](#table-of-contents)
  - [Front-end Framework](#front-end-framework)
  - [React](#react)
    - [Creating a Component](#creating-a-component)
  - [JSX](#jsx)
    - [Babel](#babel)
    - [Using a JSX](#using-a-jsx)
    - [JSX with JS and CSS](#jsx-with-js-and-css)
    - [Multiple Components](#multiple-components)
      - [Refactoring](#refactoring)
  - [Create React App](#create-react-app)
    - [Webpack](#webpack)
    - [Using Create React App](#using-create-react-app)
    - [Files Created by CRA](#files-created-by-cra)
  - [React Props](#react-props)
    - [Passing in Props to a Component](#passing-in-props-to-a-component)
    - [Props Are Immutable](#props-are-immutable)
    - [defaultProps](#defaultprops)
    - [propTypes](#proptypes)
    - [props.children](#propschildren)
  - [State](#state)
    - [setState](#setstate)
      - [Update Complex State Example](#update-complex-state-example)
  - [Component Architecture](#component-architecture)
    - [Shared State](#shared-state)
    - [Who Owns the State?](#who-owns-the-state)
      - [Bad Practice](#bad-practice)
    - [Stateless Functional Component](#stateless-functional-component)
    - [setState Can Be Tricky](#setstate-can-be-tricky)
      - [setState That Depends on Previous State](#setstate-that-depends-on-previous-state)
      - [setState Is Asynchronous](#setstate-is-asynchronous)
  - [Virtual DOM](#virtual-dom)
    - [Synthetic Events](#synthetic-events)
    - [React 16 (Fiber)](#react-16-fiber)
      - [Error Boundary](#error-boundary)
      - [Fiber](#fiber)
  - [Events](#events)
    - [onClick](#onclick)
  - [Forms](#forms)
    - [Uncontrolled Component](#uncontrolled-component)
    - [Controlled Component](#controlled-component)
    - [Controlled Component with Update](#controlled-component-with-update)
    - [Submitting a Form](#submitting-a-form)
  - [Refs](#refs)
  - [Component Lifecycle Methods](#component-lifecycle-methods)
    - [Mounting](#mounting)
    - [Unmounting](#unmounting)
    - [Updating](#updating)
    - [Forcing Update](#forcing-update)
    - [Use Cases of Lifecycle Methods](#use-cases-of-lifecycle-methods)
      - [componentWillUnmount](#componentwillunmount)
      - [componentDidMount](#componentdidmount)
  - [React Router](#react-router)
    - [HTML5 History Object](#html5-history-object)
      - [Server Rendering](#server-rendering)
      - [Cliend Side Rendering (React)](#cliend-side-rendering-react)
      - [Handling a Bookmark](#handling-a-bookmark)
    - [Intro to Router](#intro-to-router)
    - [BrowserRouter vs HashRouter](#browserrouter-vs-hashrouter)
    - [ReactRouter Install](#reactrouter-install)
    - [Switch And Route](#switch-and-route)
    - [Link](#link)
    - [NavLink](#navlink)
    - [URL Parameters](#url-parameters)
    - [Route Props](#route-props)
    - [withRouter](#withrouter)
    - [Route: Render vs Component](#route-render-vs-component)

## Front-end Framework

- JS libraris that handle DOM manipulation
- Handles navigations (HTML5 push state which allows to change the address bar with JS and without refreshing the page)
- State management (keeping track of all the data)

They are run on the client side, in the browser

GET/ -> index.html -> GET /bundle.js -> bundle.js (includes JS for framework, and now React is in control of the page and makes decisions about DOM manipulation)

## React

- Released by Facebook in 2013
- A view library that uses composable (reusable) components (concerned with displaying stuff on the screen)
- Other libraries are commonly used with React
  - React Router which deals with navigation in our app
  - Redux which is a single place to store the state in the app
- React is used in many places: React Native, VR, etc., that's why React and ReactDOM are separated

All of the pieces of the app should be in its own component (core of React philosophy)

### Creating a Component

Conceptually, components are like JavaScript functions; they accept arbitrary inputs (called 'props') and return React elements describing what should appear on the screen

This is not the code we'll be writing typically, but this is React at its lowest level, just HTML and JS, nothing else

```javascript
<script src="https://unpkg.com/react@16.0.0/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@16.0.0/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/react-dom-factories@1.0.0/index.js"></script>

<div id="app"></div>
  <script type="text/javascript">
    // every component extends .Component
    class Pet extends React.Component {
      // all components have a render method, its goal is to return some HTML element that we want to put in the DOM using a factory
      render() {
        // first attribute is attributes, the second is content
        const h2 = ReactDOMFactories.h2(null, "Moxie");
        const img = ReactDOMFactories.img({
          src: "https://github.com/tigarcia/Moxie/raw/master/moxie.png",
          alt: "Moxie my cat"
        });
        // with React, we can never return multiple components next to each other, we have to put it inside of one DOM element
        const cardDiv = ReactDOMFactories.div(null, h2, img);
        return cardDiv;
      }
    }
  
  // after we've created component, we have to render it in the DOM
  // first - element that we want to render (that creates HTML element out of our Pet component)
  // second - decide where in the DOM we want to put it (div with id = app)
  ReactDOM.render(React.createElement(Pet), document.getElementById("app"));
</script>
```

## JSX

JSX simplifies writing components (otherwise we need to create every DOM element with these big long functions)

It looks like returning HTML from within our JS code
- All JSX tags must be closed

### Babel

Babel is a transpiler: it converts from one type of source code to another

Babel started as a compiler from ES5 to ES6, but after a while browsers started implementing a lot of ES6 features, so it changed its goal into a general purpose transpiler

Now in react we use Babel to convert JSX into vanilla JS (we don't have to write the react factories code because JSX will be a replacement for that)

### Using a JSX

It usually is not done this way (we transpile code before the page load)

```javascript
// replace factories with Babel
<script src="https://unpkg.com/react@16.0.0-rc.2/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@16.0.0-rc.2/umd/react-dom.development.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.26.0/babel.js"></script>

// replace script type to type="text/babel"
<div id="app"></div>
  <script type="text/babel">
    class Pet extends React.Component {
      // all components have a render method, its goal is to return some HTML element that we want to put in the DOM using a factory
      render() {
        return (
          <div>
            <h2>Moxie</h2>
            <img
              src="https://github.com/tigarcia/Moxie/raw/master/moxie.png" 
              alt="Moxie my cat" />
              // can be either self-closing or or closed with a separate tag
          </div>);
      }
    }

    ReactDOM.render(<Pet />, document.getElementById("app"));
</script>
```

### JSX with JS and CSS

```javascript
  <script type="text/babel">
    class Pet extends React.Component {
      render() {
        // style is always a JS object representing CSS (notice camelCase and '')
        const liStyle = {fontSize: '1.5em'};
        // Babel doesn't know the difference between CSS class and JS class, that's why we use className in HTML
        return (<div className="card">
                  <h2 className="name">Moxie</h2>
                  <img src="https://github.com/tigarcia/Moxie/raw/master/moxie.png" />
                  // using inline styles
                  // in React inline styles are not bad because everything is bundled up as one
                  // so we don't have that confustion in HTML when we might have a class doing one thing and inline style doing another thing
                  <h5 style={{fontSize: '2em', margin: '2px'}}>Hobbies:</h5>
                  // anytime we include {}, we're saying 'I want to evaluate JS here', so when we include styles, we need double {} (one for object, one for JSX)
                  // and it is not limited to styles only (we can do something like {5 + 5}, and '10' will be rendered)
                  <ul>
                    <li style={liStyle}>Sleeping</li>
                    <li style={liStyle}>Eating</li>
                  </ul>
                </div>);
      }
    }

    ReactDOM.render(<Pet />, document.getElementById("app"));
  </script>
```

### Multiple Components

```javascript
<div id="app"></div>
<script type="text/babel">
  class Pet extends React.Component {
    render() {
      const style = {fontSize: '1.5em'};
      const hobbies = ['Sleeping', 'Eating', 'Cuddling'];
      return (<div className="card">
                <h2 className="name">Moxie</h2>
                <img src="https://github.com/tigarcia/Moxie/raw/master/moxie.png" />
                <h5 style={{fontSize: '2em', margin: '2px'}}>Hobbies:</h5>
                <ul>
                // we can use {} inside of JSX to render this array into JSX li's
                // every time we render an array in react, we need to give each element a unique key (should have smth to do with the data we're displaying) which is important for react's rendering
                  {hobbies.map((hobby, index) => {
                    // usually using index is not ok since array could be changed, but here it's alright
                    return <li key={index} style={style}>{hobby}</li>
                  })}
                </ul>
              </div>);
    }
  }

  ReactDOM.render(<Pet />, document.getElementById("app"));
</script>
```

#### Refactoring

```javascript
<div id="app"></div>
<script type="text/babel">
  class HobbyList extends React.Component {
    render() {
      const style = {fontSize: '1.5em'};
      const hobbies = ['Sleeping', 'Eating', 'Cuddling'];
      return (
        <ul>
          {hobbies.map((hobby, index) => {
            return <li key={index} style={style}>{hobby}</li>
          })}
        </ul>
      );
    }
  }

  class Pet extends React.Component {
    render() {
      return (
        <div className="card">
          <h2 className="name">Moxie</h2>
          <img src="https://github.com/tigarcia/Moxie/raw/master/moxie.png" />
          <h5 style={{fontSize: '2em', margin: '2px'}}>Hobbies:</h5>
          // using HobbyList inside of our Pet component for it to render
          <HobbyList />
        </div>
      );
    }
  }

  ReactDOM.render(<Pet />, document.getElementById("app"));
</script>
```

## Create React App

It is a tool which helps to build React apps without knowing much about Webpack (it comes with pre-configured webpack)

### Webpack

It is a module bundler for modern JS applications

- Combines different JS files into a single bundle.js
- Has a plugin system to run tools like babel
- Also bundles other assets like css, images, etc.
- Separate components (e.g. pet.css contains styles only for pet.js, and is different from app files)

### Using Create React App

```bash
$npm i -g create-react-app
$create-react-app <projectname>
```

### Files Created by CRA

- package.json (with npm install all of the needed packages)
  - react-scripts doing the webpack config for us
- public
  - contains files that do not go through the webpack build process
  - index.html
    - basis of the website, page that will be delivered to the client
    - contains div #root - we attach our app component to it (in index.js)
- src
  - index.js is our render file
  - app.js is our app component

## React Props

Props are immutable data passed to our components

When React sees an element representing a user-defined component, it passes JSX attributes to this component as a single object - we call this object 'props'

Accessible in our components as an object calls: this.props

```javascript
class ShowText extends Component {
  render() {
    // inside this render methos, we have access to this.props (this - ShowText instance); text is our data
    // since this is a normal javascript object with a property when we put it in the curly braces here the string that comes back will be the string is displayed in the view
    return <div>{this.props.text}</div>;
  }
}
```

### Passing in Props to a Component

```javascript
<ShowText
  text="This is a prop named text"
/>
```

### Props Are Immutable

```javascript
class ShowText extends Component {
  render() {
    // never ever change this.props
    this.props.text = 'Wrong!'; // TypeError
    this.props = {}; // Never do this, it breaks react!
    this.props.newProp = 'Also wrong!'; // Use default props, never create a new property that doesn't exist on this.props
    return <div>{this.props.text}</div>
  }
}
```

### defaultProps

Default values for props in a component

```javascript
class IngredientList extends Component {
  static defaultProps = {
    // expect an array of ingredients, and if a user doesn't provide an ingredients prop, we set a default value
    // we use static because defaultProps isn't something that is specific to one instance of the component, it is shared for all components
    ingredients: []
  }

  render() {
    return (
      <ul>
        {this.props.ingredients.map((ing, index) => (<li key={index}>{ing}</li>))}
      </ul>
    )
  }
}
```

This also works:

```javascript
class IngredientList extends Component {
  render() {
    return (
      <ul>
        {this.props.ingredients.map((ing, index) => (<li key={index}>{ing}</li>))}
      </ul>
    )
  }
}

// we can set this after we've defined a class, it's equivalent to 'static' inside of a class
IngredientList.defaultProps = {
  ingredients: []
};
```

More complex example:

```javascript
class App extends Component {
  static defaultProps = {
    recipes: [{
      title: "Spaghetti",
      ingredients: ["flour", "water"],
      instructions: "Mix ingredients",
      img: "spaghetti.jpg"
    }]
  }
  render() {
    return (
      <div>
        {this.props.recipes.map((r, index) => (
          <Recipe key={index} {...r} />
          // this is a shorthand rest operator for (use it only if you know EXACTLY what you're doing so that you don't pass props to a component that you don't need to or mean to pass):
          // <Recipe key={index} title={r.title}
          //         ingredients={r.ingredients}
          //         img={r.img} instructions={r.instructions}
          // />
        ))}
      </div>
    );
  }
}
```

### propTypes

Development time (only works when our app is running in development (not in production) mode) type checker for our props
- npm i --save prop-types
- They specify requirements for our props for users of that component

https://facebook.github.io/react/docs/typechecking-with-proptypes.html

```javascript
import PropTypes from 'prop-types';

class IngredientList extends Component {
  static propTypes = {
    // instead of setting a default value, we say that this ingredients array must be an array, and it must be an array of strings, and this array is required
    ingredients: PropTypes.arrayOf(PropTypes.string)
                          .isRequired
  }
  render() {
    return (
      <ul>
        {this.props.ingredients.map((ing, index) => (
          <li key={index}>{ing}</li>
        ))}
      </ul>
    );
  }
}
```

### props.children

It is a collection of the children inside of a component

It is a collection that we have access to inside our component

http://mxstbr.blog/2017/02/react-children-deepdive/

```javascript
// we want to make a row component where we can just put the row tag around some other content, and it will be displayed in a row

class App extends Component {
  render() {
    return (
      <Row>
        <p>Timothy</p>
        <div>Moxie</div>
        <h1>React</h1>
      </Row>
    );
  }
}

class Row extends Component {
  render() {
    return (
      <div style={{
        display: 'flex',
        flexDirection: 'row',
        justifyContent: 'space-around',
      }}>
        // it is a collection of all those elements that are inside, different types of things (here it is a p, a div, and an h1)
        {this.props.children}
      </div>
    )
  }
}
```

## State

Stateful data, data in our application that can change (remember, we can't change props, but we can change state)
- We can't modify state directly
- State is similar to props, but it is private and fully controlled by the component

```javascript
class App extends Component {
  constructor(props) {
    // every constructor takes props as parameter and has super(props) which calls constructor of the Component that we're inheriting from
    super(props);
    this.state = { favColor: 'red' };
  }
  render() {
    return (
      <div>
        My favorite color:
        // normal JS object
        {this.state.favColor}
      </div>
    );
  }
}
```

### setState

The correct way to change state on our application

Simplest usage: setState accepts an object with new properties and values for this.state:
- ``this.setState({   });``
- We should NEVER modify state directly, just like props, we need to call setState for this
- this.setState is ASYNC function
- setState will eventually invoke the render method, so whenever a setState is called, the state will eventually be updated and render will be invoked
  - When render is invoked, it will render a new view, so the DOM will be updated
- Changes to state should be pure (see pure functions), i.e. we should never change object in this.state, we should give it new objects for our state

```javascript
class App extends Component {
  constructor(props) {
    super(props);
    this.state = { favColor: 'red' };

    setTimeout(() => {
      this.setState({favColor: 'blue'})
    }, 3000);
  }
  render() {
    return (
      <div>
        My favorite color:
        {this.state.favColor}
      </div>
    );
  }
}
```

#### Update Complex State Example 

Selecting one of the instructors and removing one of their hobbies after 5 sec

```javascript
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      instructors: [
        {
          name: 'Tim',
          hobbies: ['sailing', 'react']
        }, {
          name: 'Matt',
          hobbies: ['math', 'd3']
        }, {
          name: 'Colt',
          hobbies: ['css', 'hiking']
        }, {
          name: 'Elie',
          hobbies: ['music', 'es2015']
        }
      ]
    };
    
    setTimeout(() => {
      // finding random instructor
      const randInst = Math.floor(
        Math.random() * this.state.instructors.length
      );
      
      // finding random hobby
      const hobbyIndex = Math.floor(
        Math.random() * this.state.instructors[randInst].length
      );
      
      // updating state

      // do NOT do this:
      // const instructors = this.state.instructors.slice();
      // instructors[randInst].hobbies.splice(hobbyIndex, 1); - modifies original array
      // this isn't a deep copy, slice just gives us a new array with all the same elements inside
      // so this object that we're accessing inside of our instructors array is still the same state, and so this code is modifying our state

      // one approach:
      const instructors = this.state.instructors.slice();
      // we create a copy of that object inside of our instructors array (which is OK to be modified since it's our own array)
      instructors[randInst] = Object.assign({}, instructors[randInst]);
      // even though we made a copy of this instructor's objects, the array that we're referencing inside that object still is the original array, so we need to copy that as well
      instructors[randInst].hobbies = instructors[randInst].hobbies.slice();
      // now we can use splice, since all the data that we have inside this instructor instance is our new version of the old data
      instructors[randInst].hobbies.splice(hobbyIndex, 1);
      this.setState({instructors}); // shorthand notation

      // *****************************

      // easier approach:
      const instructors = this.state.instructors.map((inst, i) => {
        // checking if we've found the instructor
        if (i === randInst) {
          // creating a copy of hobbies array by spreading the values (we can use inst.hobbies.slice())
          const hobbies = [...inst.hobbies];
          hobbies.splice(hobbyIndex, 1);
          return {
            // we return the new object while keeping all the keys in that object, that's why we use spread, and then overwriting hobbies
            ...inst,
            hobbies
          };
        }
        // if we haven't found the needed instructor, we just return the original data
        return inst;
      });
      this.setState({instructors});
    }, 5000);
  }
  
  render() {
    const instructors = this.state.instructors.map((instructor, index) => (
      <li key={index}>
        <h3>{instructor.name}</h3>
        <h4>Hobbies: {instructor.hobbies.join(", ")}</h4>
      </li>
    ));
    return (
      <div className="App">
        <ul>
          {instructors}
        </ul>
      </div>
    );
  }
}
```

## Component Architecture

https://facebook.github.io/react/docs/thinking-in-react.html

Idea where we should put our state in our app, and how props and state should interact

### Shared State

- State is always passed from a parent DOWN to a child component as a prop
- State should not be passed to a sibling or a parent

```javascript
// see prevoius example
// instead of creating li's here in render, we create a child component, and we pass state from that parent component as props

// the way it was before in app component
render() {
    const instructors = this.state.instructors.map((instructor, index) => (
      <li key={index}>
        <h3>{instructor.name}</h3>
        <h4>Hobbies: {instructor.hobbies.join(", ")}</h4>
      </li>
    ));
    return (
      <div className="App">
        <ul>
          {instructors}
        </ul>
      </div>
    );
  }

// creating a child component
class InstructorItem extends Component {
    static propTypes = {
    name: PropTypes.string,
    hobbies: PropTypes.arrayOf(PropTypes.string)
  };
  render() {
    return (
      <li>
        // these props can never change inside of the component
        <h3>{this.props.name}</h3>
        <h4>
          Hobbies: {this.props.hobbies.join(", ")}
        </h4>
      </li>
    );
  }
}

// inside of our app component:
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      instructors: [
        {
          name: 'Tim',
          hobbies: ['sailing', 'react']
        }, {
          name: 'Matt',
          hobbies: ['math', 'd3']
        }, {
          name: 'Colt',
          hobbies: ['css', 'hiking']
        }, {
          name: 'Elie',
          hobbies: ['music', 'es2015']
        }
      ]
    };
    
    setTimeout(() => {
      const randInst = Math.floor(
        Math.random() *
        this.state.instructors.length
      );
      
      const hobbyIndex = Math.floor(
        Math.random() *
        this.state.instructors[randInst].length
      );
      
      const instructors = this.state.instructors.map((inst, i) => {
        if (i === randInst) {
          const hobbies = [...inst.hobbies];
          hobbies.splice(hobbyIndex, 1);
          return {
            ...inst,
            hobbies
          };
        }
        return inst;
      });
      this.setState({instructors});
    }, 5000);
  }
  
  render() {
    // whenever setState is called, render is callsed, and when it is called, there is a new value for the state
    // so this.state.insturctors will render new instructor items
    // any instructor item that has different values, wil get re-rendered in the DOM with the new set of props
    // props can never change, but a component can be unmounted and remounted with a new value if the props do change on a new render
    const instructors = this.state.instructors.map((instructor, index) => (
      // we're taking the state from this.state.instructors and passing it down to a child component as props
      <InstructorItem
        key={index}
        name={instructor.name} // inside InstructorItem it is a prop
        hobbies={instructor.hobbies} // a prop
      />
    ));
    return (
      <div className="App">
        <ul>
          {instructors}
        </ul>
      </div>
    );
  }
}

```

### Who Owns the State?

State should be owned by 1 component

```javascript
// tic tac toe app
class App extends Component {
  render() {
    return(
      <div>
        <Navbar />
        <TicTacToe />
      </div>
    );
  }
}
```

It would make sense for all the state to be owned by TicTacToe component because TicTacToe is the component that cares about X's and O's, and who won and who lost

But let's say we want a button on a navbar to restart the game, and restarting the game affects the state of the tic tac toe board, so Navbar would want some state about the game

And in this case we've got state in TicTacToe and we also need state in Navbar, and these are two siblings, and we can't share state between siblings

The common solution is if two components both need some state, that state needs to be pushed up to a parent, so the App component will own state and pass that data down into the TicTacToe component as props


#### Changing State in a Parent Component

If you need to change state in a parent component, do it with callbacks:

```javascript
// parent component
handleInput = (inputValue) => {
  this.setState = ({
    value: inputValue,
  });
}

...

render() {
  return <ChildComponent handleInput={this.handleInput} />
}

// child component
// where you need to call your function:
this.props.handleInput(newValue);
```

#### Bad Practice

- Never duplicate our state and never assign a prop to our state

This is an example of that same InstructorItem component, except this time for whatever reason the InstructorItem has some event, and it wants to modify the state of that instructor (maybe it wants to be able to edit the name, or it wants to be able to add a new hobby)

```javascript
class InstructorItem extends Component {
  constructor(props) {
    super(props);
    // often people want to do something like this
    this.state = {
      name: this.props.name, // comes from the props
      hobbies: this.props.hobbies // comes from the props
    };
    // we should never do this because what we're trying to do here is essentially copy some data that came in as a prop to state
    // typically this is not a good sign that we're duplicating state somewhere, and that's always a bad thing
  }
  render() {
    return (
      <div>
        <h3>{this.state.name}</h3>
        <h4>
          Hobbies: {this.state.hobbies.join(", ")}
        </h4>
      </div>
    );
  }
}
```

### Stateless Functional Component

Components implemented using a function, not a class (we're no longer extending component, we're just writing a function)

The function implements the render method only: no constructor, no state

It's a good idea to turn any component that doesn't have any state into a stateless functional component

```javascript
import React from 'react';

// props is always a parameter
const Greeting = props => {
  <h1>Hello, {props.name}</h1>
};

export default Greeting;
```

Refactoring our example to make it a stateless functional component

```javascript
const InstructorItem = props => {
  return (
    <li>
      <h3>{props.name}</h3>
      <h4>
        Hobbies: {props.hobbies.join(", ")}
      </h4>
    </li>
  );
}

// if we want to use prop types
InstructorItem.propTypes = {
  name: PropTypes.string,
  hobbies: PropTypes.arrayOf(PropTypes.string)
};
```

### setState Can Be Tricky

#### setState That Depends on Previous State

When a setState depends on previous state, use a function parameter rather than just passing an object

```javascript
this.state = { counter: 1 };

// we might do something like this
this.setState({
  counter: this.state.counter + 1
});

// but if we have a couple of setStates (adding one more), the value after these setStates will be 2 (not 3)
this.setState({
  counter: this.state.counter + 1
});

// setState is asynchronous

// we can think if setState is doing this:
// when there are multiple setStates that are called that are depending on the previous state, it's kind of like doing Object.assign
// in this assignment, counter will still be 2 because this.state.counter never changes in all of these Object.assign
Object.assign({},
  {counter: this.state.counter + 1},
  {counter: this.state.counter + 1},
  {counter: this.state.counter + 1},
)

// solution: update function
// setState can actually take an update function rather than an object
// this is what we want to use if the state that we're updating depends on a previous value for the state
this.setState((prevState, props) => {
  // our goal is to return an object for new state
  return {
    counter: prevState.counter + 1
  };
});
```

#### setState Is Asynchronous

```javascript
this.setState({name: 'Tim'});
// won't be updated yet
console.log(this.state.name);

// the correct way to do that (we can also use promises or async/await instead of a callback)
// 1 param is an object (or update function), 2 param is a callback which will only be invoked after setState is completed
this.setState({name: 'Tim'}, () => {
  console.log(
    'Now state is up to date',
    this.state.name
  );
});
```

## Virtual DOM

It is a data structure stored by React that tracks changes from one render state to the next

If something has changed from one render to the next, the browser's DOM is updated (reconciliation)

What this means is that React keeps track of all the changes we make when setState is called, and then it decides how to change the DOM based on what happens in virtual DOM

Reconciliation is the process of 'figuring out' what changes happened and what needs to be updated in the DOM (e.g. component unmounting), so we don't have to worry about all the details of updating the DOM, we just describe the changes in a declarative way (we go from one state to another state and then React figures out how to make these changes happen in the DOM)

### Synthetic Events

Synthetic events are similar to browser events (all the same synthetic events exist as native browser events), but the main difference is that synthetic event API is the same across any browser, it smooths out all those API differences (provides a consistent API on all browsers)

We still can access native events, but this is rarely done

### React 16 (Fiber)

Render can return an array of JSX elements or a string

```javascript
// in React 15 it wasn't possible, we had to return one high level component and then put all our components inside of that
render() {
  return [
    <div key='a'>First Element</div>
    <div ket='b'>Second Elements</div>
  ];
}
```

#### Error Boundary

https://facebook.github.io/react/blog/2017/07/26/error-handling-in-react-16.html

We can isolate components in our app with an error boundary

#### Fiber

The new reconciliation engine (the majority is the same as far as the API is concerned, but there are some new cool abilities)

https://www.youtube.com/watch?v=ZCuYPiUIONs

## Events

We add any types of event by adding it directly to the component

### onClick

```javascript
class ClickExample extends Component {
  constructor(props) {
    super(props);
    this.state = {name: 'Tim'};
  }
  render() {
    return (
      <div>
        <p>{this.state.name}</p>
        <button type="button"
          onClick={() => this.setState({name: 'TIM'})}>
          UPPERCASE
        </button>
      </div>
    )
  }
}
```

onClick with a function as part of our class:

```javascript
class ClickExample extends Component {
  constructor(props) {
    super(props);
    this.state = {name: 'Tim'};
    // we need to bind 'this' because 'this' otherwise wouldn't refer to our component
    this.handleClick = this.handleClick.bind(this);
  }
  // there is no noticeable performance benefits of using bind or arrow functions, we can use both
  // although in some cases, arrow functions lead to extra re-rendering
  handleClick(e) {
    this.setState((prevState, props) => ({name: prevState.name.toUpperCase()}));
  }
  render() {
    return (
      <div>
        <p>{this.state.name}</p>
        // this pattern is used a lot: we make come callback handlers for events and then put those events inside of JSX using this.method
        // notice that we DO NOT put () after handleClick - we don't want to invoke the function immediately
        <button type="button" onClick={this.handleCLick}>
          UPPERCASE
        </button> 
      </div>
    )
  }
}
```

## Forms

### Uncontrolled Component

It is a component that React doesn't have contorl over

```javascript
<input type="text" />
// react is not aware of what the user is typing, the browser is in charge of the state
```

### Controlled Component

```javascript
<input type="text" value={this.state.inputText} />
// react is now in control of the state via this.state.inputText, it renders it, but the input can't be updated (we can't type anything)
```

### Controlled Component with Update

```javascript
<input
  type="text"
  name="inputText"
  value={this.state.inputText}
  onChange={(e) => {
    this.setState({inputText: e.target.value})
  }}
/>

// react is now in control of the state via this.state.inputText and the state can change via onChange
// onChange will be invoked every time we type a single input
// react always knows the exact state of that input at any time
```

### Submitting a Form

```javascript
<form onSubmit={(e) => {
  // this is standard for the browser, if we don't preventDefault, it will submit any request to server and the whole page will refresh -> we will lose all our state
  e.preventDefault();
  // updating data array which keeps track of all the inputs that we've submitted
  const data = [...this.state.data, this.state.inputText];
  // updating the state and clearing the form
  this.setState({data, inputText: ''});
}}>
  <input
    type="text"
    name="inputText"
    value={this.state.inputText}
    onChange={(e) => {
      this.setState({inputText: e.target.value})
    }}
  />
</form>

// we shouldn't do like this because there are a lot of behaviors in the browser that don't get covered by this click event
<button onClick={/* trying to handle submit here */} type="submit">Save</button>
```

## Refs

Ref is a direct reference to a DOM element (all the things before haven't referenced the DOM directly, we just provided state to react and then react decided what to do with the DOM)

Use cases:
- Managing focus, text selection, or media playback
- Triggering imperative animations
- Integrating with third-party DOM libraries

Do not use refs when the job can be done with react - we should not need direct DOM access for most tasks

```javascript
<form onSubmit={(e) => {
  e.preventDefault();
  // access to the form value
  console.log(this.inputText.value);
}}>
  // leaving input uncontrolled
  <input
    type="text"
    // saving a reference to that DOM element into our component
    ref={(input) => this.inputText = input}
  />
</form>
```

## Component Lifecycle Methods

https://reactjs.org/docs/state-and-lifecycle.html

See here: https://reactjs.org/blog/2018/03/27/update-on-async-rendering.html for migrating from legacy lifecycles to new ones (getDerivedStateFromProps and getSnapshotBeforeUpdate)

### Mounting

Mounting happens when the component is first rendered in the DOM

- constructor()
- componentWillMount()
  - inside of our component we can implement componentWillMount, and that will tell us then the constructor is finished, but we haven't called render() yet
  - called only once per the lifetime of the component
  - used for some setup things
  - LEGACY, AVOID
- render()
- componentDidMount()
  - will be invoked after our component markup has been placed in the DOM
  - called only once per the lifetime of the component
  - used for some setup things

### Unmounting

Unmounting happens when our component gets taken out of the DOM (e.g. if the component was in an array of elements and then that element got deleted)

- componentWillUnmount()
  - we might use it if we want to clean up something inside of our component (e.g. if we started a setInterval, we can cancel it here)

### Updating

This happens whenever setState is called

- componentWillReceiveProps(nextProps)
  - if any of our props have changed
  - LEGACY, AVOID
- shouldComponentUpdate(nextProps, nextState)
  - we can implement ourselves, and if we return false from this method, the component will not render
  - it's a way to short circuit React's normal behavior
  - used in special cases when we need to optimize our code, sometimes it leads to more problems than solutions
- componentWillUpdate(nextProps, nextState)
  - it gets called right before the render so we can see what props and state are about to be updated
- render()
  - new state and props
  - LEGACY, AVOID
- componentDidUpdate(prevProps, prevState)
  - we can see previous props and state
  - used for logging, for example

### Forcing Update

Skips shouldComponentUpdate and forces a render

- forceUpdate(callback)
  - should be avoided in most cases


### Use Cases of Lifecycle Methods

#### componentWillUnmount

```javascript
const NUM_BOXES = 32;
class Boxes extends Component {
  constructor(props) {
    super(props);
    const boxes = Array(NUM_BOXES).fill()
      .map(this.getRandomColor, this);
    this.state = {boxes};

    // saving intervalId to the component instance
    this.intervalId = setInterval(() => {
      const boxes = this.state.boxes.slice();
      const ind = Math.floor(Math.random()*boxes.length);
      boxes[ind] = this.getRandomColor();
      this.setState({boxes});
    }, 300);
  }

  // removing setInterval
  componentWillUnmount() {
    clearInterval(this.intervalId);
  }
}
```

#### componentDidMount

Common use case is making an AJAX request because the componentDidMount will only happen once per the life of the component (like getting all the data for our view)

```javascript
// app which gets top stories from the HackerNews

import React, { Component } from 'react';
import './App.css';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      stories: []
    };
  }
  
  componentDidMount() {
    const topStories = 'https://hacker-news.firebaseio.com/v0/topstories.json';
    const storyUrlBase = 'https://hacker-news.firebaseio.com/v0/item/';
    
    fetch(topStories)
      .then(data => data.json())
      // promise chaining
      .then(data => data.map(id => {
        const url = `${storyUrlBase}${id}.json`;
        return fetch(url).then(d => d.json());
        // returns an array of promises
      }))
      .then(promises => Promise.all(promises))
      // promises are resolved, we have an array of stories
      .then(stories => this.setState({stories}));
  }
  
  render() {
    let views = <div>Loading...</div>;
    const {stories} = this.state;
    // we only want to map if we have some data for the stories, if there are no stories - we keep the loading view, that's a good UI
    if (stories && stories.length > 0) {
      views = stories.map(s=> (
        <p key={s.id}>
          <a href={s.url}>{s.title}</a> from <strong>{s.by}</strong>
        </p>
      ));
    }
    
    return (
      <div className="App">
        <h2>Hacker News Top Stories</h2>
        {views}
      </div>
    );
  }
}

export default App;
```

## React Router

React Router allows us to conditionally show different components on the page based on the URL

### HTML5 History Object

HTML5 introduced the history object which allows javascript to manipulate the browser history
- Changes the URL address bar
- Changes the browser's local navigation history
- DOES NOT cause the browser to make a GET request (page doesn't reload)

It is used
- To create a good single page application, we want to have it feel like a normal website
- Browser back button, internal links, etc should all seem to behave like a page with server rendering

```javascript
history.back();
history.forward();
history.pushState({}, 'title', '/newpage'); // changes the address bar and history
// 1 param is a state object, some data that we want to tie to the new change in the history object, 3 - route
// we can check where we are
window.location;
```

#### Server Rendering

Old way of webdev, client doesn't do much, server does the rendering of the display

Client (HTML, CSS) -> GET/ -> Server (all the logic) -> index.html (fully rendered)

Client (HTML, CSS) -> GET /signin -> Server (all the logic) -> /signin (fully rendered)

#### Cliend Side Rendering (React)

Client (HTML, CSS, a lot of JS) -> GET/ -> Server -> index.html (only div id="root"; empty HTML file with just that div) -> Render react app (React grabs onto that div and renders its page; once React loads there's content on the page) -> Change address to /signin -> Render /signin component (no GET request, React handles rendering all by itself)

React Router will look at the location of the website/signin and decide which components to render (client (not server) decides what to render on what route)

#### Handling a Bookmark

Another thing we want to be able to do is to bookmark a URL or share a URL from our website

So if we bookmark some profile page like /user/55, by loading the bookmark we'll make a get request to /user/55 and server is going to return the exact same index.html with nothing inside but div #root (the index.html that gets returned should be the same no matter what valid URL we're asking for)

And on the React side, it (based on the URL we asked for) renders the specific component to match that URL

The key thing for handling this bookmark problem is that the server must support this, it has to return the same html file no matter what the route is

Now if we also have an API that we're working with, there is a way to proxy requests for certain routes, but for the most part all the valid routes that we care about are going to return our HTML template

### Intro to Router

It is a library to manage routing in our single page applications; version 4 is a declarative api that uses component to make rendering decisions

https://reacttraining.com/react-router/web/guides/philosophy

### BrowserRouter vs HashRouter

BrowserRouter uses the history object and makes changes to the URL, the hash router adds hashes to the URL instead

<table>
  <tr>
    <td>Browser Router (modifies history, changes URL)</td>
    <td>HashRouter (doesn't modify history, it adds a hash)</td>
  </tr>
  <tr>
    <td>/</td>
    <td>/#</td>
  </tr>
  <tr>
    <td>/users</td>
    <td>/#users</td>
  </tr>
  <tr>
    <td>/users/57492/messages</td>
    <td>/#users/57492/messages</td>
  </tr>
<table>

Hash router can make sense of this hash in order to re-render the page, so bookmarking still works
- Some people think it's a weird user experience

BrowserRouter requires server support whereas HashRouter does not

Choose BrowserRouter if possible

### ReactRouter Install

```bash
$npm install --save react-router-dom
# there are other types of react routers
```

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import {BrowserRouter as Router} from 'react-router-dom';
import App from './App';

ReactDOM.render(
  <Router>
    <App />
  <Router>,
  document.getElementById('root');
)
```

### Switch And Route

Conditionally rendering something based on the URL

```javascript
import React, {Component} from 'react';
import {Switch, Route} from 'react-router-dom';

// two components
const Homepage = () => (<div>HOMEPAGE</div>);
const About = () => (<div>ABOUT</div>);

const SwitchDemo = () => (
  // switch makes those routes 'if-else': if the first one renders, even if the second one matches, it won't be rendered
  // we don't need it, but it's nice two have so that we don't accidentaly render two components matching the same URL
  <Switch>
    <Route path="/about" component={About}/>
    <Route path="/" component={Homepage}/>
  </Switch>
);
```

### Link

Link component is essentially just an anchor tag except when we click that link, React Router takes over and rather than doing the default link behavior, the link will use the history object to change the URL

```javascript
import React from 'react';
import {Link} from 'react-router-dom';
import SwitchDemo from './SwitchDemo'; // from above

const App = () => (
  <div>
    <Link to="/">HOME</Link>
    <Link to="/about">ABOUT</Link>
    <div style={{fontSize: '3em', margin: '25px'}}>
      <SwitchDemo/>
    </div>
  </div>
);
```

### NavLink

Similar to the link component, it's designed for navigation

It has additional prop called activeStyle, and whenever we're currently on some path, activeStyle will be active

```javascript
import React from 'react';
import {NavLink} from 'react-router-dom';
import SwitchDemo from './SwitchDemo';

// these would be in render() in class
const s = {color: "red"}; // active style, when we are on some link, it will be colored in red
const defaultStyle = {color: "blue"}; // default style

// this will be return
const App = () => (
  <div>
    <NavLink exact style={defaultStyle} activeStyle={s} to="/">
      HOME
    </NavLink>
    <NavLink exact style={defaultStyle} activeStyle={s} to="/about">
      ABOUT
    </NavLink>
    <div style={{fontSize: '3em',margin: '25px'}}>
      <SwitchDemo/>
    </div>
  </div>
);
```

### URL Parameters

If we have some parameter that we care about within the URL, we can add a colon and then parameter

```javascript
import React from 'react';
import {
  Switch, Route
} from 'react-router-dom';

const Homepage = () => (<div>HOMEPAGE</div>);
const Name = ({match}) => (
  // if we want to render whatever is in that route (or use whatever is in that route), we can use match.params
  <div>Hello, {match.params.name}</div>
);
const SwitchDemo = () => (
  <Switch>
    // match.params.name is the string that matches from this route
    // so if we have a path like /Tim, this component would say Hello Tim
    <Route path="/:name" component={Name}/>
    <Route path="/" component={Homepage}/>
  </Switch>
);
```

### Route Props

A component inside of a Route (rendered from within a Route) gets 3 props
- match - info about how the current url matches the route component (path inside the route)
- location - where we are now, similar to window.location
- history - similar to html5 history object, allows explicit changes to the url (go back, change url)

### withRouter

If a component is not rendered inside of a Route component, we can use withRouter to get route props (wrap it in withRouter function)

```javascript
import {
  withRouter, Switch, Route
} from 'react-router-dom';

const SwitchDemo = ({history}) => (
  <div>
    <Switch>
      <Route path="/:name" component={Name}/>
      <Route path="/" component={Homepage}/>
    </Switch>
    // button is not inside of the Route component
    <button onClick={() => history.push('/')}>
      Go Home
    </button>
  </div>
);
export default withRouter(SwitchDemo);
// it will return a new component that gets all there of these props
```

### Route: Render vs Component

Another problem that we run into is that when we use a route component: if we want to pass additional props to the component that we end up rendering, the component property won't allow us to do that

To solve this problem we use render (insted of component) to pass custom props to our component

The route component can either use render or component (never both)

Route Render example:

```javascript
import {Route} from 'react-router-dom';
const teachers = ['Tim', 'Colt', 'Matt', 'Elie'];

// component called Teachers expects a teachers prop
const Teachers = ({teachers}) => (
  <ul>
    {teachers.map((teach, ind) => (
      // looping over the array and displaying each teacher in an li
      <li key={i}>{teach}</li>
    ))}
  </ul>
);

// inside of our main app we want to use that Teachers component, but we also want to pass that teachers prop in
const App = () => (
  // in order for that to work, we'll use render rather than component
  // render takes a stateless functional component
  // 1 param is props (the props that we're going to get); those props are the props that we get inside of our Route component like match, location, and history
  <Route path="/teachers" render={props => (
    // when we render that component, we want to spread out our props so that this Teachers component will get match, location, and history, and we also want to provide our additional props (the teachers array here)
    <Teachers {...props} teachers={teachers} />
  )}/>
);
```
