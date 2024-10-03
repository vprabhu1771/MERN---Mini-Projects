### 5. **useRef**
The `useRef` hook provides a way to persist values between renders without causing re-renders. It's often used to access DOM elements or store mutable values.

**Example: Accessing a DOM element**

```jsx
import React, { useRef } from 'react';

function TextInput() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus(); // Access DOM input and focus it
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus the input</button>
    </div>
  );
}

export default TextInput;
```

- `useRef()` creates a reference object (`inputRef`), and `inputRef.current` refers to the input DOM element.