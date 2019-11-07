# What is JSX?

- Mixes HTML and JavaScript together
- Makes building the UI easier
- Is transformed into ReactDom code
- Is used everyday in React

# Starting JSX

- This is JSX Code

```jsx
ReactDOM.render(
  <h1>Hello, React!</h1>,
  document.getElementById('root')
);
```

# Resulting ReactDOM code

- This is the ReactDOM code the JSX creates

```jsx
ReactDOM.render(React.createElement(
  'h1',
  null,
  'Hello, React!'
), document.getElementById('root'));
```

# Resulting HTML in the DOM

```html
<div id="root">
  <h1>Hello, React!</h1>
</div>
```

# What we learned

- JSX feels like HTML (inside of JavaScript)
- JSX compiles to ReactDOM code
- JSX is a 'syntax extension' to JavaScript
- ReactDOM builds the HTML from the virtual DOM

# JSX can embed JavaScript expressions

Building a greeter

```jsx
const user = {
  firstName: 'Ada',
  lastName:  'Lovelace',
  nickName:  'The queen of numbers'
};

function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const element = (
  <div>
    <h1>Welcome to React, {formatName(user)}!</h1>
    <h2>{user.nickName}</h2>  
  </div>
  );

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

# After compiling JSX -> JS

```js
const user = {
  firstName: 'Ada',
  lastName:  'Lovelace',
  nickName:  'The queen of numbers'
};

function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const element = (
  <div>
    <h1>Welcome to React, {formatName(user)}!</h1>
    <h2>{user.nickName}</h2>  
  </div>
  );

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

# JSX is transformed into ReactDOM

- Tools like [Babel](https://babeljs.io) convert JSX to ReactDOM
- You can use Babel as a script in your project HTML for development

```jsx
<script src="https://unpkg.com/babel-standalone@6/babel.min.js">
<script type="text/babel">
console.log( <h1>yo</h1> );
</script>
```


## Example

```html
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    <script src="https://unpkg.com/react/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom/umd/react-dom.development.js"></script>
  </head>
  <body>
    <div id="output"></div>
    
    <!-- Load Babel and React -->
    <!-- Your script here -->
    <script type="text/babel">
      ReactDOM.render(
       <h1>Hello, again React!</h1>,
       document.getElementById('output')
      );
    </script>
  </body>
</html>
```
