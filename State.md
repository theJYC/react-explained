# State

*tl;dr: State is mutable data that is managed within a component. State can be mutated by the `setState()` method, which can either be passed in an object parameter or a functional parameter whereby the latter can make reference to the previous version of state.*

State is simply data that a component maintains. 

This is to say, a component can mutate (change the value of) this piece of data. 

This type of mutability is what distinguishes state from props, since the value of props **cannot** be changed by its receiving component; props are immutable by definition.  

In order to use state, a component first needs to be a class component \[0\]. 

Within this class component, you initialise `state` as an object in which you store variables as its property (e.g. `count : 0`):

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

> State is akin to a set of clothes that you wear. 
> When it comes time to change your clothing, you don't modify your existing set of clothing (e.g. paint your shirt to a new colour); 
>
> you completely replace your old clothes with your new clothes. 

This is similar to what you want to do with regards to state. You should stay away from directly modifying state, like:

`this.state.count++`
or
`this.state.count = 1`

Instead, you are going to use a method called `setState()` anytime you want to change state.

And, in a typical scenario, you will be writing this method within a function (e.g. `incrementCount`) that would be invoked by an event listener (e.g. `onClick`):

```jsx 
class App extends React.Component {

    state = {
        count : 0
    }

    //arrow notation to bypass the necessity of binding
    incrementCount = () => {
        this.setState()
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

    incrementCount = () => {
        this.setState({
            count : 1
        })
    }
...
```

What this would do is that, upon `incrementCount()` being invoked by the event handler, state object will be replaced from being `{ count : 0 }` to `{ count : 1 }`. 

After the first time `incrementCount()` is called, though, the value of count will remain at 1 after however many times `incrementCount()` is called subsequently.

This is because `setState` is passing in an object (i.e. a fixed value), and hence it would be that every time `setState` is called, the state object *is* being updated, but *updated as the same object as the previously updated state object*.

Consequently, a more practical parameter to pass into the `setState` method is a function. 

This function will typically be an anonymous function, which is going to receive the previous version of state (e.g. prevState) as its parameter.

The benefit of the functional approach is that, by passing in prevState as the parameter, *you can retain and make reference to the previous version of state*. 

i.e. this functional parameter can return an object (new version of state) with reference to the prevState:

```jsx
...

    incrementCount = () => {
        this.setState((prevState) => {
            return {
                count: prevState.count + 1
            }
        })
    }
...
```


[W3Schools Doc](https://www.w3schools.com/react/react_state.asp) 


\[0\] *Hooks are a modern way of managing state, which is conducted via functional (rather than class-based) components. Correspondingly, the useState() hook is used as a replacement to the setState() method. More on this on [Hooks.md](https://github.com/jinyoungch0i/react-notes/blob/master/Hooks.md)*

\[1\] Whenever the value of state is changed, React will automatically apply the changed value of state to all the instances where you reference the state. This means that child components that work with that state via props will be given the updated value of state. 
