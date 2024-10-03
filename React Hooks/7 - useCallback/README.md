### 7. **useCallback**
The `useCallback` hook memoizes a function and returns a stable reference to it across renders. It’s helpful for optimizing re-rendering of child components.

**Example: Callback Memoization**

```jsx
import React, { useState, useCallback } from 'react';

function Child({ onClick }) {
  console.log('Child rendered');
  return <button onClick={onClick}>Click me</button>;
}

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]); // handleClick only changes when `count` changes

  return (
    <div>
      <p>Count: {count}</p>
      <Child onClick={handleClick} />
    </div>
  );
}

export default Parent;
```

- `useCallback(() => setCount(count + 1), [count])` returns the same `handleClick` function across renders unless `count` changes.

These hooks are the foundation of React's functional component capabilities, helping developers manage state, effects, and performance optimizations in a simple and declarative way.