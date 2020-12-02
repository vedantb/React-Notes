## GITHUB USERS APP

In this project we’re going to create a form. We wait for the user to type in a GitHub username, and when this happens we ask to the GitHub API the user’s public information, and we show a nice user card.

This project will teach you two new things compared to the previous one:

- iterations in JSX
- how to handle a simple form in React
- how to perform network requests and update the state of the application when we get information back

This is what we will end up creating:

// TODO ADD SCREENSHOT

### CREATE A LIST OF CARDS

Let's create a reusable **Card** component to show a single user data.

This component will display our image and details as gathered from Github. It gets its data via props, using

- `props.avatar_url` the user avatar
- `props.name` the user name
- `props.blog` the user website URL

Let's make a simple version of it. We style it using the `style` property of each HTML element. The syntax uses two curly braces, like this:

```jsx
<div style={{ margin: '1em' }}>
```

This is because one of the braces pair is to input a JavaScript value in the JSX, like we did for `count` in the previous project.

The other one is to create an object with the CSS properties:

```jsx
const Card = (props) => {
  return (
    <div style={{ margin: "1em" }}>
      <img alt="avatar" style={{ width: "70px" }} src={props.avatar_url} />
      <div>
        <div style={{ fontWeight: "bold" }}>{props.name}</div>
        <div>{props.blog}</div>
      </div>
    </div>
  );
};
```

You can check out the [official docs for style](https://reactjs.org/docs/dom-elements.html#style). This method is a quick way to add CSS without adding classes or a separate CSS file, although we’ll later use those methods as this one is only suited for quick needs or to prototype. In any case, it’s good to know how to use it.

We create a list of those `Card` components in a `CardList` component.

A parent component will provide it a list of cards using props, and CardList will take care of iterating over them and render a `Card` component for each one:

```jsx
const CardList = ({ cards }) => (
  <div>
    {cards.map((card) => (
      <Card {...card} />
    ))}
  </div>
);
```

We use `map()` to iterate on the array.

Here's the full code of those 2 first components:

`src/components/Card.jsx:`

```jsx
import React from "react";

const Card = ({ avatar_url, name, blog }) => {
  return (
    <div style={{ margin: "1em" }}>
      <img alt="avatar" style={{ width: "70px" }} src={avatar_url} />
      <div>
        <div style={{ fontWeight: "bold" }}>{name}</div>
        <div>{blog}</div>
      </div>
    </div>
  );
};

export default Card;
```

`src/components/CardList.js:`

```jsx
import React from "react";
import Card from "./Card.js";

const CardList = ({ cards }) => (
  <div>
    {cards.map((card) => (
      <Card {...card} />
    ))}
  </div>
);

export default CardList;
```

### ASKING GITHUB FOR THE INFORMATION

Cool! We must have a way now to ask GitHub for the details of a single username. We’ll do so using a `Form` component, where we manage our own state (`username`), and we ask GitHub for information about a user using their public APIs, via the [Axios](HTTP-Requests-Using-Axios.md) library.

Let's create a new `Form` component:

```jsx
const Form = (props) => {
  return <form></form>;
};
```

Our form hosts a single input element, and a button. Let’s add them to the JSX:

```jsx
const Form = (props) => {
  return (
    <form>
      <input type="text" placeholder="GitHub username" required />
      <button type="submit">Add card</button>
    </form>
  );
};
```

We make form handle a piece of state called `username`. As we did in the previous project, we’re going to use hooks.

When the username is updated, we are notified in the `onChange()` event callback. In there, we update the username value by calling `setUsername()`:

```jsx
import React, { useState } from "react";

//...

const Form = (props) => {
  const [username, setUsername] = useState("");

  return (
    <form>
      <input
        type="text"
        value={username}
        onChange={(event) => setUsername(event.target.value)}
        placeholder="GitHub username"
        required
      />
      <button type="submit">Add card</button>
    </form>
  );
};
```

Next, we handle the form submit event, which is emitted when the user click the “Add card” button, automatically because the button is `type="submit"`.

We add an `onSubmit` attribute to the form, and we attach an `handleSubmit` function callback:

```jsx
const Form = (props) => {
  const [username, setUsername] = useState("");

  const handleSubmit = (event) => {
    //...
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={username}
        onChange={(event) => setUsername(event.target.value)}
        placeholder="GitHub username"
        required
      />
      <button type="submit">Add card</button>
    </form>
  );
};
```

In this method we first call `event.preventDefault()` to avoid the browser submitting the form to the server (which is the default behavior) and we call the Axios library `axios.get()` method, to get the person information from GitHub.

We need to install and import Axios first:
`npm install axios`

(or install it on CodeSandbox by clicking the “Add Dependency” button)

then add

```js
import axios from "axios";
```

The `axios.get()` method returns a promise, so we can add a function callback as the `.then()` argument, and when it’s invoked it means we got our data back.

When this happens, we call the `onSubmit` prop, which is passed to us by the App component (we’ll add it later).

We also reset the username:

```jsx
handleSubmit = (event) => {
  event.preventDefault();

  axios.get(`https://api.github.com/users/${username}`).then((resp) => {
    props.onSubmit(resp.data);
    setUsername("");
  });
};
```

Here’s the complete component code:

```jsx
import React, { useState } from "react";
import axios from "axios";

