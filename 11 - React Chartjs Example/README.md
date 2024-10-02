Here’s an example of how to integrate Chart.js into a React application using the `react-chartjs-2` library, which is a wrapper for Chart.js in React.

### Step 1: Install the required libraries
You’ll need to install both `react-chartjs-2` and `chart.js`:

```bash
npm install react-chartjs-2 chart.js
```

### Step 2: Create a Chart Component

Now you can create a simple chart component in your React app. This example will use a `Bar` chart, but you can use other chart types like `Line`, `Pie`, etc.

```jsx
// BarChart.js
import React from 'react';
import { Bar } from 'react-chartjs-2';
import { Chart as ChartJS, CategoryScale, LinearScale, BarElement, Title, Tooltip, Legend } from 'chart.js';

// Register the required components for the chart
ChartJS.register(CategoryScale, LinearScale, BarElement, Title, Tooltip, Legend);

const BarChart = () => {
  const data = {
    labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
    datasets: [
      {
        label: 'Votes',
        data: [12, 19, 3, 5, 2, 3],
        backgroundColor: [
          'rgba(255, 99, 132, 0.2)',
          'rgba(54, 162, 235, 0.2)',
          'rgba(255, 206, 86, 0.2)',
          'rgba(75, 192, 192, 0.2)',
          'rgba(153, 102, 255, 0.2)',
          'rgba(255, 159, 64, 0.2)',
        ],
        borderColor: [
          'rgba(255, 99, 132, 1)',
          'rgba(54, 162, 235, 1)',
          'rgba(255, 206, 86, 1)',
          'rgba(75, 192, 192, 1)',
          'rgba(153, 102, 255, 1)',
          'rgba(255, 159, 64, 1)',
        ],
        borderWidth: 1,
      },
    ],
  };

  const options = {
    scales: {
      y: {
        beginAtZero: true,
      },
    },
  };

  return <Bar data={data} options={options} />;
};

export default BarChart;
```

### Step 3: Use the Chart Component

Now, import and use the `BarChart` component in your main application.

```jsx
// App.js
import React from 'react';
import BarChart from './BarChart';

function App() {
  return (
    <div className="App">
      <h2>React Chart.js Example</h2>
      <BarChart />
    </div>
  );
}

export default App;
```

### Step 4: Run your app

Start the app using:

```bash
npm start
```

You should see a bar chart displaying the votes for different colors.

### Customizing the Chart
You can customize the chart by changing `type`, `data`, and `options` based on the requirements. Other chart types you can try include `Line`, `Pie`, `Doughnut`, `Radar`, etc., by importing them from `react-chartjs-2`.