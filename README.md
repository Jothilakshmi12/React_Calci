# Ex04 Simple Calculator - React Project
## NAME: JOTHILAKSHMI PALANI
## REG NO: 212223110017
## Date: 18/03/2026

## AIM
To  develop a Simple Calculator using React.js with clean and responsive design, ensuring a smooth user experience across different screen sizes.

## ALGORITHM
### STEP 1
Create a React App.

### STEP 2
Open a terminal and run:
  <ul><li>npx create-react-app simple-calculator</li>
  <li>cd simple-calculator</li>
  <li>npm start</li></ul>

### STEP 3
Inside the src/ folder, create a new file Calculator.js and define the basic structure.

### STEP 4
Plan the UI: Display screen, number buttons (0-9), operators (+, -, *, /), clear (C), and equal (=).

### STEP 5
Create a new file Calculator.css in src/ and add the styling.

### STEP 6
Open src/App.js and modify it.

### STEP 7
Start the development server.
  npm start

### STEP 8
Open http://localhost:3000/ in the browser.

### STEP 9
Test the calculator by entering numbers and operations.

### STEP 10
Fix styling issues and refine content placement.

### STEP 11
Deploy the website.

### STEP 12
Upload to GitHub Pages for free hosting.

## PROGRAM
```java
import React, { useState, useEffect } from 'react';
import * as math from 'mathjs';
import { History as HistoryIcon } from 'lucide-react';
import Display from './components/Display';
import Keypad from './components/Keypad';
import HistorySidebar from './components/HistorySidebar';
import './App.css';

function App() {
  const [expression, setExpression] = useState('');
  const [result, setResult] = useState('0');
  const [history, setHistory] = useState([]);
  const [isSidebarOpen, setIsSidebarOpen] = useState(false);
  const [isScientific, setIsScientific] = useState(false);
  const [isError, setIsError] = useState(false);

  // Load history on mount
  useEffect(() => {
    const saved = localStorage.getItem('calculator_history');
    if (saved) {
      setHistory(JSON.parse(saved));
    }
  }, []);

  // Save history on change
  useEffect(() => {
    localStorage.setItem('calculator_history', JSON.stringify(history));
  }, [history]);

  const calculateResult = (expr) => {
    try {
      // Replace some display operators with mathjs valid ones
      const evalExpr = expr
        .replace(/×/g, '*')
        .replace(/÷/g, '/')
        .replace(/π/g, 'pi');
      
      const res = math.evaluate(evalExpr);
      // Format to avoid super long decimals
      const formatted = math.format(res, { precision: 14 });
      setResult(String(formatted));
      setIsError(false);
      return String(formatted);
    } catch (e) {
      setResult('Error');
      setIsError(true);
      return null;
    }
  };

  const handleDigit = (digit) => {
    setIsError(false);
    if (result === 'Error') setResult('0');
    
    // Auto clear if starting new calculation after a result via equals
    // We would need an isCalculated flag, but let's just append for now
    setExpression((prev) => prev + digit);
  };

  const handleOperator = (op) => {
    setIsError(false);
    if (!expression && result && result !== '0' && result !== 'Error') {
      // Continue from previous result
      setExpression(result + op);
    } else {
      setExpression((prev) => prev + op);
    }
  };

  const handleAction = (action) => {
    setIsError(false);
    
    if (action === 'clear') {
      setExpression('');
      setResult('0');
    } else if (action === 'delete') {
      setExpression((prev) => prev.slice(0, -1));
    } else if (action === 'percentage') {
      if (expression) {
        setExpression((prev) => prev + '%');
      } else if (result !== '0') {
        const val = math.evaluate(result + '/100');
        setResult(String(val));
      }
    } else {
      // Scientific functions: sin, cos, sqrt, etc.
      setExpression((prev) => prev + action + '(');
    }
  };

  const handleEquals = () => {
    if (!expression) return;
    const finalResult = calculateResult(expression);
    
    if (finalResult && finalResult !== 'Error') {
      const newHistory = [{ expression, result: finalResult }, ...history].slice(0, 50); // Keep last 50
      setHistory(newHistory);
      setExpression('');
      // Result is already set in calculateResult
    }
  };

  return (
    <div className="app-container">
      <div className="calculator-wrapper glass">
        <div className="header">
          <h1>Calculator</h1>
          <button 
            className="history-toggle" 
            onClick={() => setIsSidebarOpen(true)}
            aria-label="Open History"
          >
            <HistoryIcon size={20} />
          </button>
        </div>

        <Display 
          expression={expression} 
          result={result} 
          isError={isError} 
        />

        <Keypad 
          onDigit={handleDigit}
          onOperator={handleOperator}
          onAction={handleAction}
          onEquals={handleEquals}
          isScientific={isScientific}
        />

        <HistorySidebar 
          history={history}
          isOpen={isSidebarOpen}
          onClose={() => setIsSidebarOpen(false)}
          onSelectHistory={(item) => {
            setExpression(item.expression);
            setResult(item.result);
            setIsSidebarOpen(false);
          }}
          onClearHistory={() => setHistory([])}
          isScientific={isScientific}
          onToggleScientific={() => setIsScientific(!isScientific)}
        />
      </div>
    </div>
  );
}

export default App;
```

## OUTPUT

<img width="1062" height="812" alt="image" src="https://github.com/user-attachments/assets/4452e477-279f-4e2b-8b41-5cfcba73d229" />

<img width="1321" height="841" alt="image" src="https://github.com/user-attachments/assets/5695182f-74fd-47d5-b1ad-da08a7de68a9" />


## RESULT
The program for developing a simple calculator in React.js is executed successfully.
