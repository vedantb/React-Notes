## PIXEL DRAWING APP

You’ve probably seen a similar application already. I have a few of them on my iPad. It’s a cool way to relax, by drawing something cool.

This is what we aim for:

![Pixel Drawing App](https://thereactcourse.com/giaKcDqEO00cTaLABN3F/pixel-art/1-intro/end-result.png)

### WHAW WE'LL BUILD IN DETAIL

The goal of this tutorial is to build a canvas, a 30x30 “pixels” matrix. When the app starts, there is a blank canvas showing up.

There is also a color picker, which shows the colors you can use. A default color is pre-selected when the app starts.

When you click one pixel in the canvas, that pixel will be colored with that default color.


### THE MAIN COMPONENTS OF THE APP

Given the UI that is showed above, we basically need 3 components.

1. One is the **Pixel**, a single unit that will compose our drawing.
2. One is the **Canvas**, which is a set of 30 rows, each containing 30 Pixel components.
3. The last one is the **ColorPicker**, which contains one or more Pixels components.

Clicking a Pixel component inside a ColorPicker will change the active color.

Clicking a Pixel component inside a Canvas will change the background color of the pixel.


### BOOTSTRAP A REACT APP USING CREATE-REACT-APP

We can now start creating our app. Let's start up a project using the create-react-app or a code sandbox react project.


### THE FIRST COMPONENT, PIXEL

Create a `components` folder in the `src` folder, and inside it create an empty `Pixel.js` file.

We’re going to import React, then we create a function component that simply outputs a `div` with two classes: `pixel` and the `background` property passed as a prop:

```jsx
import React from 'react'

export default props => {
  return <div className={`${props.background} pixel`} />
}
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
export default ['white', 'lightblue', 'blue', 'darkblue'];
```

The first color is the base color, and defines the color used by all cells when the app starts. The others are 3 choices I give to color the pixels.

Now let’s just add this one pixel in the app. Open `src/App.js` and after all the imports add

```js
import Pixel from './components/Pixel'
```

and inside the `App()` function, make the component return one Pixel:

```jsx
return (
  <div className="App">
    <Pixel />
  </div>
)
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

