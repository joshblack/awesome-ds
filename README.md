# awesome-ds

A personal collection of resources, notes, and references for Design Systems

<!-- prettier-ignore-start -->
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of Contents

- [Design Language](#design-language)
  - [Tooling](#tooling)
  - [Icons](#icons)
    - [Tooling](#tooling-1)
- [Design Tokens](#design-tokens)
- [Components](#components)
- [React](#react)
  - [Components](#components-1)
    - [Stable prop callbacks](#stable-prop-callbacks)
    - [Stable default values](#stable-default-values)
  - [Hooks](#hooks)
    - [Prefer accepting a `ref` instead of creating and returning one](#prefer-accepting-a-ref-instead-of-creating-and-returning-one)
    - [Using `useCallback` and `useMemo`](#using-usecallback-and-usememo)
  - [Refs](#refs)
    - [Prefer using `useMergedRefs` when using `forwardRef`](#prefer-using-usemergedrefs-when-using-forwardref)
  - [Recipes](#recipes)
    - [`useEvent`](#useevent)
    - [`useMergedRefs`](#usemergedrefs)
    - [`useStableCallback`](#usestablecallback)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- prettier-ignore-end -->

## Design Language

**Areas**

- Color scales
- Typography
- Motion
- Spacing
- Layout
- Icons

### Tooling

### Icons

#### Tooling

## Design Tokens

## Components

**Common issues**

- When using a modal dialog, `<header>` and `<footer>` semantics are not
  stripped if the dialog is appended to the body
- When using some form of pop-up, if it is positioned in DOM order it will be
  cut off if a parent has overflow hidden

## React

### Components

#### Stable prop callbacks

A component may accept callback functions as props, such as an `onChange`, that
is invoked in response to an action or event.

```jsx
function ExampleComponent({ onClick }) {
  return (
    <button type="button" onClick={() => onClick()}>
      Example
    </button>
  );
}
```

At times, this handler may need to be referenced in an effect or in other
primitive that requires a dependency array. This may result in the following
implementation:

```jsx
function ExampleComponent({ onChange }) {
  // ...

  useEffect(() => {
    onChange(state);
  }, [state, onChange]);

  // ...
}
```

In this situation, the effect will fire each time the `state` value updates but
also each time the `onClick` function reference changes. If you're expecting
consumers to use values like an arrow function expression, this may lead to
unexpected work being performed.

```jsx
<ExampleComponent
  onChange={() => {
    /* ... */
  }}
/>
```

There may also be a correctness aspect to this, as well, in that the effect
should not be synchronizing with the identity of the `onChange` function and is
firing unexpected as a result. For example, a `ResizeObserver` implementation
may look like:

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

In this situation, the `useEffect` will run each time the reference of
`onResize` changes. As a result, the callback to `ResizeObserver` will also call
`onResize` as this is the default behavior of this primitive. Depending on what
`onResize` does when called, this may lead to an infinite loop as the state of
the parent changes which in turn causes the `onResize` to change, which then
updates the `useEffect`, and finally the `ResizeObserver` callback before
looping again.

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

This allows you to always have the latest reference to `onChange` while still
having a stable reference in the effect as to avoid adding it to the depenendecy
array.

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

function ExampleComponent({
  onChange = defaultOnChange,
  foo = defaultFoo,
  bar = defaultBar,
}) {
  // ...
}
```

#### Working with timeouts

Use a wrapper hook like [`useTimeout`](#usetimeout) when working with `setTimeout` inside of a component. This provides safety so that you can guarantee that no `setTimeout` function will run after a component unmounts.

### Hooks

#### Prefer accepting a `ref` instead of creating and returning one

<table>
<thead><tr><th>Unpreferred</th><th>Preferred</th></tr></thead>
<tbody>
<tr><td>

```scss
const { ref } = useExample()
```

</td><td>

```scss
const ref = useRef(null);
useExample(ref)
```

</td></tr>
</tbody>
</table>

When designing hooks that require a reference to a DOM node (using a `ref`) you
should design the hook to take in a `ref` as an argument instead of creating a
`ref` on behalf of the caller.

This is important when a caller decides to use multiple hooks that rely on a
`ref`. For example,

```jsx
function MyComponent() {
  const [ref1, isHovering] = useHover();
  const [ref2, isDragging] = useDrag();

  // How should the caller merge these two refs?
}
```

If, instead, these hooks took in a `ref` we could have the caller manage the
`ref` and pass it into the hooks.

```jsx
function MyComponent() {
  const ref = useRef(null);
  const isHovering = useHover(ref);
  const isDragging = useDrag(ref);

  // Caller has to add `ref` to a node below
}
```

#### Using `useCallback` and `useMemo`

`useCallback` and `useMemo` can be incredibly useful tools in certain
situations. In general, however, we try to avoid them unless one of the
following conditions occur:

- The identity of a function or object is required as a dependency in a
  dependency array
- We have observed performance issues due to allocations that can be reproduced
  and resolved using these techniques

This practice is to avoid introducing `useCallback` and `useMemo` prematurely,
which can create extra work for our components to perform.

A rule of thumb for this is to understand how frequently a dependency will
update that is given to `useCallback` or `useMemo`. If a dependency is likely to
update frequently, then React will have to perform comparisons and re-run
callback to `useCallback` and `useMemo`. This would be slower than creating a
new function each render instead.

### Refs

#### Prefer using `useMergedRefs` when using `forwardRef`

```jsx
const ExampleComponent = React.forwardRef(function ExampleComponent(
  props,
  forwardRef
) {
  const inputRef = React.useRef(null);
  const ref = useMergedRefs(forwardRef, inputRef);

  // ...

  return <input ref={ref} type="text" />;
});
```

### Recipes

#### `useEvent`

```js
import * as React from 'react';

function useEvent(elementOrRef, name, callback) {
  const ref = React.useRef(null);

  React.useEffect(() => {
    ref.current = callback;
  });

  React.useEffect(() => {
    const element = elementOrRef.current ?? elementOrRef;

    function listener(event) {
      ref.current?.(event);
    }

    element.addEventListener(name, listener);

    return () => {
      element.removeEventListener(name, listener);
    };
  }, [elementOrRef, name]);
}
```

#### `useMergedRefs`

```js
import * as React from 'react';

function useMergedRefs(...refs) {
  return React.useCallback((node) => {
    for (const ref of refs) {
      if (!ref) {
        continue;
      }

      if (typeof ref === 'function') {
        ref(node);
      } else if (typeof ref === 'object') {
        ref.current = node;
      }
    }
  }, []);
}
```

#### `useStableCallback`

aka `useSavedCallback`

```js
import * as React from 'react';

function useStableCallback(callback) {
  const ref = React.useRef(null);

  React.useEffect(() => {
    ref.current = callback;
  });

  return React.useCallback((...args) => {
    return ref.current?.(...args);
  }, []);
}
```

#### `useTimeout`

```js
import * as React from 'react';

export function useTimeout() {
  const timers = React.useRef(null);
  
  if (timers.current === null) {
    timers.current = new Set();
  }

  React.useEffect(() => {
    return () => {
      for (const id of timers.current) {
        clearTimeout(id);
      }
      timers.current.clear();
    };
  }, []);

  return React.useCallback((fn, delayMs) => {
    const timeoutId = setTimeout(() => {
      fn();
      timers.current.delete(timeoutId);
    }, delayMs);
    
    timers.current.add(timeoutId);

    return timeoutId;
  }, []);
}
```
