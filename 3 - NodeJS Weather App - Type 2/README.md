Building a weather app with Node.js typically involves fetching weather data from an API (like OpenWeatherMap) and displaying it in a user-friendly format. Here's a simple guide to create a basic Node.js weather app.

### Step 1: Setting up the Project

1. **Initialize a Node.js project:**

   Open your terminal, navigate to the directory where you want your project to live, and run:
   ```bash
   npm init -y
   ```
   This will create a `package.json` file for your project.

2. **Install required packages:**

   You'll need a few packages:
   - `express` for the server.
   - `axios` to make HTTP requests to the weather API.
   - `dotenv` to manage your API key securely.

   Run this command to install the necessary dependencies:
   ```bash
   npm install express axios dotenv
   ```

### Step 2: Get an API Key

Sign up at [OpenWeatherMap](https://openweathermap.org/api) and get your API key.

### Step 3: Project Structure

Create the following file structure:

```
weather-app/
├── node_modules/
├── views/
│   └── index.ejs
├── .env
├── app.js
├── package.json
└── package-lock.json
```

### Step 4: Create the `.env` File

Create a `.env` file in your project root to store your API key:

```
API_KEY=your_openweathermap_api_key
```

### Step 5: Build the Express Server (`app.js`)

Now, let's build the basic server using Express. In your `app.js` file, add the following code:

```javascript
require('dotenv').config();
const express = require('express');
const axios = require('axios');
const app = express();

app.set('view engine', 'ejs');
app.use(express.urlencoded({ extended: true }));

const API_KEY = process.env.API_KEY;
const BASE_URL = 'http://api.openweathermap.org/data/2.5/weather';

app.get('/', (req, res) => {
    res.render('index', { weather: null, error: null });
});

app.post('/', async (req, res) => {
    const city = req.body.city;
    const url = `${BASE_URL}?q=${city}&appid=${API_KEY}&units=metric`;

    try {
        const response = await axios.get(url);
        const weatherData = response.data;
        const weatherText = `It's ${weatherData.main.temp}°C in ${weatherData.name}!`;
        res.render('index', { weather: weatherText, error: null });
    } catch (error) {
        res.render('index', { weather: null, error: 'Error, please try again' });
    }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
```

### Step 6: Create the EJS View (`views/index.ejs`)

Now, let's create a simple front-end form to submit a city name. In the `views/index.ejs` file, add the following:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
</head>
<body>
    <h1>Weather App</h1>
    <form action="/" method="POST">
        <input type="text" name="city" placeholder="Enter city" required>
        <button type="submit">Get Weather</button>
    </form>

    <% if (weather) { %>
        <p><%= weather %></p>
    <% } %>

    <% if (error) { %>
        <p><%= error %></p>
    <% } %>
</body>
</html>
```

### Step 7: Run the App

In your terminal, run the following command to start the server:

```bash
node app.js
```

Visit `http://localhost:3000` in your browser, enter a city name, and see the weather!

### Additional Enhancements:
- **Add Error Handling:** Ensure proper error handling for cases like invalid city names or network issues.
- **Add CSS:** Improve the UI by adding styles.
- **Extend Functionality:** You can display more weather data like humidity, wind speed, etc., from the OpenWeatherMap API.

Let me know if you'd like to extend this app or need help with other features!