To create a password generator in React that also allows the user to download the generated password in a `secret.txt` file, follow these steps:

1. Generate the password using a random string generator.
2. Provide an option to download the password as a text file.

Here’s a complete example:

```jsx
import React, { useState } from 'react';

const PasswordGenerator = () => {
  const [password, setPassword] = useState('');

  // Function to generate a random password
  const generatePassword = () => {
    const charset = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()_+';
    let password = '';
    for (let i = 0; i < 12; i++) {
      const randomIndex = Math.floor(Math.random() * charset.length);
      password += charset[randomIndex];
    }
    setPassword(password);
  };

  // Function to download the password as a file
  const downloadPassword = () => {
    const element = document.createElement('a');
    const file = new Blob([password], { type: 'text/plain' });
    element.href = URL.createObjectURL(file);
    element.download = 'secret.txt';
    document.body.appendChild(element); // Required for Firefox
    element.click();
  };

  return (
    <div style={{ padding: '20px' }}>
      <h2>Password Generator</h2>
      <button onClick={generatePassword}>Generate Password</button>
      {password && (
        <>
          <div style={{ marginTop: '20px' }}>
            <strong>Generated Password:</strong> {password}
          </div>
          <button onClick={downloadPassword} style={{ marginTop: '10px' }}>
            Download Password
          </button>
        </>
      )}
    </div>
  );
};

export default PasswordGenerator;
```

### How this works:
1. **Password Generation**: When the user clicks the "Generate Password" button, a random password is generated using characters from the `charset` and stored in the `password` state.
2. **Download Feature**: After generating the password, the "Download Password" button becomes visible. This button triggers the `downloadPassword` function, which creates a downloadable file (`secret.txt`) containing the password.

### Running the app:
1. Make sure you have a React environment set up (e.g., using Create React App).
2. Add this component to your app.
3. You will be able to generate and download the password as a text file.