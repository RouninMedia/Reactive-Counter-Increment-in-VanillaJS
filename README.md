# Reactive CounterIncrement in VanillaJS
*This is why I'm **still** struggling to get my head around reactive frameworks like React.js*

In Oct 2020, just after I'd learned how to build native `WebComponents` I came across a claim that, compared to components in frameworks like **React**, native `WebComponents` required:

> way to much fiddling around to get working

I was sceptical of this claim and I built this [**Data Driven WebComponent**](https://github.com/RouninMedia/Data-Driven-WebComponent) to show that that once learned, `WebComponents` were entirely straightforward to build and have them operate exactly as intended.

____

In a similar exercise, this morning, I've just come across this article:

 - [Making `setInterval` Declarative with React Hooks](https://overreacted.io/making-setinterval-declarative-with-react-hooks/) by *Dan Abramov*

In the course of the blog post, Abramov writes a **React** hook `useInterval()` which looks very similar to **VanillaJS** `setInterval()`

Anticipating the reader's question:

> So why not just use setInterval directly?

He responds:

> This may not be obvious at first, but the difference between the setInterval you know and my useInterval Hook is that its arguments are “dynamic”.

And then he demonstrates the *dynamic `setInterval`* with two examples.

### The longer example using React classes:

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

### The shorter example using React hooks:

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

The second example using React hooks is shorter and arguably clearer and better.

It's around 10-11 lines long and is straightforwardly legible to a human reader who understands the syntax.

______

Initially I imagined it would require a lot more code in **VanillaJS** to reproduce a near-identical *dynamic `setInterval`*.

But it turns out the same can be achieved, *also* in 10 clearly readable lines:

## HTML

```html
<h2 class="counterHeading">0</h2>
<input class="intervalInput" type="text" value="600" />
```

______

## JavaScript

```js
// GET DOCUMENT NODES
const counterHeading = document.querySelector('.counterHeading');
let counterIntervalInput = document.querySelector('.intervalInput');

// DECLARE FUNCTIONS
const getCounterInterval = () => {counterIntervalInput.value;}
const getCounterValue = () => {parseInt(counterHeading.textContent);}
const incrementCounterValue = () => {counterHeading.textContent = (getCounterValue() + 1);}

// DECLARE NAMED INTERVAL
let incrementCounter = setInterval(() => {incrementCounterValue();}, getCounterInterval());

// UPDATE NAMED INTERVAL
const updateCounterInterval = () => {
  clearInterval(incrementCounter);
  incrementCounter = setInterval(() => {incrementCounterValue();}, getCounterInterval());
}

// EVENT LISTENER TO FIRE THE UPDATE FUNCTION
counterIntervalInput.addEventListener('change', updateCounterInterval);
```

The **VanillaJS** code above is (as far as I can tell - and I'm more than happy to be corrected) robust, reusable, and more performant than **React**. It's readable, it's short - it's definitely not mind-numbingly complex. It is (I would argue) easy to reason about.

Significantly it can be written directly into the browser - it doesn't require an external dependency or compilation or toolchains or taskrunners or any infrastructure (plus associated maintenance) at all.

So... again, what is the *point* of reactive frameworks like React?

If anyone has some enlightening words, I am all ears.

