# useSpring

```js
import {useSpring, animated} from 'react-spring'
```

Turns values into animated-values.

### Either: overwrite values to change the animation

If you re-render the component with changed props, the animation will update.

```jsx
const props = useSpring({opacity: toggle ? 1 : 0})
```

### Or: pass a function that returns values, and update using "set"

You will get an updater function back. It will not cause the component to render like an overwrite would (still the animation executes of course). Handling updates like this is useful for fast-occurring updates, but you should generally prefer it. Optionally there's also a stop function as a third argument.

```jsx
const [props, set, stop] = useSpring(() => ({opacity: 1}))

// Update spring with new props
set({opacity: toggle ? 1 : 0})
// Stop animation
stop()
```

### Finally: distribute animated props among the view

The return value is an object containing animated props.

```jsx
return <animated.div style={props}>i will fade</animated.div>
```

## Properties

All properties of the [shared-api](api) apply.

## Additional notes

### To-prop shortcut

Any property that useSpring does not recognize will be combined into "to", for instance `opacity: 1` will become `to: { opacity: 1 }`.

```jsx
// This ...
const props = useSpring({opacity: 1, color: 'red'})
// is a shortcut for this ...
const props = useSpring({to: {opacity: 1, color: 'red'}})
```

### Async chains/scripts

The `to` prop also allows you to (1) script your animation, or (2) chain multiple animations together. Since these animations will execute asynchronously, make sure to provide a `from` property for base values (otherwise, props will be empty).

#### This is how you create a script

```jsx
const props = useSpring({
  to: async (next, cancel) => {
    await next({opacity: 1, color: '#ffaaee'})
    await next({opacity: 0, color: 'rgb(14,26,19)'})
  },
  from: {opacity: 0, color: 'red'}
})
// ...
return <animated.div style={props}>I will fade in and out</animated.div>
```

#### And this is how you create a chain

```jsx
const props = useSpring({
  to: [{opacity: 1, color: '#ffaaee'}, {opacity: 0, color: 'rgb(14,26,19)'}],
  from: {opacity: 0, color: 'red'}
})
// ...
return <animated.div style={props}>I will fade in and out</animated.div>
```

## Demos
