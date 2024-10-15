To create a **MERN** (MongoDB, Express, React, Node.js) project that uses an environment variable for MongoDB connection and integrates **Arduino** for an LED blinking function, here’s a breakdown of how to set it up.

### Steps Overview:
1. **Set up MongoDB with an environment variable in your Node.js backend**.
2. **Create an Express route for controlling the LED using Arduino**.
3. **Connect the Arduino for LED control** (using serial communication).
4. **Create a React frontend for user interaction**.

### 1. Setting up MongoDB with Environment Variables in Node.js

**Backend - Express Setup:**

1. **Install necessary packages**:
   ```bash
   npm init -y
   npm install express mongoose dotenv
   ```

2. **Set up `.env` file**:
   Create a `.env` file in the root of your project and add your MongoDB URI:
   ```
    HOST=192.168.1.122
    PORT=9000
    MONGO_URI=mongodb://localhost:27017/mydatabase
    ```

3. **Configure `server.js`**:
```javascript
require('dotenv').config(); // Load environment variables
const express = require('express');
const mongoose = require('mongoose');
const SerialPort = require('serialport');
const Readline = require('@serialport/parser-readline');
const cors = require('cors'); // Import CORS

const app = express();
const host = process.env.HOST || '0.0.0.0'; // Changed to 'localhost'
const port = process.env.PORT || 5000;

// Middleware to parse JSON bodies
app.use(express.json());
app.use(cors()); // Enable CORS for all routes

// Connect to MongoDB
mongoose.connect(process.env.MONGO_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
.then(() => console.log('MongoDB connected'))
.catch(err => console.log(err));

// Serial communication for Arduino
const portName = 'COM4';  // Replace with the correct port where Arduino is connected
const arduinoPort = new SerialPort(portName, { baudRate: 9600 });
const parser = arduinoPort.pipe(new Readline({ delimiter: '\n' }));

arduinoPort.on('open', () => {
  console.log(`Serial Port ${portName} is opened`);
});

// Route to trigger LED blink
app.post('/blink-led', (req, res) => {
  
  const { blink } = req.body;  // Extract the blink value from the request body
  
  console.log(req.body);
  
  if (blink === '1') 
  {
    arduinoPort.write('1');  // Send signal to Arduino to turn LED on
    return res.send('LED is turned ON');
  } 
  else if (blink === '0') 
  {
    arduinoPort.write('0');  // Send signal to Arduino to turn LED off
    return res.send('LED is turned OFF');
  } 
  else 
  {
    return res.status(400).send('Invalid value for blink. Use "0" to turn off and "1" to turn on.');
  }
});

app.listen(port, () => {
  console.log(`Server running on http://${host}:${port}`);
});
```

4. **Arduino Code**:
   Upload this code to your Arduino board:
   ```cpp
   int ledPin = 13; // Pin where the LED is connected

   void setup() {
     Serial.begin(9600); // Set the baud rate to match the one in Node.js
     pinMode(ledPin, OUTPUT);
   }

   void loop() {

        // Check if data is available to read
        if (Serial.available() > 0) {

            // Read the incoming byte
            char command = Serial.read();
            
            if (command == '1') 
            {
                // Turn the LED on
                digitalWrite(ledPin, HIGH);
            }
            else if (command == '0')
            {
                // Turn the LED off
                digitalWrite(ledPin, LOW);
            }
        }
   }
   ```

### 2. Creating a Flutter Frontend Frontend


`.env`

```
API_BASE_URL=http://192.168.1.122:9000
```

`pubspec.yaml`

```
flutter:

  # The following line ensures that the Material Icons font is
  # included with your application, so that you can use the icons in
  # the material Icons class.
  uses-material-design: true

  # To add assets to your application, add an assets section, like this:
  assets:
    - .env
```



2. **Create a Switch Widget to Trigger LED Blink**:
   Inside `libs/screens/LedControlScreen.dart`, set up a Switch Widget to trigger the LED blink route:
```dart
import 'package:flutter/material.dart';

import 'package:http/http.dart' as http;

import 'package:flutter_dotenv/flutter_dotenv.dart';

