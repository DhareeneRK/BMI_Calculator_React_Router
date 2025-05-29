# Ex06 BMI Calculator
## Date: 27-05-2025
## Name: Dhareene R K
## Reg No: 212222040035

## AIM
To develop a responsive and interactive Body Mass Index (BMI) Calculator using React that allows users to input their height and weight, and calculates their BMI to categorize their health status (e.g., Underweight, Normal, Overweight, Obese).

## DESIGN STEPS

### STEP 1: Initialize React Project

<li>Create a new React app using create-react-app.</li>
<li>Install React Router using:</li>
npm install react-router-dom

### STEP 2: Set Up Routing

Create routing structure with react-router-dom:

<li>Home route (/) – Intro or Navigation</li>

<li>BMI Calculator route (/bmi)</li>

<li>Result route (/result)</li>

### STEP 3: Design the BMI Form Page

<li>Create a form to accept Height (in cm or m) and Weight (in kg).</li>

<li>On form submit, navigate to the result page with entered values via URL query params or context/state.</li>

## STEP 4: Handle Input Validation

<li>Check if height and weight are valid numbers.</li>

<li>Optionally, show error messages for invalid inputs.</li>

### STEP 5: Perform BMI Calculation

<li>In the result component:

<li>Extract height and weight from the route (URL or passed state).</li>

<li>Apply the BMI formula:</li>

![image](https://github.com/user-attachments/assets/ec785506-c96b-489e-8783-fb1a5d36101a)
​
 
<li>Convert height from cm to m if needed.</li></li>

### STEP 6: Display Result

<li>Show calculated BMI.</li>

<li>Show category based on BMI range:

<li>Underweight, Normal, Overweight, Obese, etc.</li></li>

### STEP 7: Navigation Options

<li>Provide a button to go back to the BMI form to calculate again.</li>

### STEP 8: Enhancements

<li>Add styling using CSS or Tailwind.</li>

## PROGRAM
#  App.js
```
import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Home from './Home';
import BMICalculator from './BMICalculator';
import Result from './Result';
import './App.css';

function App() {
  return (
    <Router>
      <div className="app">
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/bmi" element={<BMICalculator />} />
          <Route path="/result" element={<Result />} />
        </Routes>

        {/* Footer added below */}
        <footer className="footer">
          Dhareene R K (212222040035)
        </footer>
      </div>
    </Router>
  );
}

export default App;

```
# App.css
```
body {
  margin: 0;
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(to right, #ffecd2, #fcb69f);
  color: #222;
  min-height: 100vh;
}

.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.container {
  background: #fff;
  padding: 30px;
  border-radius: 16px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  max-width: 400px;
  width: 90%;
  margin: 50px auto;
  text-align: center;
  transition: transform 0.3s;
}

.container:hover {
  transform: scale(1.02);
}

input[type="number"] {
  width: 100%;
  padding: 12px;
  margin: 12px 0;
  border: none;
  border-radius: 10px;
  background-color: #fcefee;
  font-size: 16px;
}

input[type="number"]:focus {
  outline: none;
  background-color: #fff0f5;
  box-shadow: 0 0 5px #ff9a9e;
}

button {
  background: linear-gradient(to right, #ff758c, #ff7eb3);
  color: white;
  border: none;
  padding: 14px 20px;
  margin-top: 15px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 16px;
  width: 100%;
  transition: background 0.3s;
}

button:hover {
  background: linear-gradient(to right, #ff6a95, #f94d7d);
}

h2 {
  margin-bottom: 20px;
  color: #ff5e78;
}

.result {
  font-size: 20px;
  font-weight: bold;
  margin-top: 20px;
  color: #444;
}

.footer {
  background: #222;
  color: white;
  text-align: center;
  padding: 15px;
  margin-top: auto;
  font-size: 14px;
}

```
# Home.js
```
import React from 'react';
import { Link } from 'react-router-dom';

function Home() {
  return (
    <div className="container">
      <h2>BMI Calculator</h2>
      <Link to="/bmi">
        <button>Start Calculating</button>
      </Link>
    </div>
  );
}

export default Home;

```

# BMICalculator.js
```
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';

function BMICalculator() {
  const [height, setHeight] = useState('');
  const [weight, setWeight] = useState('');
  const navigate = useNavigate();

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!height || !weight || height <= 0 || weight <= 0) {
      alert("Enter valid height and weight");
      return;
    }
    navigate(`/result?height=${height}&weight=${weight}`);
  };

  return (
    <div className="container">
      <h2>Enter Your Details</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="number"
          placeholder="Height (cm)"
          value={height}
          onChange={(e) => setHeight(e.target.value)}
        />
        <input
          type="number"
          placeholder="Weight (kg)"
          value={weight}
          onChange={(e) => setWeight(e.target.value)}
        />
        <button type="submit">Calculate BMI</button>
      </form>
    </div>
  );
}

export default BMICalculator;

```

# Result.js
```
import React from 'react';
import { useLocation, useNavigate } from 'react-router-dom';

function Result() {
  const query = new URLSearchParams(useLocation().search);
  const height = parseFloat(query.get('height'));
  const weight = parseFloat(query.get('weight'));
  const navigate = useNavigate();

  if (!height || !weight) {
    return <div className="container">Invalid input</div>;
  }

  const heightInMeters = height / 100;
  const bmi = (weight / (heightInMeters * heightInMeters)).toFixed(2);

  let category = '';
  if (bmi < 18.5) category = 'Underweight';
  else if (bmi < 24.9) category = 'Normal';
  else if (bmi < 29.9) category = 'Overweight';
  else category = 'Obese';

  return (
    <div className="container">
      <h2>Your BMI Result</h2>
      <div className="result">BMI: {bmi}</div>
      <div className="result">Category: {category}</div>
      <button onClick={() => navigate('/bmi')}>Calculate Again</button>
    </div>
  );
}

export default Result;

```
## OUTPUT
![web6 1](https://github.com/user-attachments/assets/68997e63-1ad4-4e5b-b46b-9a4e026e935d)
![web6 2](https://github.com/user-attachments/assets/d8cdf303-aba3-447e-a050-d40dc2135350)
![web6 3](https://github.com/user-attachments/assets/4dc6300a-b820-4883-8ade-21ec6a2b37c7)
![web6 4](https://github.com/user-attachments/assets/70ed579c-fda5-484f-8f85-71af01f005e2)




## RESULT
The BMI Calculator successfully takes user input for height and weight, performs the BMI calculation in real-time using React state and event handling, and displays the BMI value along with the corresponding health category.
