# Fragments

In React, the components you create can only return a single parent element. 

This means that you couldn't return two sibling elements; instead, you have to wrap them in a single parent element. In the below example, that parent element is represented by the outer <div> tags in the App component's return:

```jsx
import React from "react"

function App() {
    return(
        <div>
            <p>Some text...</p>
            <p>Some more text...</p>
        </div>
    )
}
```

Now, say that the App component is rendered onto the virtual DOM in `index.js`, as: 

```jsx
import React from "react"
import ReactDOM from "react-dom"

ReactDOM.render(<App />, document.getElementById("root"))
```

This will mean that, upon render, React will insert the App component (and its corresponding JSX) into the "root" div of the virtual DOM tree, as: 

```html
<html>
    <head>
        <link rel="stylesheet" href="style.css">
    </head>
    <body>
        <div id="root">
            <!-- the returned JSX of <App /> -->
            <div>
                <p>Some text...</p>
                <p>Some more text...</p>
            </div>
            <!-- end of returned JSX of <App /> -->
        </div>
    </body>
</html>
```

*Comments written for illustration purposes*

Notice here how the App component renders the JSX that you care about, within the single parent element `<div> </div>`. 

While, in this particular example, the App component does not render any child components, you can start to notice how many redundant `<div>`s can be populating the DOM tree once you start to have multiple children / grandchildren components, given how you'd have to wrap each component within the `<div>`s to comply with React's strict 'only return a single parent' rule.

In order to prevent the redundant rendering of `<div>`s, what you could do is use `React.Fragment`:

```jsx
import React from "react"

function App() {
    return (
        <React.Fragment>
            <p>Some text...</p>
            <p>Some more text...</p>
        </React.Fragment>
    )
}
```

In doing so, the App component will be inserted into the virtual DOM tree *without* the extra parent `<div>` tags that were present in the before example:

```html
<html>
    <head>
        <link rel="stylesheet" href="style.css">
    </head>
    <body>
        <div id="root">
            <!-- the returned JSX of <App /> -->
                <p>Some text...</p>
                <p>Some more text...</p>
            <!-- end of returned JSX of <App /> -->
        </div>
    </body>
</html>
```

If using React.Fragment, you could clean up the code by performing a named import on top of the App.js file:

```jsx
import React, { Fragment } from "react"
```

and just call `Fragment` in the JSX as:
```jsx
function App() {
    return (
        <Fragment>
        <p>Some text...</p>
        <p>Some more text...</p>
        </Fragment>
    )
}
```

With updates to React, you can now use a short and simple syntax that will perform the same thing it would with `<React.Fragment>` or `<Fragment>`. You just have to write empty tags (`<>`):

```jsx
function App() {
    return (
        <>
        <p>Some text...</p>
        <p>Some more text...</p>
        </>
    )
}
```

With the benefits of Fragments being acknowledged, you should be cautious of going ahead and changing every `<div>` in your components to fragments. 

This is because **fragmenting will change the parent/child relationship of your components to sibling relationships instead**, which may break your CSS if your components are reliant on things like CSS flexbox/grid that operate on explicit parent/child relationships. 

tl;dr : Understanding fragmenting is useful, and may even be helpful when you want to clean up your virtual DOM tree. You should, however, only use fragmenting when your components are not involved in CSS styles (e.g. flexbox/grid) where the strict parent/child relationships need to be maintained. 

[Official Doc](https://reactjs.org/docs/fragments.html)  
