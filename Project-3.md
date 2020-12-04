## PIXEL DRAWING APP

You’ve probably seen a similar application already. I have a few of them on my iPad. It’s a cool way to relax, by drawing something cool.

This is what we aim for:

![Pixel Drawing App](https://thereactcourse.com/giaKcDqEO00cTaLABN3F/pixel-art/1-intro/end-result.png)

---

## WHAW WE'LL BUILD IN DETAIL

The goal of this tutorial is to build a canvas, a 30x30 “pixels” matrix. When the app starts, there is a blank canvas showing up.

There is also a color picker, which shows the colors you can use. A default color is pre-selected when the app starts.

When you click one pixel in the canvas, that pixel will be colored with that default color.

---

## THE MAIN COMPONENTS OF THE APP

Given the UI that is showed above, we basically need 3 components.

1. One is the **Pixel**, a single unit that will compose our drawing.
2. One is the **Canvas**, which is a set of 30 rows, each containing 30 Pixel components.
3. The last one is the **ColorPicker**, which contains one or more Pixels components.

Clicking a Pixel component inside a ColorPicker will change the active color.

Clicking a Pixel component inside a Canvas will change the background color of the pixel.

---

## BOOTSTRAP A REACT APP USING CREATE-REACT-APP

We can now start creating our app. Let's start up a project using the create-react-app or a code sandbox react project.

---

## THE FIRST COMPONENT, PIXEL

Create a `components` folder in the `src` folder, and inside it create an empty `Pixel.js` file.

We’re going to import React, then we create a function component that simply outputs a `div` with two classes: `pixel` and the `background` property passed as a prop:

```jsx
import React from "react";

export default (props) => {
  return <div className={`${props.background} pixel`} />;
};
```

I use an HTML class to determine the look of the component. Remember that React outputs any value found in the `className` attribute in JSX as the HTML `class` attribute. The name differs from “regular HTML” because `class` is a reserved word in JavaScript.

I style those classes using CSS, in `src/styles.css`, which is already existing, I define 4 colors, which I’ll use as the base for the app example:

```css
.white {
  background-color: white;
}
.lightblue {
  background-color: rgb(0, 188, 212);
}
.blue {
  background-color: rgb(3, 169, 244);
}
.darkblue {
  background-color: rgb(33, 150, 243);
}
```

Where are these 4 colors defined? I define them in a `Colors.js` module, because they are used in various places.

```js
export default ["white", "lightblue", "blue", "darkblue"];
```

The first color is the base color, and defines the color used by all cells when the app starts. The others are 3 choices I give to color the pixels.

Now let’s just add this one pixel in the app. Open `src/App.js` and after all the imports add

```js
import Pixel from "./components/Pixel";
```

and inside the `App()` function, make the component return one Pixel:

```jsx
return (
  <div className="App">
    <Pixel />
  </div>
);
```

Let’s add a bit of styling to `src/styles.css` to make our Pixel actually appear on screen, by setting its width/height, and the border:

```css
.pixel {
  width: 30px;
  height: 30px;
  border: 1px solid lightgray;
  box-sizing: border-box;
}
```

In your browser now you will see a little pixel showing up:

![Image of Pixel](https://thereactcourse.com/giaKcDqEO00cTaLABN3F/pixel-art/2-pixel/pixel.png)

---

## CREATE THE CANVAS COMPONENT

Now we'll take the Pixel component and use it as a base for a grid of pixels, which will be the canvas of our application. **Canvas**, therefore seems a good name for the new component.

Create an empty file in `src/components/Canvas.js`

We want the canvas to show a 30x30 grid composed by Pixel components.

The components stores this grid in its state.

How we construct this grid? This piece of JavaScript creates a row of 30 elements:

```js
Array(30).fill();
```

I want it to be initialized with 0 , so they all default to the our default color, so I use map() for this:

```js
Array(30)
  .fill()
  .map(() => 0);
```

This works for one row.

For one single row, I could pass `0` as a parameter to `fill()`:

```js
Array(30).fill(0);
```

I want 30 rows, so I can just repeat this process for the rows as well:

```js
const matrix = Array(30)
  .fill()
  .map(() => Array(30).fill(0));
```

This is our matrix.

We initialize it in the Canvas component, and use it to initialize the `matrix` state property using the hooks `useState()` function:

```jsx
import React, { Component, useState } from "react";
//...
const Canvas = () => {
  const [matrix, setMatrix] = useState(
    Array(30)
      .fill()
      .map(() => Array(30).fill(0))
  );
};
```

We are using what we previously called `const matrix` as the initial value of our matrix.

`setMatrix` is the function that we’ll use to update the matrix later on.

Now we can render this matrix with the render() method. I use map() on the `matrix` array, to iterate every row, and for each row I do the same for every column, and return a Pixel component for every single element in the matrix:

```jsx
import React, { Component, useState } from "react";
import Colors from "../Colors";
import Pixel from "./Pixel";

const Canvas = () => {
  const [matrix, setMatrix] = useState(
    Array(30)
      .fill()
      .map(() => Array(30).fill(0))
  );

  return (
    <div className={"canvas"}>
      {matrix.map((row, rowIndex) =>
        row.map((_, colIndex) => {
          return <Pixel key={`${rowIndex}-${colIndex}`} background={Colors[matrix[rowIndex][colIndex]]} />;
        })
      )}
    </div>
  );
};

export default Canvas;
```

as usual when outputting components in a loop, we add a `key` prop (Why? Because React wants it for performance purposes - to render things faster inside our list).

The `background` prop makes sure we add the color name to the Pixel. Since the matrix stores the color index (an integer from 0 to 3 in this case), we import the `Colors` array and get the name of the color from it.

Tip: we're not using the value returned by the map() function, the second time I call it, so I assign it the value `_`, to mean I don't care about it. It's just a convention.

Now let's go back to `src/App.js` file, and instead of including and then adding the Pixel component to the output, we do the same with the Canvas compnent.

```jsx
//...
import Canvas from "./components/Canvas";

function App() {
  return (
    <div className="App">
      <Canvas />
    </div>
  );
}
//...
```

The last thing we need is to add some CSS to the `src/styles.css` file to style our `canvas` and `App` classes:

```css
.App {
  background-color: #333;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.canvas {
  display: flex;
  flex-wrap: wrap;
  max-width: 900px;
}
```

Cool! We can now see the canvas:

![Canvas](https://imgur.com/qst2r4F.png)

---

## WRAPPING UP CANVAS

So we went from displaying a single Pixel to composing multiple Pixel components into a bigger Canvas component. So far, so good!

Next, we’ll add a property to the Pixel component to make it have a color!

---

## THE COLORPICKER COMPONENT

Now we’ll work on a ColorPicker component, which will allow us to choose between 4 different colors, and as such will be composed by 4 pixels.

Let’s create a new file in `src/components` named `ColorPicker.js`.

This will host our ColorPicker component.

The ColorPicker component as said in the introduction will simply host 4 instances of the Pixel component.

Everyone will have a different color. In the next lesson we’ll make it do something useful, but for now, we just need to display it correctly and add it to our app in the correct position.

So we start from zero:

```jsx
import React from "react";
import Pixel from "./Pixel";

export default (props) => {
  return <div className="colorpicker"></div>;
};
```

and we add 4 Pixel components by iterating over the Colors array, which is defined in the `src/Colors.js` file:

```jsx
//...
import Colors from "../Colors";

export default (props) => {
  return (
    <div className="colorpicker">
      {Colors.map((color, index) => {
        return <Pixel key={index} background={color} />;
      })}
    </div>
  );
};
```

We pass a `background` prop with a different color for each pixel, so we have 4 pixels, all colored differently. We can add new colors simply by adding a new entry in the `src/Colors.js` file.

Now we can import the ColorPicker component in the `src/App.js` file and render it in the template:

```jsx
import React from "react";

import "./styles.css";
import Canvas from "./components/Canvas";
import ColorPicker from "./components/ColorPicker";

export default function App() {
  return (
    <div className="App">
      <ColorPicker />
      <Canvas />
    </div>
  );
}
```

Here it is in the browser preview:

![ColorPicker](https://imgur.com/26nKkRn.png)

---

## COLORPICKER RECAP

We wrapped 4 Pixel components in a ColorPicker component, assigning to each one of them a different color.

Next, we’ll change a property in the App component state when one of those pixels is clicked, to set the currently selected color.

---

## LET THE USER CHOOSE A COLOR

Now we put the ColorPicker component to good use. When the user clicks the button we’ll change the default color of the application. This will be in turn used when we’ll allow to color pixels by clicking with the mouse.

The user must be able to click one of the colors in the color picker, and select it to draw something.

First, let’s make the default color highlighted when the app starts.

In `src/App.js` we store the current color in the color property of the state:

```jsx
import React, { useState } from "react";

//... in App()
const [color, setColor] = useState(0);
```

Let’s pass it down to the `ColorPicker` component:

```jsx
<ColorPicker currentColor={color} />
```

Now inside the `ColorPicker` component, we use this prop to identify the current color:

```jsx
<Pixel key={index} background={color} current={Colors[props.currentColor] === color} />
```

Every `pixel` now will get a `current` prop that’s either `true` or `false`.

If it’s true, we’re going to add a `current-color` class to the Pixel component, in `src/components/Pixel.js`:

```jsx
export default (props) => {
  return <div className={`${props.background} pixel ${props.current ? "current-color" : ""}`} />;
};
```

which we style in `src/styles.css` to make it highlighted, have a bigger border:

```css
.pixel.current-color {
  border: 4px solid yellow;
}
```

When the Pixel component in the ColorPicker is clicked, we must update the currently selected color. We start by adding an `onClick` property to the Pixel JSX:

```jsx
export default (props) => {
  return (
    <div className={`${props.background} pixel ${props.current ? "current-color" : ""}`} onClick={props.onClick} />
  );
};
```

which calls a function that’s passed down by the parent component, `ColorPicker`:

```jsx
<Pixel
  onClick={(e) => props.setColor(index)}
  key={index}
  background={color}
  current={Colors[props.currentColor] === color}
/>
```

See, we pass an arrow function to `onClick` which calls the `props.setColor()` function, which is passed by the `App` component.

In App.js we add `setColor()` and we pass it to `ColorPicker` as a prop:

```jsx
function App() {
  const [color, setColor] = useState(0);

  return (
    <div className="App">
      <ColorPicker currentColor={color} setColor={(color) => setColor(color)} />
      <Canvas />
    </div>
  );
}
```

We can now choose another color from the list, and that will be the new default!

---

## WRAPPING UP DEFAULT COLOR

This was probably the most complex part of the project, we first added a centralized place to store the current color in the App component.

Then you set up an event system to handle changing the color when a Pixel in the ColorPicker is clicked.

Let's start coloring next!

---

## COLOR A PIXEL WHEN CLICKED

So far, we have started with a simple Pixel component, we grouped up 900 Pixel components in a Canvas component, and we used 4 Pixel components in a ColorPicker component.

We made it possible to color Pixels, and to have a set of colors to choose from. We also set a default color.

Now let’s add the last bit of functionality: we want to color pixels in the canvas, with the color selected in the ColorPicker.

---

When I click a pixel, that’s going to transform its background color to the currently active color in the color picker.

The Pixel component already has an `onClick` handler, because we added it when we wanted to change the default color in the ColorPicker. Luckily this is just calling the parent prop, so we can pass our handler in the Canvas component:

```jsx
//...
const Canvas = (props) => {
  //...

  const changeColor = (rowIndex, colIndex) => {
    //...
  };

  return (
    <div className={"canvas"}>
      {matrix.map((row, rowIndex) =>
        row.map((_, colIndex) => {
          return (
            <Pixel
              key={`${rowIndex}-${colIndex}`}
              background={Colors[matrix[rowIndex][colIndex]]}
              onClick={(e) => changeColor(rowIndex, colIndex)}
            />
          );
        })
      )}
    </div>
  );
};
```

The `changeColor()` method is what actually changes the color of a pixel. It’s in the Canvas component because that’s where the state of the whole set of pixels (the matrix variable) is kept.

Let's implement it.

We clone the matrix nested array, by using this kind of strange way:
`JSON.parse(JSON.stringify(my_array))` so that we don't go and change the previous array, but we create a new one. Why we do so? For immutability reasons. We can't change the matrix implicitly by editing its current value, but we want it to only change through our `setMatrix()` call.

After cloning the matrix we then update the specific location given by `rowIndex` and `colIndex` and we set it to the new color, and after this we set the new state:

```jsx
const changeColor = (rowIndex, colIndex) => {
  const newMatrix = JSON.parse(JSON.stringify(matrix));
  newMatrix[rowIndex][colIndex] = props.currentColor;
  setMatrix(newMatrix);
};
```

`currentColor` is passed by the App component when it includes Canvas, like it already does for ColorPicker:

```jsx
function App() {
  const [color, setColor] = useState(0);

  return (
    <div className="App">
      <ColorPicker currentColor={color} setColor={(color) => setColor(color)} />
      <Canvas currentColor={color} />
    </div>
  );
}
```

We are done!

Here is the result: https://codesandbox.io/s/lively-wood-3klqm?file=/src/App.js

---

## WHAT WE LEARNED

In this module we wrote a nice little application with React, that lets you draw pixel art with the mouse.

We started from a problem, we identified the units called components we needed, we combined them, and we explored a few of the possibilities that React gives us to build very cool applications.

---
