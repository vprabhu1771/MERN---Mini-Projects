### 3. **useContext**
The `useContext` hook is used to consume context values in a function component, allowing for easier data sharing across the component tree.

To implement dark and light theme switching in a React app using the Context API with Vite, follow these steps:

### Step 1: Set up the Vite project
If you haven’t already created a Vite project, use the following commands to set it up:
```bash
npm create vite@latest
cd your-project-name
npm install
```

### Step 2: Create the ThemeContext

Inside the `src` folder, create a new folder named `context` and add a file called `ThemeContext.jsx`. This file will hold the theme logic.

```jsx
// src/context/ThemeContext.jsx
import { createContext, useState, useContext, useEffect } from 'react';

// Create a context for theme
const ThemeContext = createContext();

// Custom hook to use ThemeContext
export const useTheme = () => {
  return useContext(ThemeContext);
};

// Theme provider component
export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState(() => localStorage.getItem('theme') || 'light');

  // Toggle between light and dark mode
  const toggleTheme = () => {
    setTheme(prevTheme => (prevTheme === 'light' ? 'dark' : 'light'));
  };

  // Save the theme to localStorage and update the body class
  useEffect(() => {
    localStorage.setItem('theme', theme);
    document.body.className = theme; // Add theme class to body for CSS styling
  }, [theme]);

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};
```

### Step 3: Create a Theme Switch Button

Next, create a `ThemeSwitcher` component to allow users to toggle between dark and light themes. You can add this anywhere in your app.

```jsx
// src/components/ThemeSwitcher.jsx
import { useTheme } from '../context/ThemeContext';

const ThemeSwitcher = () => {
  const { theme, toggleTheme } = useTheme();

  return (
    <button onClick={toggleTheme}>
      Switch to {theme === 'light' ? 'Dark' : 'Light'} Mode
    </button>
  );
};

export default ThemeSwitcher;
```

### Step 4: Wrap Your App with the ThemeProvider

In your `main.jsx` file, wrap the app with the `ThemeProvider` to make the theme context available throughout the application.

```jsx
// src/main.jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { ThemeProvider } from './context/ThemeContext';
import './index.css'; // Include the CSS styles for both light and dark modes

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <ThemeProvider>
      <App />
    </ThemeProvider>
  </React.StrictMode>
);
```

### Step 5: Add CSS for Light and Dark Themes

You can define the CSS for light and dark modes in `index.css` (or any global CSS file you're using):

```css
/* src/index.css */
body.light {
  background-color: white;
  color: black;
}

body.dark {
  background-color: black;
  color: white;
}

/* Optional: Style for the toggle button */
button {
  padding: 0.5rem 1rem;
  margin: 1rem;
  cursor: pointer;
  border: none;
  border-radius: 5px;
}

button:hover {
  background-color: lightgray;
}
```

### Step 6: Use the Theme Switcher in the App

Finally, you can use the `ThemeSwitcher` component anywhere in your app to allow users to switch themes.

```jsx
// src/App.jsx
import ThemeSwitcher from './components/ThemeSwitcher';

const App = () => {
  return (
    <div>
      <h1>React Context API Theme Switcher</h1>
      <ThemeSwitcher />
    </div>
  );
};

export default App;
```

### Running the app

Now, start the Vite dev server to test the theme switcher:

```bash
npm run dev
```

### Explanation:

- **Context API**: Provides a way to manage the state (in this case, the theme) and share it across the entire app without needing to pass props manually at every level.
- **localStorage**: Used to persist the theme across page reloads.
- **CSS**: `body` classes `light` and `dark` define the theme styles, ensuring the theme applies globally.

Now you should have a React app with a dark and light theme toggle using Vite!
