We first add

```css
.container {
    display: flex;
    border: 10px solid goldenrod;
    height: 100vh;
}
```

You see that the flex contianer goes across the entire page (similar to block level elements)
We also have:

```css
.container {
    display: inline-flex;
    border: 10px solid goldenrod;
}
```

Just like an inline element and will only wrap around the content.


By defining this container as `display: flex`, what happens is that the divs inside the container automatically become flex items. The relationship between the immediate children of flex container makes these divs flex items. 


## Working with Flexbox Flex Direction

If we add a flex direction to container:

```css
.container {
    display: flex;
    border: 10px solid goldenrod;
    min-height: 100vh;
    flex-direction: row;
}
```

Nothing changes, because that is the default of any flex container. This means that the flex items will stack right beside each other and they will extend vertically to hit the height of whatever the container is. In this case, we have `min-height: 100vh` so it takes up the page else it would just default to the amount of content.

The other direction is `column`:

```css
.container {
    display: flex;
    border: 10px solid goldenrod;
    min-height: 100vh;
    flex-direction: column;
}
```

You see that instead of putting them side by side, flex items will stack vertically on top of each other. 

The main thing we want to take away from this is that when we are defining things as `flex-direction: row` or `flex-direction: column`, we have 2 axes: 
- the main axis: when the direction is row, this is left to right, when its column its top to bottom
- cross axis: when direction is row, cross axis is top to bottom, when its column its left to right

We also have `row-reverse`. This has main axis from right to left
We also have `column-reverse`. This has the main axis from bottom to top


## WRAPPING ELEMENTS WITH FLEXBOX

What happens when there's too much space and the elements go wider than they should. 
Take your existing knowledge of floats and throw it out of the window.

Now, lets give each box a width of 300px
```cs
.box {
    width: 300px;
}
```

The nature of flexbox is that it will try to work with the widths you give it. But, if it doesn't work out it's going to evenly distribute it through the children. Or we'll soon learn about the `flex` property where we can assign more or less to a particular flex item.

That's where `flex-wrap` comes in. This is a property for the flex container, not the item. 
The default is `flex-wrap: nowrap;`

We can change it to `flex-wrap: wrap`. Woah!
It works with 300px now. It tries to fit as many as it can and then breaks to the next line.
Before, this they were as high as the container. Now, they'll still try to stretch all across the height of the container.

Our main axis here is left to right. Cross axis is top to bottom.

If we change it to `wrap-reverse`, the cross axis is now bottom to top. So the items are still starting at the left hand side, but they go from bottom and go on up.

