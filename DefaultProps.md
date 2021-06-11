# Default Props

*tl;dr : When you are **expecting** a parent component to return a child component with certain props, specifying default props for the child component enables you to automatically pass in the props in the case that those props are not passed in at the parent level.

Say you have a parent component `List.js`, which returns 3 children elements `TodoItem.js`:

```jsx
import React from "react"

function List() {
    return (
        <div>
            <TodoItem info="groceries" checked="false" />
            <TodoItem info="workout" checked="false" />
            <TodoItem info="journal" checked="false" />
        </div>   
    )
}
```

Since you are manually passing in the props here, when you need to add an extra `<TodoItem info="walk"/>` to be returned, you may forget to pass in its respective `checked` prop:

```jsx
function List() {
    return (
        <div>
            <TodoItem info="groceries" checked="false" />
            <TodoItem info="workout" checked="false" />
            <TodoItem info="journal" checked="false" />
            <TodoItem info="walk" />
        </div>   
    )
}
```

Depending on the devised logic of the `<TodoItem />` child component, this mistaken omit of the checked prop may break the application if the `checked` prop determines the way in which the `<TodoItem />` component is rendered. 

One way to prevent this breaking from happening is to define a set of default props to the functional component in question (<TodoItem />):

```jsx
function TodoItem(props) {
    return (
        <div>
            <label>{props.info}
            <input type="checkbox" checked={props.checked}>{props.info}</p>
        </div>
    )
}

//specifiying a static property defaultProps on the TodoItem functional component:

TodoItem.defaultProps = {
    
}
```

And within the TodoItem.defaultProps object, you can specify any value to the corresponding props:

```jsx
TodoItem.defaultProps = {
    checked : false
}
```

That way, if you forget to specify a `checked` prop in the parent component, the defaultProps object will provide a fall back value (in this case `checked="false"`). 

Conversely, if you do pass in the `checked` prop in the parent component and it happens to be different to what has been defined in defaultProps (i.e. `checked="true"`), the prop that you manually passed in will **override the default value of that prop**. 

N.B. You can add as many default props to the defaultProps object (as opposed to just one that is shown in the above example).

