### 2. **useEffect**
The `useEffect` hook allows you to perform side effects in function components, such as fetching data, subscribing to a service, or manually changing the DOM.

**Example: Fetch Data on Component Mount**

```jsx
import React, { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState([]);

  useEffect(() => {
    // Perform a fetch request when the component mounts
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(response => response.json())
      .then(json => setData(json));
  }, []); // Empty array means the effect runs once when the component mounts

  return (
    <div>
      <h1>Fetched Data:</h1>
      <ul>
        {data.map(item => (
          <li key={item.id}>{item.title}</li>
        ))}
      </ul>
    </div>
  );
}

export default DataFetcher;
```

- The `useEffect` hook runs the fetch function when the component mounts.
- The empty dependency array (`[]`) ensures it only runs once.