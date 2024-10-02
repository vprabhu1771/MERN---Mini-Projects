To create a full-stack MERN (MongoDB, Express, React, Node.js) app that retrieves system information (RAM, OS, monitor, etc.) from the user's machine and stores it in a MongoDB database, you will need the following:

### **Frontend (React)**:
The frontend will gather the basic system information using JavaScript and make a request to the backend to store this data.

### **Backend (Node.js + Express)**:
The backend will receive the system information, process it, and save it into a MongoDB database.

---

## **Step-by-Step Guide**

### **1. Setting Up the MERN Stack Project**

#### 1.1. **Backend: Initialize Node.js and Express**

- Create a new directory for your project and initialize Node.js:
  ```bash
  mkdir mern-system-info
  cd mern-system-info
  npm init -y
  ```

- Install the required packages:
  ```bash
  npm install express mongoose systeminformation cors
  ```

- Install `nodemon` for development:
  ```bash
  npm install --save-dev nodemon
  ```

- Set up the `package.json` scripts:
  ```json
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  }
  ```

#### 1.2. **Backend: Basic Express Server Setup**

Create a `server.js` file in the root of your project:

```js
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
const si = require('systeminformation');
require('dotenv').config();

// Initialize express
const app = express();

// Middleware
app.use(cors());
app.use(express.json());

// Connect to MongoDB
mongoose.connect(process.env.MONGODB_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
.then(() => console.log('MongoDB connected'))
.catch(err => console.error('MongoDB connection error:', err));

// Define a schema and model for storing system info
const systemSchema = new mongoose.Schema({
  os: String,
  platform: String,
  ram: Number,
  screen: {
    width: Number,
    height: Number,
  },
  date: {
    type: Date,
    default: Date.now
  }
});

const SystemInfo = mongoose.model('SystemInfo', systemSchema);

// Endpoint to receive system info from frontend and save it
app.post('/api/system-info', async (req, res) => {
  try {
    const systemInfo = new SystemInfo(req.body);
    await systemInfo.save();
    res.status(200).send('System info saved');
  } catch (error) {
    res.status(500).send('Server Error: Could not save system info');
  }
});

// Get system info (Node.js side for testing)
app.get('/api/system-info/node', async (req, res) => {
  try {
    const osInfo = await si.osInfo();
    const memInfo = await si.mem();
    const graphics = await si.graphics();

    const systemData = {
      os: osInfo.distro,
      platform: osInfo.platform,
      ram: memInfo.total / (1024 ** 3), // Convert bytes to GB
      screen: graphics.displays[0] ? {
        width: graphics.displays[0].resolutionx,
        height: graphics.displays[0].resolutiony
      } : { width: 'Unknown', height: 'Unknown' }
    };
    res.status(200).json(systemData);
  } catch (error) {
    res.status(500).json({ message: 'Error fetching system info', error });
  }
});

// Start the server
const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

#### 1.3. **Backend: .env File**

Create a `.env` file to store your MongoDB URI:
```bash
MONGODB_URI=mongodb://localhost:27017/systeminfo
```

#### 1.4. **Backend: Mongoose Model**
The backend uses the Mongoose model (`SystemInfo`) to store OS, RAM, screen dimensions, and platform data.

---

### **2. Setting Up the Frontend (React)**

#### 2.1. **Frontend: Create React App**

In the root folder, create a new `client` directory and initialize a React app:

```bash
npx create-react-app client
cd client
npm install axios
```

#### 2.2. **Frontend: System Information Retrieval**

Edit `src/App.js` to gather system information and send it to the backend:

```jsx
import React, { useEffect, useState } from 'react';
import axios from 'axios';

function App() {
  const [systemInfo, setSystemInfo] = useState(null);

  // Function to gather basic system info
  const getSystemInfo = () => {
    return {
      os: navigator.userAgent, // OS info
      platform: navigator.platform, // Platform (Win32, MacIntel)
      ram: navigator.deviceMemory || 'Unknown', // RAM (GB, only works in Chromium-based browsers)
      screen: {
        width: window.screen.width, // Screen width
        height: window.screen.height, // Screen height
      }
    };
  };

  // Function to send system info to the backend
  const sendSystemInfoToBackend = async () => {
    try {
      const systemInfo = getSystemInfo();
      setSystemInfo(systemInfo);

      await axios.post('http://localhost:5000/api/system-info', systemInfo);
      console.log('System info sent successfully');
    } catch (error) {
      console.error('Error sending system info:', error);
    }
  };

  useEffect(() => {
    sendSystemInfoToBackend();
  }, []);

  return (
    <div className="App">
      <h1>System Information</h1>
      {systemInfo ? (
        <div>
          <p>OS: {systemInfo.os}</p>
          <p>Platform: {systemInfo.platform}</p>
          <p>RAM: {systemInfo.ram} GB</p>
          <p>Screen Width: {systemInfo.screen.width} px</p>
          <p>Screen Height: {systemInfo.screen.height} px</p>
        </div>
      ) : (
        <p>Loading system info...</p>
      )}
    </div>
  );
}

export default App;
```

#### 2.3. **Frontend: CORS Setup**
To allow the React app to communicate with the backend, ensure CORS is enabled in `server.js`:
```js
app.use(cors());
```

---

### **3. Run the Application**

#### 3.1. **Backend**

Start the backend server:
```bash
npm run dev
```

#### 3.2. **Frontend**

Start the React app:
```bash
cd client
npm start
```

---

### **4. MongoDB Setup**
Ensure MongoDB is running locally or use MongoDB Atlas to host the database. Your data will be stored in the `systeminfo` collection with details about the user's machine.

---

### **Summary of the System**

- **Frontend (React)**: Collects basic system info (OS, RAM, screen size) using browser APIs.
- **Backend (Node.js + Express)**: Receives system info from React and stores it in MongoDB using Mongoose.
- **MongoDB**: Stores the system information sent from the frontend.

You can extend this system by adding more detailed information or building out the frontend with more features like fetching and displaying data from the MongoDB database.