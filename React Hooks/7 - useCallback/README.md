### 7. **useCallback**
The `useCallback` hook memoizes a function and returns a stable reference to it across renders. It’s helpful for optimizing re-rendering of child components.

**Example: Callback Memoization**

```jsx
import React, { useState, useCallback}  from 'react';

function Counter() {

    const [count, setCount] = useState(0);

    const incrementClick = useCallback(() => {
        setCount(count + 1);
    }, [count]); // handleClick only changes when `count` changes

    const decrementClick = useCallback(() => {
    setCount(count - 1);
    }, [count]); // handleClick only changes when `count` changes

    function IncrementUI({ onClick }) {
        console.log('Increment rendered');
        return <button onClick={onClick}>+</button>
    }

    function DecrementUI({ onClick }) {
        console.log('Decrement rendered');
        return <button onClick={onClick}>-</button>
    }
  
  return (
    <div>
       <p>Count: {count}</p>
        <IncrementUI onClick={incrementClick}/>
        <DecrementUI onClick={decrementClick}/>
    </div>
  );
}

export default Counter;
```

- `useCallback(() => setCount(count + 1), [count])` returns the same `handleClick` function across renders unless `count` changes.

These hooks are the foundation of React's functional component capabilities, helping developers manage state, effects, and performance optimizations in a simple and declarative way.