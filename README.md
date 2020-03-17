# react-hooks

There are two important rules to follow when writing React functional components with hooks.

1. Use the hooks at the top level of your functions, without wrapping them in `if` statements, loops, or other types of control logic.

2. Only call hooks from React functions or custom hooks that start with the word `use` - so that you and other developers know that those functions are hooks, and can use them appropriately.

## :page_facing_up: 목차
* [useState](#usestate)
* [Custom Hook](#custom-hook)
* [useEffect](#useeffect)
* [useContext](#usecontext)
* [useReducer](#usereducer)
* [useMemo](#usememo)
* [useRef](#useref)

## useState

```jsx
import React, { useState } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

const NameFunc = () => {
  const [name, changeName] = useState('')

  return (
    <div>
      <p>What's your name?</p>
      <input 
        value={name}
        onChange={e => changeName(e.target.value)}
      />
      <p>{name && `Hello, ${name}!`}</p>
    </div>
  )
}

class Name extends React.Component {

  state = {
    name: ''
  }

  render() {
    return (
      <div>
        <p>What's your name?</p>
        <input 
          value={this.state.name}
          onChange={e => this.setState({name: e.target.value})}
        />
        <p>{this.state.name && `Hello, ${this.state.name}!`}</p>
      </div>
    )
  }
}

function App() {
  return (
    <div className="App">
      <h2>React Hooks</h2>
      <NameFunc />
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
[https://jesfg.csb.app/](https://jesfg.csb.app/)

## Custom Hook

```jsx
import React, { useState } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

const useInput = (initialValue) => {
  const [name, changeName] = useState(initialValue)

  const changeNameWithInput = (e) => {
    changeName(e.target.value)
  }

  return [name, changeNameWithInput]
}

const Name = () => {
  const [name, changeName] = useInput('')

  return (
    <div>
      <p>What's your name?</p>
      <input 
        value={name}
        onChange={changeName}
      />
      <p>{name && `Hello, ${name}!`}</p>
    </div>
  )
}

function App() {
  return (
    <div className="App">
      <h2>React Hooks</h2>
      <Name />
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
````
[https://mcmt2.csb.app/](https://mcmt2.csb.app/)

## useEffect

```jsx
import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

const Counter = () => {
  const [count, setCount] = useState(0)
  const [posts, setPosts] = useState([])

  useEffect(() => {
    setCount(c => c + 1)
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(response => response.json())
      .then(json => setPosts(json))
  }, [])

  return (
    <div>
      <p>Count: {count}</p>
      {
        posts &&
        posts.map(post => {
          return <p key={post.id}>{post.title}</p>
        })
      }
    </div>
  )
}

function App() {
  return (
    <div className="App">
      <h2>React Hooks</h2>
      <Counter />
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
[https://wg7yx.csb.app/](https://wg7yx.csb.app/)

## useContext
```jsx
import React, { useState, useContext } from "react";
import ReactDOM from "react-dom";
import "./styles.css";

const UserContext = React.createContext()

const UserDetails = () => {
  const { user, setUser } = useContext(UserContext)

  return (
    <div>
      <p>User: {user}</p>
      <button onClick={() => setUser(null)}>Logout</button>
    </div>
  )
}

const Login = () => {
  const { user, setUser } = useContext(UserContext)
  const [loginName, setLoginName] = useState('')

  return (
    <div>
      <input 
        value={loginName}
        onChange={e => setLoginName(e.target.value)}
        placeholder="Login Name"
      />
      <button onClick={() => setUser(loginName)}>Login</button>
    </div>
  )
}

function App() {
  const [user, setUser] = useState()

  return (
    <UserContext.Provider value={{ user, setUser }}>
      <div className="App">
        <h2>React Hooks</h2>
        {user ? <UserDetails /> : <Login />}
      </div>
    </UserContext.Provider>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
[https://b1wo1.csb.app/](https://b1wo1.csb.app/)

## useReducer
```jsx
import React, { useState, useReducer } from "react";
import ReactDOM from "react-dom";
import "./styles.css";

const myReducer = (state, action) => {
  switch(action.type) {
    case 'changeName':
      return {...state, name: action.name}
    case 'changeColor':
      return {...state, color: action.color}
    case 'changeQuest':
      return {...state, quest: action.quest}
  }
}

function App() {
  const [state, dispatch] = useReducer(myReducer, {})

  return (
    <div className="App">
      <h2>React Hooks</h2>
      
      <input
        placeholder="What is your name?"
        value={state.name}
        onChange={e => dispatch({
          type: 'changeName',
          name: e.target.value
        })}
      />

      <input
        placeholder="What is your favorite color?"
        value={state.color}
        onChange={e => dispatch({
          type: 'changeColor',
          color: e.target.value
        })}
      />

      <input
        placeholder="What is your quest?"
        value={state.quest}
        onChange={e => dispatch({
          type: 'changeQuest',
          quest: e.target.value
        })}
      />

      <p>Name: {state.name}</p>
      <p>Color: {state.color}</p>
      <p>Quest: {state.quest}</p>

    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
[https://p6igx.csb.app/](https://p6igx.csb.app/)

## useMemo
```jsx
import React, { useState, useMemo } from "react";
import ReactDOM from "react-dom";
import "./styles.css";

function App() {
  const [count, setCount] = useState(0)
  const [name, setName] = useState('')

  const label = useMemo(() => {
    console.log("computing clicks", Date.now())
    return `${count} click${count === 1 ? '' : 's'}`
  }, [count])

  return (
    <div className="App">
      <h2>React Hooks</h2>

      <input 
        placeholder="Name"
        value={name}
        onChange={e => setName(e.target.value)}
      />
      
      <button onClick={() => setCount(count + 1)}>
        Press Me
      </button>
      
      <p>{name} has {label}</p>

    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
[https://d5vqy.csb.app/](https://d5vqy.csb.app/)

## useRef
```jsx
import React, { useRef } from "react";
import ReactDOM from "react-dom";
import "./styles.css";

function App() {
  const divEle = useRef(null)

  const checkDims = () => {
    let dims = divEle.current.getBoundingClientRect()
    console.log("x, y", dims.x, dims.y)
    console.log("height, width", dims.height, dims.width)
  }

  return (
    <div className="App">
      
      <div style={{
        width: 100,
        height: 50,
        backgroundColor: 'orange'
      }} 
        ref={divEle}
      />

      <button onClick={checkDims}>
        Check Dimensions
      </button>

    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
[https://8iu5f.csb.app/](https://8iu5f.csb.app/)

:memo: **참고 자료**    
[https://egghead.io/playlists/using-react-hooks-in-functional-components-be394e6c](https://egghead.io/playlists/using-react-hooks-in-functional-components-be394e6c)
