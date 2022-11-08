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
- [Theming](#theming)
- [Components](#components)
  - [Lifecycle](#lifecycle)
  - [Testing](#testing)
  - [Implementation](#implementation)
    - [Persistence](#persistence)
- [Layouts](#layouts)
- [Styling](#styling)
- [React](#react)
  - [API Design](#api-design)
    - [Changes](#changes)
    - [Questions](#questions)
  - [Components](#components-1)
    - [Stable prop callbacks](#stable-prop-callbacks)
    - [Stable default values](#stable-default-values)
    - [Working with timeouts](#working-with-timeouts)
  - [Hooks](#hooks)
    - [Prefer accepting a `ref` instead of creating and returning one](#prefer-accepting-a-ref-instead-of-creating-and-returning-one)
    - [Using `useCallback` and `useMemo`](#using-usecallback-and-usememo)
    - [Prefer returning event handlers versus adding them](#prefer-returning-event-handlers-versus-adding-them)
  - [Refs](#refs)
    - [Prefer using `useMergedRefs` when using `forwardRef`](#prefer-using-usemergedrefs-when-using-forwardref)
  - [Testing](#testing-1)
  - [Recipes](#recipes)
    - [`useEvent`](#useevent)
    - [`useMergedRefs`](#usemergedrefs)
    - [`useStableCallback`](#usestablecallback)
    - [`useTimeout`](#usetimeout)
- [Releases](#releases)
  - [Packages](#packages)
- [Community](#community)
  - [Proposals](#proposals)
  - [Support](#support)
  - [Contribution](#contribution)
- [Testing](#testing-2)
  - [Black box](#black-box)
  - [Automated checks](#automated-checks)
- [Links & Resources](#links--resources)

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

## Theming

**Topics**

- Light & Dark mode
  - Setting user preference
    - Avoiding flash of theme switch if loading in via JavaScript (support SSR
      use-case)
  - Conditional rendering of items based on theme (illustrations)

## Components

**Gotchas**

- When using a modal dialog, `<header>` and `<footer>` semantics are not
  stripped if the dialog is appended to the body
- When using some form of pop-up, if it is positioned in DOM order it will be
  cut off if a parent has overflow hidden
- When using `localStorage`, it may be helpful to wrap calls to prevent errors
  if a user has this disabled in their browser

### Lifecycle

- Unstable
- Stable
- Intermediate stages
- Evolving a stable component
  - Adding functionality
  - Removing functionality
    - Deprecation cycle
      - Deprecate in current major release, notify of support policy
      - Remove in either next release or n + 1 release
  - Tools
    - Codemods

### Testing

- Areas
  - Visual
    - Visual Regression Testing
  - Behavior
    - User interactions
    - States
  - Accessibility
    - Automated checks
    - Manual checks
  - Types
    - Unknown type is not valid
    - Types autocomplete correctly
    - Types are sufficiently narrow

### Implementation

#### Persistence

- Local storage

## Layouts

Layouts in a design system define the common structure and presentation of your
product. They create consistent and predictable structure to pages and foster
cohesion across page and product boundaries.

Providing common layouts provide a way to quickly build pages with similar data
requirements. Providing a foundational grid system allows any layout to feel
like it belongs in the family of products even if it does not use a pre-defined
layout.

Some great layouts and corresponding components to get started with include:

- A general grid system, with Grid and Column components
  - Subgrid support is a plus
- Vertical rhythm support through a Stack component
- [Pancake](https://web.dev/one-line-layouts/#04-pancake-stack-grid-template-rows-auto-1fr-auto)
  and
  [Holy grail](https://web.dev/one-line-layouts/#05-classic-holy-grail-layout-grid-template-auto-1fr-auto-auto-1fr-auto)
  layouts
- Side-nav and panel placement layouts
- Flex components that derive gap based on spacing system

**Techniques**

- CSS Grid, grid areas
- N column Grid

**Links & Resources**

- https://web.dev/one-line-layouts/
- https://every-layout.dev/
- https://ryanmulligan.dev/blog/layout-breakouts/
- https://www.joshwcomeau.com/css/full-bleed/
- Grid
  - https://carbon-elements.netlify.app/grid/examples/css-grid/
  - https://carbon-elements.netlify.app/grid/examples/preview/
  - https://getbootstrap.com/docs/5.1/layout/css-grid/

## Styling

**Approaches**

- Native CSS
- CSS Modules
- Sass
- CSS-in-JS
  - https://vimeo.com/116209150?embedded=true&source=video_title&owner=24051491
  - https://www.youtube.com/watch?v=9JZHodNR184

**Topics**

- Runtime vs zero runtime styling approaches

**Techniques**

- Linting for specific properties in style to make sure they come from design
  language or tokens

## React

### API Design

**Approaches**

- Kitchen-sink
  - Pros
    - Easy to coordinate complex behavior
    - Able to swap out implementation without breaking changes
  - Cons
    - Lack of flexibility
- Structural
  - Pros
    - Maximum flexibility
  - Cons
    - Hard to build in behavior, requires orchestration
- Layouts
- Behavior
- Styling
- Async

**Questions**

- When do you add to a component versus create another component?
  - Are you adding multiple props? Do they only work with each other and don't
    make sense without each other?

#### Changes

| Category | Situation                                                                      | Semver |
| :------- | :----------------------------------------------------------------------------- | :----- |
| Children | Change from a wrapping element to a fragment                                   |        |
|          | Change from fragment to wrapping element                                       |        |
|          | Change node type of wrapping element                                           |        |
| Props    | A prop is added                                                                | minor  |
|          | A prop is removed                                                              | major  |
|          | A prop is renamed (make sure to follow deprecation flow)                       | major  |
|          | The propType broadens or accepts more types                                    | minor  |
|          | The propType narrows or accepts fewer types                                    | major  |
| Refs     | Add a ref to a component                                                       | minor  |
|          | Remove a ref from a component                                                  | major  |
|          | Move where a ref is placed within a component                                  |        |
| Styles   | A class name, style data attribute, etc is added                               |        |
|          | The position or display value of an element changes                            |        |
|          | The position of a parent element changes (e.g. changes to `position: relative` |        |
| Types    | A type is added                                                                |        |
|          | The type is removed                                                            |        |
|          | The type broadens or accepts more types                                        |        |
|          | The type narrows or accepts fewer types                                        |        |

#### Questions

- When to add to a component versus creating a new component?
  - If the props that are being added can exist independently, it might be
    useful to bake into an existing component
  - If props being added are coupled, in other words they don't only exist for
    each other and specifying one without the other doesn't make sense, it might
    be worth exploring a new component that combines these concepts

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

Use a wrapper hook like [`useTimeout`](#usetimeout) when working with
`setTimeout` inside of a component. This provides safety so that you can
guarantee that no `setTimeout` function will run after a component unmounts.

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

#### Prefer returning event handlers versus adding them

When working with hooks that listen to events like `onKeyDown`, `onClick`, etc
you have two options for how to bind logic to these events:

- Return a function that the caller can attach to their component

  ```jsx
  function MyComponent() {
    const { onKeyDown } = useCustomHook();

    return <CustomComponent onKeyDown={onKeyDown} />;
  }
  ```

- Accept a `ref` and call `addEventListener` in an effect

  ```jsx
  function MyComponent() {
    const ref = React.useRef(null);

    useCustomHook(ref);

    return <CustomComponent ref={ref} />;
  }

  // useCustomHook.js
  function useCustomHook(ref) {
    React.useEffect(() => {
      function listener(event) {
        // ...
      }

      const { current: node } = ref;
      node.addEventListener('keydown', listener);
      return () => {
        node.removeEventListener('keydown', listener);
      };
    }, []);
  }
  ```

- Pros of returning a function
  - The caller has control over when this behavior is applied
  - It's clear, especially with multiple handlers for the same event, what is
    being called
- Cons of returning a function
  - The caller is responsible for placing the function on the correct element
  - Changing the event requires a breaking change (or an intermediate layer that
    obfuscates props)
- Pros of accepting a `ref`
  - You can swap out the implementation or event without a Public API change
- Cons of accepting a `ref`
  - It's difficult to enable or disable this behavior due to conditional hooks
    (would likely need an option or flag passed to turn off)
  - It can hide the fact that a keydown event is being registered on the element
  - The caller is responsible for creating a ref and placing it on the correct
    element

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

### Testing

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

## Releases

### Packages

- Release cadency
  - Merge
  - Time-based
  - On-demand
- Versioning
  - Prerelease (0.x)
  - Semver (1.x)
- Multi-package vs single package
- Bundling
  - Side-effects
  - `package.json` exports
- Breaking changes
  - Prerelease packages
  - Separate changes into branch or feature flag

## Community

### Proposals

### Support

**Areas**

- Slack
- GitHub issues
- GitHub discussions
- Office hours
- Design crits

**SLA**

- What level of support do you provide for teams?
  - When can they expect to hear back from you when they make an issue?
  - How long should they expect it take to address an issue?
- How do teams stay engaged with updates?
- What level of support should teams expect from contributions or proposals?

### Contribution

## Testing

It's important to test the API and the implementation of what is delivered in a
design system. Testing the API ensures that you're not accidentally shipping a
breaking change to the code that you're shipping for developers to use. Testing
the implementation makes sure that the appearance and behavior of the component
does not change unexpected for end users (or for developers using the design
system who may be relying on that behavior).

It's also important to include certain structural and automated tests to make
sure that the foundations of the design system (like tokens or the underlying
design language or brand) setup teams for success by shipping accessible
primitives.

### Black box

When considering how to test components or aspect of your design system, it is
helpful to consider the overall interface and ways in which a developer or user
interacts with your component. These different factors make up the overall
contract of your component and should be what you prioritize in your testing
strategy. In addition, it's helpful to frame tests from this "black box" model
in order to allow you to make changes as needed to the internal structure of a
component or module.

As a rough guide, here are some things that are useful to incorporate into your
testing strategy:

- The API of your component, how does a developer use this in their project?
- How does an end-user interact with this component?
  - Make sure to consider mouse and keyboard and test with both
  - If a specific role or `aria-*` property is necessary for a screen reader,
    make sure this is also tested
- What is the expected visual appearance of this component across themes?
  (Visual Regression Testing)
- Anything related to how your team defines semver that you want to make sure is
  captured (for example, changing the location of a `ref` if you're writing a
  React component)

It is also helpful to include certain automated checks, like color contrast for
design color tokens or automated accessibility checks, in your testing strategy.

### Automated checks

- Make sure all possible design color token combinations meet contrast
  requirements
- Run an accessibility checker to catch common issues, axe or Accessibility
  Checker are great tools to get started

## Links & Resources

- https://component.gallery/
- https://designsystemsrepo.com/design-systems/
