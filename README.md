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

```sx
const defaultOnChange = () => {};
const defaultFoo = {};
const defaultBar = [];

function ExampleComponent({ onChange = defaultOnChange, foo = defaultFoo, bar = defaultBar }) {
  // ...
}
```
