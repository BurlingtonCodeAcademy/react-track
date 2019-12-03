# React Notes

React is a system for doing client-side components

What's a component? It's a *part* of your page. It's like a div that knows how to draw itself and respond to events, but it's *self-contained*.

How self-contained is it? It's so self-contained that it lives in its *own file*, like a `class` or NPM module. This file is called `JSX` and it *compiles* into `JS`.

Two kinds of data: props and state

  props are like arguments/parameters
  state is like a global, or a closure var


"the context" makes deeply nested components easier, but is otherwise ignorable

```
JSX --------> JS
      Babel
```

`create-react-app` generates a **lot** of code

"golden rule" -- don't query the DOM; use a "ref" instead

--- 

# React Live Example

```jsx live=true
function reactExample() {
  return(
    <h1>Hello from React</h1>
  )
}
```