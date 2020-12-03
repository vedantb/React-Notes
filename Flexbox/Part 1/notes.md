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