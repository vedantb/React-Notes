### Introduction To React Events

React provides an easy way to manage events. Prepare to say goodbye to `addEventListener`.

In the previous article about the State you saw this example:

```jsx
const CurrencySwitcher = props => {
  return (
    <button onClick={props.handleChangeCurrency}>
      Current currency is {props.currency}. Change it!
    </button>
  )
}
```

If you’ve been using JavaScript for a while, this is just like plain old JavaScript event handlers, except that this time you’re defining everything in JavaScript, not in your HTML, and you’re passing a function, not a string.

The actual event names are a little bit different because in React you use camelCase for everything, so `onclick` becomes `onClick`, `onsubmit` becomes `onSubmit`.

For reference, this is old school HTML with JavaScript events mixed in:
```html
<button onclick="handleChangeCurrency()">...</button>
```

#### Event handlers

It's a convention to have event handlers defined as methods on the Component class:

```jsx
class Converter extends React.Component {
  handleChangeCurrency = event => {
    this.setState({ currency: this.state.currency === '€' ? '$' : '€' })
  }
}
```

All handlers receive an event object that adheres to the spec.

#### Bind `this` in methods

Don’t forget to bind methods. The methods of ES6 classes by default are not bound. What this means is that `this` is not defined unless you define methods as arrow functions:

```jsx
class Converter extends React.Component {
  handleClick = e => {
    /* ... */
  }
  //...
}
```

when using the property initializer syntax with Babel (enabled by default in `create-react-app`), otherwise you need to bind it manually in the constructor:

```jsx
class Converter extends React.Component {
  constructor(props) {
    super(props)
    this.handleClick = this.handleClick.bind(this)
  }
  handleClick(e) {}
}
```

