# State

*tl;dr: *

State is simply data that a component maintains. 

This is to say, a component can mutate (change the value of) this piece of data. 

This type of mutability is what distinguishes state from props, since the value of props **cannot** be changed by its receiving component; props are immutable by definition.  

In order to use state, a component first needs to be a class component \[0\]. 

Within this class component, you initialise `state` as an object in which you store variables as its property (e.g. count = 0):

```jsx
import React from "react"

class App extends React.Component {
    //state is always an object
    state = {
        count : 0
    }

    render() {
        return (
        <>
            <h1>Current count: </h1>
        </>
        )
    }
}
```

And to reference this state property, you will have to use `this.state.whateverProperty`. 

In this example, that would be `this.state.count`:

```jsx 
class App extends React.Component {

    state = {
        count : 0
    }

    render() {
        return (
        <>
            <h1>Current count: {this.state.count}</h1>
        </>
        )
    }
}
```

State can be passed down to child components (e.g. `<Child />`) via props, following the same logic \[1\]: 

```jsx 
class App extends React.Component {

    state = {
        count : 0
    }

    render() {
        return (
        <>
            <h1>Current count: {this.state.count}</h1>
            <Child count={this.state.count} />
        </>
        )
    }
}
```

Now, a brief analogy to preface the modification of state: 

> State is akin to a set of clothes that you wear. When it comes time to change > your clothing, you don't modify your existing set of clothing (e.g. paint     > your t shirt to a new colour); 
>
> you completely replace your old clothes with your new clothes. 

This is similar to what you want to do with regards to state. You should stay away from directly modifying state, like:

`this.state.count++`
or
`this.state.count = 1`

Instead, you are going to use a method called `setState()` anytime you want to change state.

And, in a typical scenario, you will be writing this within a function (e.g. `incrementCount`)that would be invoked by an event listener (e.g. `onClick`):

```jsx 
class App extends React.Component {

    state = {
        count : 0
    }

    incrementCount() {
        setState()
    }

    render() {
        return (
        <>
            <h1>Current count: {this.state.count}</h1>
            <button onClick={incrementCount}>increment</button>
        </>
        )
    }
}
```

Now, the setState method can take in two options as its parameter. 

The first option would be to pass in an object:

```jsx
...

    incrementCount() {
        setState({
            count : 1
        })
    }
...
```

What this would do is that, upon incrementCount() being invoked by the event handler, state object will be replaced from being `{ count : 0 }` to `{ count : 1 }`. 

After the first time incrementCount() is called, though, the value of count will remain at 1 after however many times incrementCount() is called subsequently.

This is because `setState` is passing in an object (i.e. a fixed value), and hence it would be that every time `setState` is called, the state object *is* b eing updated, but *updated as the same object as the previously updated state object*.

Consequently, a more practical parameter to pass into the `setState` method is a function. This function will typically be an anonymous function, which is going to receive the previous version of state (e.g. prevState) as its parameter:

```jsx
...

    incrementCount() {
        setState((prevState) => {

        })
    }
...
```

The benefit of the functional approach is that you can retain the previous version of state (via the `prevState` parameter). 

Now, this function can return an object with reference to the prevState:

```jsx
...

    incrementCount() {
        setState((prevState) => {
            return {
                count: prevState.count + 1
            }
        })
    }
...
```


\[0\] *Hooks enable you to use state via functional (vs. class-based) components. More on this on Hooks.md*

\[1\] Whenever the value of state is changed, React will automatically apply the changed value of state to all the instances where you reference the state. This means that child components that work with that state via props will be given the updated value of state. 
