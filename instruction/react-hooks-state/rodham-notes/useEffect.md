
# useEffect

Conceptually, components are intended to be pure functions of props and state.

The JSX they return is determined by their prop and state variable values.

However, components often need to execute logic in addition to rendering.

- Fetching data from an API
- Setting up subscriptions or event listeners
- Updating the DOM manually
- Starting a timer/interval
- Cleaning up resources (closing WebSocket, removing listeners, etc.)

## Basic Example

```
import React, { useState, useEffect } from "react";

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Clicked ${count} times`;
  }, [count]); // only rerun if count changes

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(c => c + 1)}>Click me</button>
    </div>
  );
}
```

Here, the effect updates the browser tab title whenever count changes.

## Anatomy of useEffect

```
useEffect(() => {
  // effect code here (runs after render)

  return () => {
    // optional cleanup code here (runs before next effect or unmount)
  };
}, [dependencies]);
```

Effect function: runs after the render is committed to the DOM.

Cleanup function: runs before the effect re-runs or when the component unmounts.

Dependency array: controls when the effect runs.

`[]` → run once (on mount/unmount).

`[count]` → run when count changes.

`Omit` → run after every render.

## Common Use Cases

### Fetching data

```
useEffect(() => {
  fetch("/api/data")
    .then(res => res.json())
    .then(setData);
}, []); // only on mount
```

### Event listeners

```
useEffect(() => {
  const onResize = () => console.log("Window resized");
  window.addEventListener("resize", onResize);
  return () => window.removeEventListener("resize", onResize);
}, []); // set up once
```

### Timers

```
useEffect(() => {
  const id = setInterval(() => console.log("Tick"), 1000);
  return () => clearInterval(id); // cleanup
}, []);
```

### Syncing with external systems

```
useEffect(() => {
  localStorage.setItem("theme", theme);
}, [theme]);
```
