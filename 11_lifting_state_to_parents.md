# Lifting State to Parents

* React components can manage their own state
* Often components need to communicate state to others
* Siblings do not pass state to each other directly
* State should pass through a parent, then trickle down

### Boiling Point Calculator

```jsx
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return (
      <p>The water would boil.</p>;
    )
  }
  return (
    <p>The water would not boil.</p>
  )
}
```

# Lifting State - Calculator Input

* An element to collect input is needed
* The input is passed to the `BoilingVerdict` Component

```jsx
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    return (
      <fieldset>
        <legend>Enter temperature in Celsius:</legend>
        <input
          value={temperature}
          onChange={this.handleChange} />

        <BoilingVerdict
          celsius={parseFloat(temperature)} />

      </fieldset>
    );
  }
}
```

[CodePen](https://codepen.io/Dangeranger/pen/jpJKGN?editors=0010)

# Lifting State - Another Input

* The calculator should work for Celsius & Fahrenheit

### What we want

```jsx
class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
}
```

### Extract TemperatureInput

```jsx
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

# Lifting State - Passing State

* The components must be kept in sync
* Each TemperatureInput holds it's own state
* State can be moved to `Calculator` to achieve this

### Remove State from TemperatureInput

```jsx
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  /* Send changes to the parent */
  handleChange(e) {
    this.props.onTemperatureChange(e.target.value);
  }

  /* Use READ-ONLY Props instead of State */
  render() {
    const temperature = this.props.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

# Lifting State - Parent State

* Children call `onTemperatureChange` with new state
* Parent updates state with `setState`
* Children re-render

### Parent Calculator Passes State to Children

```jsx
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />

        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />

        <BoilingVerdict
          celsius={parseFloat(celsius)} />

      </div>
    );
  }
}
```

[CodePen](https://codepen.io/Dangeranger/pen/djrKLj?editors=0010)

# Lifting State - Live Example

<iframe height="265" style={{width: "100%"}} scrolling="no" title="Lifting State to Parent Components" src="https://codepen.io/burlingtoncodeacademy/embed/oNNRrKx?height=265&theme-id=default&default-tab=js,result" frameBorder="no" allowtransparency="true" allowFullScreen={true}>
  See the Pen <a href='https://codepen.io/burlingtoncodeacademy/pen/oNNRrKx'>Lifting State to Parent Components</a> by Joshua Burke
  (<a href='https://codepen.io/burlingtoncodeacademy'>@burlingtoncodeacademy</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

# Lifting State - Conclusions

* A single "point of truth" holds the state
* Components communicate state by lifting it up to a Parent
* Child components use a Parent updater function to lift state up
* State flowing down makes state changes simpler to debug
* Props should be derived from State

### Links

- [Top Down Data Flow](https://reactjs.org/docs/state-and-lifecycle.html#the-data-flows-down)
