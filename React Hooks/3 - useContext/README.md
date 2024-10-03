### 3. **useContext**
The `useContext` hook is used to consume context values in a function component, allowing for easier data sharing across the component tree.

**Example: Theme Context**

```jsx
import React, { useContext } from 'react';

// Create a Context for the theme
const ThemeContext = React.createContext('light');

function ThemedButton() {
  const theme = useContext(ThemeContext); // Use the theme from context
  return <button style={{ background: theme === 'dark' ? '#333' : '#fff' }}>I am styled by theme!</button>;
}

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <ThemedButton />
    </ThemeContext.Provider>
  );
}

export default App;
```

- `useContext(ThemeContext)` reads the current value from the `ThemeContext`.
- The button’s background color changes based on the current theme.