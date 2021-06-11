# Default Props

*tl;dr : When you are **expecting** a parent component to return a child component with certain props, specifying default props for the child component enables you to automatically pass in the props in the case that those props are not passed in at the parent level.*

Say you have a parent component `List.js`, which returns 3 children elements `ListItem.js`:

```jsx
import React from "react"

function List() {
    return (
        <div>
            <ListItem info="groceries" checked="false" />
            <ListItem info="workout" checked="false" />
            <ListItem info="journal" checked="false" />
        </div>   
    )
}
```

Since you are manually passing in the props here, when you need to add an extra `<ListItem info="walk"/>` to be returned, you may forget to pass in its respective `checked` prop:

```jsx
function List() {
    return (
        <div>
            <ListItem info="groceries" checked="false" />
            <ListItem info="workout" checked="false" />
            <ListItem info="journal" checked="false" />
            <ListItem info="walk" />
        </div>   
    )
}
```

Depending on the devised logic of the `<ListItem />` child component, this mistaken omit of the checked prop may break the application if the `checked` prop determines the way in which the `<ListItem />` component is rendered. 

One way to prevent this breaking from happening is to define a set of default props to the functional component in question (`<ListItem />`):

```jsx
function ListItem(props) {
    return (
        <div>
            <label>{props.info}
            <input type="checkbox" checked={props.checked}>{props.info}</p>
        </div>
    )
}

//specifiying a static property defaultProps on the ListItem functional component:

ListItem.defaultProps = {
    
}
```

And within the ListItem.defaultProps object, you can specify any value to the corresponding props:

```jsx
ListItem.defaultProps = {
    checked : false
}
```

That way, if you forget to specify a `checked` prop in the parent component, the defaultProps object will provide a fall back value (in this case `checked="false"`). 

Conversely, if you do pass in the `checked` prop in the parent component and it happens to be different to what has been defined in defaultProps (i.e. `checked="true"`), the prop that you manually passed in will **override the default value of that prop**. 

N.B. You can add as many default props to the defaultProps object (as opposed to just one that is shown in the above example).

[Official Doc](https://reactjs.org/docs/typechecking-with-proptypes.html#default-prop-values)
