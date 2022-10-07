# awesome-ds

A personal collection of resources, notes, and references for Design Systems

<!-- prettier-ignore-start -->
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of Contents

- [React](#react)
  - [Components](#components)
    - [Stable default values](#stable-default-values)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- prettier-ignore-end -->

## React

### Components

#### Stable prop callbacks

A component may accept callback functions as props, such as an `onChange`, that is invoked in response to an action or event.

```jsx
function ExampleComponent({ onClick }) {
  return <button type="button" onClick={() => onClick()}>Example</button>;
}
```

At times, this handler may need to be referenced in an effect or in other primitive that requires a dependency array. This may result in the following implementation:

```jsx
function ExampleComponent({ onChange }) {
  // ...
  
  useEffect(() => {
    onChange(state);
  }, [state, onChange]);
  
  // ...
}
```

In this situation, the effect will fire each time the `state` value updates but also each time the `onClick` function reference changes. If you're expecting consumers to use values like an arrow function expression, this may lead to unexpected work being performed.

```jsx
<ExampleComponent onChange={() => { /* ... */ }} />
```

There may also be a correctness aspect to this, as well, in that the effect should not be synchronizing with the identity of the `onChange` function and is firing unexpected as a result. For example, a `ResizeObserver` implementation may look like:

```jsx
function ExampleComponent({ onResize }) {
  const ref = useRef();
  
  useEffect(() => {
    const observer = new ResizeObserver((entries) => {
      // ...
      
      onResize();
    });
    
    const { current: element } = ref;
    observer.observe(element);
    
    return () => {
      observer.disconnect();
    };
  }, [onResize]);
  
  // ...
}
```

In this situation, the `useEffect` will run each time the reference of `onResize` changes. As a result, the callback to `ResizeObserver` will also call `onResize` as this is the default behavior of this primitive. Depending on what `onResize` does when called, this may lead to an infinite loop as the state of the parent changes which in turn causes the `onResize` to change, which then updates the `useEffect`, and finally the `ResizeObserver` callback before looping again.

To address this, you can save the value of the callback into a ref:

```jsx
function ExampleComponent({ onChange }) {
  const savedOnChange = useRef(null);
  
  // ...
  
  useEffect(() => {
    savedOnChange.current = onChange;
  });

  useEffect(() => {
    savedOnChange.current?.(state);
  }, [state]);
  
  // ...
}
```

This allows you to always have the latest reference to `onChange` while still having a stable reference in the effect as to avoid adding it to the depenendecy array. 

#### Stable default values

A component may provide a default value for a `prop` as a default parameter.

```jsx
function ExampleComponent({ onChange = () => {}, foo = {}, bar = [] }) {
  // ...
}
```

Historically, default props may also be defined as `defaultProps` on the
component function.

```jsx
function ExampleComponent() {
  // ...
}

ExampleComponent.defaultProps = {
  onChange: () => {},
  foo: {},
  bar: [],
};
```

When moving from `defaultProps` to default parameters, there is a change in the
reference of a value. Specifically, with `defaultProps` values will have the
same reference for complex types like functions, objects, arrays, etc. When used
as a default parameter, however, these values will no longer have a stable
reference as they are re-created each render.

Most of the time, this will not be an issue. However, if you are using these
values in dependency arrays or notice components unnecessarily re-rendering when
provided these values then you can move these values to the top-level scope to
retain the same reference semantics as before.

```jsx
const defaultOnChange = () => {};
const defaultFoo = {};
const defaultBar = [];

function ExampleComponent({ onChange = defaultOnChange, foo = defaultFoo, bar = defaultBar }) {
  // ...
}
```
