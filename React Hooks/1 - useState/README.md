React hooks provide a way to use state and lifecycle features in functional components. Here are detailed examples of some of the most commonly used hooks:

### 1. **useState**
The `useState` hook allows you to add state to your functional components. It returns an array with two elements: the current state and a function to update the state.

**Example: Counter**

```jsx
import React, { useState } from 'react';

function Counter() {
  // Declare a state variable called count, and set its initial value to 0
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}

export default Counter;
```

- `useState(0)` initializes the state with `0`.
- `setCount` updates the state when the button is clicked, incrementing `count`.