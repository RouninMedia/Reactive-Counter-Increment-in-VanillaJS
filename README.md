# Reactive Counter Increment in VanillaJS
*This is why I'm still struggling to get my head around reactive frameworks like React.js*

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

const updateCounterInterval = () => {
  clearInterval(incrementCounter);
  incrementCounter = setInterval(() => incrementCounterValue(), getCounterInterval());
}

let incrementCounter = setInterval(() => incrementCounterValue(), getCounterInterval());

counterIntervalInput.addEventListener('change', updateCounterInterval);
```