const Form = (props) => {
  const [username, setUsername] = useState("");

  const handleSubmit = (event) => {
    event.preventDefault();

    axios.get(`https://api.github.com/users/${username}`).then((resp) => {
      props.onSubmit(resp.data);
      setUsername("");
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={username}
        onChange={(event) => setUsername(event.target.value)}
        placeholder="GitHub username"
        required
      />
      <button type="submit">Add card</button>
    </form>
  );
};

export default Form;
```

To recap, when you enter a name in the `input` field managed by the **Form** component, this name is bound to its state.

When Add card is pressed, the input form is cleared by clearing the `userName` state of the **Form** component.

The example uses, in addition to React, the Axios library. It’s a nice useful and lightweight library to handle network requests.

When the form is submitted we call the `handleSubmit` event, and after the network call we call `props.onSubmit` passing the parent (`App`) the data we got from GitHub.

### FINALIZE THE APP

Now we can import the Form component in the App component, the main component of the application.

Remember that in the Form component we require a `onSubmit` prop. We must pass a function to this prop, so that we can add the new card to the list of cards.

Where is the list of cards maintained? In the App component state.

So we first include useState, so we can use hooks, and we call it to generate the array of cards. We initialize the state property to `[]`, an empty array.

We add it to `App`, passing a method to add a new card to the list of cards, `addNewCard`, as its `onSubmit` prop:

```jsx
import React, { useState } from "react";
import Form from "./components/Form";
import CardList from "./components/CardList";

const App = () => {
  const [cards, setCards] = useState([]);

  addNewCard = (cardInfo) => {
    //...
  };

  return (
    <div>
      <Form onSubmit={addNewCard} />
      <CardList cards={cards} />
    </div>
  );
};
```

When addNewCard is called, we are getting the information in its only parameter, `cardInfo`. We can call `setCards()`, passing a new array of cards which we get by calling `concat()` on cards. This method does not alter cards, instead it returns a new array with the old cards plus the new one:

```jsx
addNewCard = (cardInfo) => {
  setCards(cards.concat(cardInfo));
};
```

This is what our App component looks like:

```jsx
import React, { useState } from "react";
import Form from "./components/Form";
import CardList from "./components/CardList";
import "./styles.css";

export default function App() {
  const [cards, setCards] = useState([]);

  let addNewCard = (cardInfo) => {
    setCards(cards.concat(cardInfo));
  };

  return (
    <div>
      <Form onSubmit={addNewCard} />
      <CardList cards={cards} />
    </div>
  );
}
```

That’s it! The application should now be fully working.

https://codesandbox.io/s/reactformexample-3vbqb

## CHALLENGES FOR YOU

Here are the challenges I propose for the GitHub Users app:

1. Add a link to the person’s GitHub account in the card
2. Add a button to remove a card from the list
3. Search on every keypress rather than on the form submit event. If the current input box content matches a user, show it “on hover”, and only add it to the list when clicked (beware of API limits)
