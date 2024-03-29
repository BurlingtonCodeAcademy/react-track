# Fetching Data

* Most React frontend apps need server data
* The browsers `window.fetch` API can be used to make requests
* The `componentDidMount()` is recommended as a best practice

### Basic Component

```jsx
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      items: []
    };
  }

  /* This is where data will be fetched from */
  componentDidMount() {}

  render() {
    const { items } = this.state;
    return (
      <ul>
        {items.map(item => (
          <li key={item.id}>
            <h3>{item.title}</h3>
            <p>{item.body}</p>
          </li>
        ))}
      </ul>
    );
  }
}
```

# Fetching Data - componentDidMount

* Render an empty component first
* Get data after it has been rendered
* Re-render with data from `setState({some: 'state'})`

### Component with Data

```jsx
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      items: [],
      isLoaded: false,
    };
  }

  componentDidMount() {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(res => res.json())
      .then(result => {
        this.setState({
          isLoaded: true,
          items: result
        });
      });
  }

  render() {
    const { items } = this.state;
    if (!isLoaded) {
      return <div>Loading ... </div>;
    } else {
      return (
        <ul>
          {items.map(item => (
            <li key={item.id}>
              <h3>{item.title}</h3>
              <p>{item.body}</p>
            </li>
          ))}
        </ul>
      );
    }
  }
}
```

# Fetching Data - Handle Errors

* Errors in `fetch` using APIs happen
* Do not let an error ruin your page by raising in production
* Better to wait/retry and present a nice message

### Component with Error Handling

```jsx
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      error: null,
      isLoaded: false,
      items: []
    };
  }

  componentDidMount() {
    fetch("https://jsonplaceholder.typicode.com/posts")
      .then(res => res.json())
      .then(
        result => {
          this.setState({
            isLoaded: true,
            items: result
          });
        },
        error => {
          this.setState({
            isLoaded: true,
            error: error
          });
        }
      );
  }

  render() {
    const { error, isLoaded, items } = this.state;
    if (error) {
      return <div>Error: {error.message}</div>;
    } else if (!isLoaded) {
      return <div>Loading...</div>;
    } else {
      console.log(this.state.items);
      return (
        <ul>
          {items.map(item => (
            <li key={item.id}>
              <h3>{item.title}</h3>
              <p>{item.body}</p>
            </li>
          ))}
        </ul>
      );
    }
  }
}
```

# Fetching Data Example

<iframe height="265" style={{width: "100%"}} scrolling="no" title="Fetching API Data" src="https://codepen.io/burlingtoncodeacademy/embed/PoovMwO?height=265&theme-id=default&default-tab=js,result" frameBorder="no" allowtransparency="true" allowFullScreen="true">
  See the Pen <a href='https://codepen.io/burlingtoncodeacademy/pen/PoovMwO'>Fetching API Data</a> by Joshua Burke
  (<a href='https://codepen.io/burlingtoncodeacademy'>@burlingtoncodeacademy</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