class LedControlScreen extends StatefulWidget {
  const LedControlScreen({super.key});

  @override
  State<LedControlScreen> createState() => _LedControlScreenState();
}

class _LedControlScreenState extends State<LedControlScreen> {

  String BASE_URL = dotenv.env['API_BASE_URL'] ?? 'default_key';

  bool isSwitched = false; // Track the switch state

  // Function to send POST request to the backend
  Future<void> toggleLED(bool value) async {
    String blink = value ? '1' : '0';
    try {
      var response = await http.post(
        Uri.parse('${BASE_URL}/blink-led'), // Update with your backend URL
        headers: <String, String>{
          'Content-Type': 'application/json',
        },
        body: '{"blink": "$blink"}', // Sending blink value as JSON
      );

      if (response.statusCode == 200) {
        print(response.body);
      } else {
        print('Failed to toggle LED');
      }
    } catch (e) {
      print('Error: $e');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Arduino LED Controller'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              isSwitched ? 'LED is ON' : 'LED is OFF',
              style: TextStyle(fontSize: 24),
            ),
            Switch(
              value: isSwitched,
              onChanged: (value) {
                setState(() {
                  isSwitched = value;
                  toggleLED(isSwitched); // Toggle the LED on Arduino
                });
              },
              activeTrackColor: Colors.lightGreenAccent,
              activeColor: Colors.green,
            ),
          ],
        ),
      ),
    );
  }
}
```

`main.dart`

```dart
import 'package:flutter/material.dart';

import 'package:flutter_dotenv/flutter_dotenv.dart';

import 'package:flutter_hello_world/screens/LedControlScreen.dart';

void main() async {
  // Load the environment variables
  await dotenv.load(fileName: ".env");
  
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: true,
      home: LedControlScreen(),
    );
  }
}
```

### 4. Testing
- Open the React frontend, and when you click the **"Blink LED"** button, a request is sent to the Express backend, which in turn sends a command to the Arduino to blink the LED.

### Summary
- **MongoDB**: Connected using an environment variable.
- **Express**: Provides an endpoint for triggering LED blink.
- **React**: Frontend with a button to send requests.
- **Arduino**: Blinks an LED based on serial commands sent from Node.js.

Let me know if you need further clarification!

To test the modified `/blink-led` route using Postman, follow these steps:

### Step 1: Open Postman

Launch Postman on your computer.

### Step 2: Create a New Request

1. Click on the **New** button or the **+** tab to create a new request.
2. Select **HTTP Request**.

### Step 3: Set Up the Request

1. **Select the Request Type**: Change the request type from GET to POST by clicking the dropdown to the left of the URL field.
2. **Enter the URL**: Enter the URL for your server, including the endpoint. For example:
   ```
   http://localhost:5000/blink-led
   ```
   Adjust the port if you have configured a different one.

### Step 4: Set Up the Request Body

1. Click on the **Body** tab below the URL field.
2. Select the **raw** radio button.
3. In the dropdown that appears to the right of the **raw** option, select **JSON**.
4. Enter the JSON body to control the LED. For example, to turn the LED on, you would enter:
   ```json
   {
     "blink": "1"
   }
   ```

   To turn the LED off, you would enter:
   ```json
   {
     "blink": "0"
   }
   ```

### Step 5: Send the Request

1. Click the **Send** button to send the request to your server.
2. You should see a response in the response area below indicating whether the LED was turned on or off.

### Example Request for Turning On the LED

**Request Type**: POST  
**URL**: `http://localhost:5000/blink-led`  
**Body**:
```json
{
  "blink": "1"
}
```

**Expected Response**:
```
LED is turned ON
```

### Example Request for Turning Off the LED

**Request Type**: POST  
**URL**: `http://localhost:5000/blink-led`  
**Body**:
```json
{
  "blink": "0"
}
```

**Expected Response**:
```
LED is turned OFF
```

### Error Response Example

If you send an invalid value, such as:
```json
{
  "blink": "2"
}
```

You should receive a response like:
```
Invalid value for blink. Use "0" to turn off and "1" to turn on.
```

By following these steps, you should be able to test your Express server's LED control functionality using Postman effectively!