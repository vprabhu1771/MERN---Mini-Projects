### 4. **useReducer**
The `useReducer` hook is an alternative to `useState` for managing complex state logic. It takes a reducer function and an initial state, then returns the current state and a dispatch function.

**Example: Counter with `useReducer`**

```jsx
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}

export default Counter;
```

- `useReducer(reducer, initialState)` returns the current state and `dispatch`.
- The `dispatch` function triggers state changes by sending actions.