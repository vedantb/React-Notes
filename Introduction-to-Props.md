### Introduction To Props

Props is how Components get their properties. Starting from the top component, every child component gets its props from the parent. In a function component, props is all it gets passed, and they are available by adding `props` as the function argument:

```jsx
const BlogPostExcerpt = props => {
  return (
    <div>
      <h1>{props.title}</h1>
      <p>{props.description}</p>
    </div>
  )
}
```

In a class component, props are passed by default. There is no need to add anything special, and they are accessible as `this.props` in a Component instance.

```jsx
import React, { Component } from 'react'

class BlogPostExcerpt extends Component {
  render() {
    return (
      <div>
        <h1>{this.props.title}</h1>
        <p>{this.props.description}</p>
      </div>
    )
  }
}
```

Passing props down to child components is a great way to pass values around in your application. A component either holds data (has state) or receives data through its props.

It gets complicated when:
 - you need to access the state of a component from a child that’s several levels down (all the previous children need to act as a pass-through, even if they do not need to know the state, complicating things)
 - you need to access the state of a component from a completely unrelated component.

#### Default values for props

If any value is not required we need to specify a default value for it if it’s missing when the Component is initialized.

```jsx
BlogPostExcerpt.propTypes = {
  title: PropTypes.string,
  description: PropTypes.string
}

BlogPostExcerpt.defaultProps = {
  title: '',
  description: ''
}
```

Some tooling like ESLint have the ability to enforce defining the defaultProps for a Component with some propTypes not explicitly required.

#### How props are passed

When initializing a component, pass the props in a way similar to HTML attributes:

```jsx
const desc = 'A description'
//...
<BlogPostExcerpt title="A blog post" description={desc} />
```

We passed the title as a plain string (something we can only do with strings!), and description as a variable.

#### Children

A special prop is `children`. That contains the value of anything that is passed in the `body` of the component, for example:

```jsx
<BlogPostExcerpt title="A blog post" description="{desc}">
  Something
</BlogPostExcerpt>
```
In this case, inside `BlogPostExcerpt` we could access “Something” by looking up `this.props.children`.

While Props allow a Component to receive properties from its parent, to be “instructed” to print some data for example, state allows a component to take on life itself, and be independent of the surrounding environment.