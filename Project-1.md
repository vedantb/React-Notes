## COUNTER APP

This project is perfect if you are totally new to React. I will go in details with every single thing we do, so that in the next projects you’ll be up to speed with the basics.

### WHAT WE'LL BUILD

In this project, we’ll build a very simple example of a counter.

We are going to have a simple web page with 4 buttons, and a place where we show the count.

The count starts at zero, and the buttons we’ll add will increment the count by 1, 10, 100, 1000 depending on which button is pressed.

We’re going to associate one of those values to a button, and we will show it in the button text.

### WHAT DOES THIS PROJECT TEACH YOU

You will learn how to use the `create-react-app`, you will write your first Components, your first JSX, you'll work with props and events.

Those are the building blocks for complex web applications too, so before getting into the code, I want you to open those pages and read the details.

1. Introduction to create-react-app
2. Introduction to React Components
3. Introduction to JSX
4. Introduction to Props
5. Introduction to React Events

### BOOTSTRAP A REACT APP USING CREATE REACT APP

With those bits of theory in mind, we can now start creating our app.

We can work locally by using `npx create-react-app counter-react`, then go in the folder and run `npm run start`.

Alternatively, we can do the same on CodeSandbox. Just go to https://codesandbox.io/s , click React and start building right away.


## START WITH THE BUTTON COMPONENT

The React application created by `create-react-app` has a single component, `App`. CodeSandbox keeps it in `src/index.js`.

As I mentioned one of the main building blocks of the application is a button.

We’re going to have 4 of them, so it makes perfect sense to separate that, and move it to its own component:

```jsx
const Button = () => {

}
```

The component will render a button:

```jsx
const Button = () => {
  return <button>...</button>
}
```

and inside of this button we must show the number this button is going to increment our count of. We’ll pass this value as a prop:

```jsx
const Button = props => {
  return <button>+{props.increment}</button>
}
```

Notice how I changed the function signature from `const Button = () => {}` to `const Button = props => {}`. This is because if I don’t pass any parameter, I must add `()` but when I pass one single parameter I can omit the parentheses.

Now add `import React from 'react'` on top, and `export default Button` at the bottom, and save this to `src/components/Button.js`. We need to import React because we use JSX to render our output, and the export is a way to make Button available to components that import this file (later on, App):

```jsx
import React from 'react'

const Button = props => {
  return <button>+{props.increment}</button>
}

export default Button
```

Our Button component is now ready to be put inside the App component output, and thus show on the page.

## SHOW THE INTERFACE

This is the App component at this point, stored in the file `src/index.js`:


```jsx
import React from "react"
import ReactDOM from "react-dom"

import "./styles.css"

function App() {
  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
    </div>
  )
}

const rootElement = document.getElementById("root")
ReactDOM.render(<App />, rootElement)
```

Let’s remove all that’s inside the `div` rendered by App.


```jsx
import React from "react"
import ReactDOM from "react-dom"

import "./styles.css"

function App() {
  return (
    <div className="App">
    </div>
  )
}

const rootElement = document.getElementById("root")
ReactDOM.render(<App />, rootElement)
```

We now import the Button compnent from the Button file:

```js
import Button from './components/Button'
```

and we can now use Button inside our App component, and thus show on the page:

```jsx
function App() {
  return (
    <div className="App">
      <Button />
    </div>
  )
}
```

We pass the `increment` prop to it, so it can show that value inside the button text:

```jsx
function App() {
  return (
    <div className="App">
      <Button increment={1} />
    </div>
  )
}
```

You should now see a button showing up in the page, with a `+1` text on it.

Let’s add 3 more buttons:

```jsx
function App() {
  return (
    <div className="App">
      <Button increment={1} />
      <Button increment={10} />
      <Button increment={100} />
      <Button increment={1000} />
    </div>
  )
}
```

and finally we add a counter, which our buttons will increment when clicked.

We store the counter result in the `count` variable, which I declare as a `let`

We then print this variable in the JSX:

```jsx
function App() {
  let count = 0

  return (
    <div className="App">
      <Button increment={1} />
      <Button increment={10} />
      <Button increment={100} />
      <Button increment={1000} />
      <span>{count}</span>
    </div>
  )
}
```

## INCREMENTING THE COUNT

Great! Let’s now add the functionality that lets us change the count by clicking the buttons, by adding a `onClickFunction` prop. We pass that to the Button component, and we use an `onClick` event handler that intercepts automatically the clicks made on the button, and it calls the `handleClick` function:

```jsx
const Button = ({ increment, onClickFunction }) => {
  const handleClick = () => {
    onClickFunction(increment)
  }
  return <button onClick={handleClick}>+{increment}</button>
}
```

When `handleClick` runs, it calls the `onClickFunction` prop, which in turn will refer back to the App component for an attached handler.

Here’s App with the new `onClickFunction` prop defined on the Button components:

```jsx
function App() {
  let count = 0

  const incrementCount = increment => {
    //TODO
  }

  return (
    <div className="App">
      <Button increment={1} onClickFunction={incrementCount} />
      <Button increment={10} onClickFunction={incrementCount} />
      <Button increment={100} onClickFunction={incrementCount} />
      <Button increment={1000} onClickFunction={incrementCount} />
      <span>{count}</span>
    </div>
  )
}
```

Here, every Button element has 2 props: `increment` and `onClickFunction`. We create 4 different buttons, with 4 increment values: 1, 10 100, 1000.

When the button in the Button component is clicked, the `incrementCount` function is called.

This function must increment the local count. How can we do so? We can use hooks.

We import `useState` from React, and we call:

```jsx
const [count, setCount] = useState(0)
```

when we need to use define our count. Notice how I removed the `count` `let` variable now. We substituted it with a React-managed state.

In the `incrementCount` function, we call `setCount()`, incrementing the count value by the increment we are passed.

Why we didn’t just update the count value, why do we need to call `setCount()`? Because React depends on this convention to manage the rendering (and re-rendering) of components. Instead of watching all variables and spend time and resource trying to “spy” on values and do something when they change, it tell us to just use the set* function, and let it handle the rest.

`useState()` initializes the count variable at 0 and provides us the `setCount()` method to update its value.

We use both in the `incrementCount()` method implementation, which calls `setCount()` updating the value to the existing value of count, plus the increment passed by each Button component.


```jsx
import React, { useState } from 'react'
import ReactDOM from 'react-dom'
import Button from './components/Button'

import './styles.css'

function App() {
  const [count, setCount] = useState(0)

  const incrementCount = increment => {
    setCount(count + increment)
  }

  return (
    <div className="App">
      <Button increment={1} onClickFunction={incrementCount} />
      <Button increment={10} onClickFunction={incrementCount} />
      <Button increment={100} onClickFunction={incrementCount} />
      <Button increment={1000} onClickFunction={incrementCount} />
      <span>{count}</span>
    </div>
  )
}

const rootElement = document.getElementById('root')
ReactDOM.render(<App />, rootElement)
```

## CHALLENGES FOR YOU

Here are the challenges I propose for the counter app:

1. Add a “reset” button to restore the count to zero
2. Add a set of buttons to decrement the count
3. Add another button that saves the result of the count to a list of results. This way the page can serve as a sort of calculator that memorizes the previous calculations.