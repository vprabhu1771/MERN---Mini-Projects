### 6. **useMemo**
The `useMemo` hook memoizes the result of a computation, re-calculating it only when the dependencies change. It's useful for optimizing expensive calculations.

**Example: Expensive Calculation**

```jsx
import React, { useState, useMemo } from 'react';

function ExpensiveComponent({ num }) {
  const expensiveCalculation = (num) => {
    console.log('Calculating...');
    return num * 2;
  };

  const result = useMemo(() => expensiveCalculation(num), [num]); // Only re-computes when `num` changes

  return <div>Calculated result: {result}</div>;
}

function App() {
  const [number, setNumber] = useState(5);
  return <ExpensiveComponent num={number} />;
}

export default App;
```

- `useMemo(() => expensiveCalculation(num), [num])` ensures the expensive calculation only happens when `num` changes.