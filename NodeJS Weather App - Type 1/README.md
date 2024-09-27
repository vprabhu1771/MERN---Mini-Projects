To build a simple weather app using Node.js, you can use an API like OpenWeatherMap to fetch weather data. Below is a step-by-step guide to create a basic weather app:

### Steps to Build the Node.js Weather App:

#### 1. **Set Up Node.js Environment**
- Install [Node.js](https://nodejs.org/) if you haven't already.

#### 2. **Create a New Project**
Open your terminal, create a directory for the app, and initialize it with `npm init`:
```bash
mkdir weather-app
cd weather-app
npm init -y
```

#### 3. **Install Required Packages**
You'll need `axios` to make HTTP requests and `express` to set up the server. Install them:
```bash
npm install express axios
```

#### 4. **Get an API Key from OpenWeatherMap**
- Go to [OpenWeatherMap](https://openweathermap.org/api), sign up, and get your API key.

#### 5. **Create an Express Server**
Create a file called `app.js` in the project directory. This will set up a simple Express server and fetch weather data.

```javascript
const express = require('express');
const axios = require('axios');
require('dotenv').config();

const app = express();
const port = 3000;

// Set up OpenWeatherMap API details
const apiKey = process.env.OPENWEATHER_API_KEY; // Store the API key in .env
const weatherApiUrl = 'http://api.openweathermap.org/data/2.5/weather';

// Serve static files (HTML/CSS)
app.use(express.static('public'));

// Route for home page
app.get('/', (req, res) => {
  res.sendFile(__dirname + '/public/index.html');
});

// Route to fetch weather by city
app.get('/weather', (req, res) => {
  const city = req.query.city;
  const url = `${weatherApiUrl}?q=${city}&appid=${apiKey}&units=metric`;

  axios.get(url)
    .then(response => {
      const data = response.data;
      res.json({
        city: data.name,
        temperature: data.main.temp,
        description: data.weather[0].description
      });
    })
    .catch(error => {
      console.error(error);
      res.json({ error: 'City not found!' });
    });
});

// Start the server
app.listen(port, () => {
  console.log(`Weather app listening at http://localhost:${port}`);
});
```

#### 6. **Create Frontend (HTML and CSS)**
Inside the `public` directory, create an `index.html` file to serve as the frontend UI.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Weather App</h1>
    <form id="weatherForm">
        <input type="text" id="cityInput" placeholder="Enter city name">
        <button type="submit">Get Weather</button>
    </form>
    <div id="result"></div>

    <script>
        document.getElementById('weatherForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const city = document.getElementById('cityInput').value;

            fetch(`/weather?city=${city}`)
                .then(response => response.json())
                .then(data => {
                    if (data.error) {
                        document.getElementById('result').innerText = data.error;
                    } else {
                        document.getElementById('result').innerHTML = 
                            `City: ${data.city}<br>Temperature: ${data.temperature}°C<br>Description: ${data.description}`;
                    }
                })
                .catch(error => console.error('Error fetching weather data:', error));
        });
    </script>
</body>
</html>
```

#### 7. **Create a `.env` File**
Store your OpenWeatherMap API key in a `.env` file to keep it secure.

Create a `.env` file in the root of the project:
```
OPENWEATHER_API_KEY=your_api_key_here
```

#### 8. **Run the Application**
To start the app, run the following command:
```bash
node app.js
```

Open your browser and go to `http://localhost:3000`. You should be able to enter a city name and retrieve the current weather information.

#### 9. **Styling (Optional)**
You can create a `styles.css` file inside the `public` directory to style the page as you like.

```css
body {
    font-family: Arial, sans-serif;
    text-align: center;
    margin-top: 50px;
}
form {
    margin-bottom: 20px;
}
input, button {
    padding: 10px;
    font-size: 16px;
}
#result {
    font-size: 18px;
    margin-top: 20px;
}
```

### Summary
You've created a simple weather app using Node.js and OpenWeatherMap. The Express server fetches weather data from the OpenWeatherMap API, and the frontend displays it dynamically when you enter a city name.