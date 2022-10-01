# Reactive CounterIncrement in VanillaJS
*This is why I'm still struggling to get my head around reactive frameworks like React.js*

In ---, 2020, just after I'd learned how to build native `WebComponents` I came across...

Similarly, this morning, I've just come across this article:

 - [Making `setInterval` Declarative with React Hooks](https://overreacted.io/making-setinterval-declarative-with-react-hooks/) by *Dan Abramov*

In the course of the blog post, Abramov writes a **React** hook `useInterval()` which looks very similar to **VanillaJS** `setInterval()`

Anticipating the reader's question:

> So why not just use setInterval directly?

He responds:

> This may not be obvious at first, but the difference between the setInterval you know and my useInterval Hook is that its arguments are “dynamic”.

And then he demonstrates the *dynamic `setInterval`* with two examples:

### The longer example using React classes

```js
class Counter extends React.Component {
  state = {
    count: 0,
    delay: 1000,
  };
  componentDidMount() {
    this.interval = setInterval(this.tick, this.state.delay);
  }
  componentDidUpdate(prevProps, prevState) {
    if (prevState.delay !== this.state.delay) {
      clearInterval(this.interval);
      this.interval = setInterval(this.tick, this.state.delay);
    }
  }
  componentWillUnmount() {
    clearInterval(this.interval);
  }
  tick = () => {
    this.setState({
      count: this.state.count + 1
    });
  }
  handleDelayChange = (e) => {
    this.setState({ delay: Number(e.target.value) });
  }
  render() {
    return (
      <>
        <h1>{this.state.count}</h1>
        <input value={this.state.delay} onChange={this.handleDelayChange} />
      </>
    );
  }
}
```

### The shorter example using React hooks

```js
function Counter() {
  let [count, setCount] = useState(0);
  let [delay, setDelay] = useState(1000);

  useInterval(() => {    // Your custom logic here    setCount(count + 1);  }, delay);
  function handleDelayChange(e) {
    setDelay(Number(e.target.value));
  }

  return (
    <>
      <h1>{count}</h1>
      <input value={delay} onChange={handleDelayChange} />
    </>
  );
}
```

______



## HTML

```html
<h2 class="counterHeading">0</h2>
<input class="intervalInput" type="text" value="600" />
```

______

## JavaScript

```js
const counterHeading = document.querySelector('.counterHeading');
let counterIntervalInput = document.querySelector('.intervalInput');

const getCounterInterval = () => counterIntervalInput.value;
const getCounterValue = () => parseInt(counterHeading.textContent);
const incrementCounterValue = () => counterHeading.textContent = (getCounterValue() + 1);

let incrementCounter = setInterval(() => incrementCounterValue(), getCounterInterval());

const updateCounterInterval = () => {
  clearInterval(incrementCounter);
  incrementCounter = setInterval(() => incrementCounterValue(), getCounterInterval());
}

counterIntervalInput.addEventListener('change', updateCounterInterval);
```
____

## See also:

