# Context

*tl;dr: Context API allows you to pass data through a component tree via a Provider-Consumer pair, preventing the need to drill props down manually at every individual component level.*

In React, the only way to pass data among components is **downward**. And the only way to pass data downward is through **props**. 

Therefore, if you have two sibling components that need to share a piece of state, that state needs to be *lifted* to a higher component (if lucky, only to the parent component immediately above). Then, have that parent component pass the state downward to those sibling components as props. 

The problem with this method, however, is **when such data needs to be shared by components that are in vastly different places along the component hierarchy**. 

In those cases, state will need to be lifted several levels up the hierarchy just so that it can be passed down as props; those props will then be drilled all the way down until they reach the child components that need the data. 

Now, prop drilling is not inherently bad if every component that receives the props through this process has some use for it. Conversely, **prop drilling is less than ideal when the intermediary components merely serve the function of passing props down without making use of them**. \[0\]
 
Recognising this problem, React introduced its Context API. 

When you have several components that need to share state, the common parent is identified and is wrapped inside a **Provider**. Then, all of the components that need the data is wrapped inside a **Consumer**. 

With the Provider-Consumer pair, React is allowed to create a **Context Tunnel** that leads the data from the Provider directly to what is consuming the data. 

You can pass whatever you want through this tunnel. Often times, you might want to pass down data; other times, some methods that let the Consumer modify that data.  

\[0\]: More on [prop drilling](https://kentcdodds.com/blog/prop-drilling)

[Official Doc](https://reactjs.org/docs/context.html) 
