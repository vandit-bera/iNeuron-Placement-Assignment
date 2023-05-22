# CALCULATOR APP USING REACT

```
Open the src/App.js file and replace its contents with the following code:


import React, { useState } from 'react';
import './App.css';

const App = () => {
  const [displayValue, setDisplayValue] = useState('0');
  const [firstOperand, setFirstOperand] = useState(null);
  const [operator, setOperator] = useState(null);
  const [waitingForSecondOperand, setWaitingForSecondOperand] = useState(false);

  const handleDigitClick = (digit) => {
    if (waitingForSecondOperand) {
      setDisplayValue(digit);
      setWaitingForSecondOperand(false);
    } else {
      setDisplayValue(displayValue === '0' ? digit : displayValue + digit);
    }
  };

  const handleDecimalClick = () => {
    if (!displayValue.includes('.')) {
      setDisplayValue(displayValue + '.');
    }
  };

  const handleOperatorClick = (op) => {
    if (operator && waitingForSecondOperand) {
      setOperator(op);
      return;
    }

    const inputValue = parseFloat(displayValue);

    if (firstOperand === null) {
      setFirstOperand(inputValue);
    } else if (operator) {
      const result = performCalculation();
      setDisplayValue(result);
      setFirstOperand(result);
    }

    setOperator(op);
    setWaitingForSecondOperand(true);
  };

  const performCalculation = () => {
    const operand1 = firstOperand;
    const operand2 = parseFloat(displayValue);

    if (isNaN(operand1) || isNaN(operand2)) return null;

    switch (operator) {
      case '+':
        return operand1 + operand2;
      case '-':
        return operand1 - operand2;
      case '*':
        return operand1 * operand2;
      case '/':
        return operand1 / operand2;
      default:
        return null;
    }
  };

  const handleEqualClick = () => {
    const result = performCalculation();
    if (result !== null) {
      setDisplayValue(result);
      setFirstOperand(result);
      setOperator(null);
      setWaitingForSecondOperand(false);
    }
  };

  const handleClearClick = () => {
    setDisplayValue('0');
    setFirstOperand(null);
    setOperator(null);
    setWaitingForSecondOperand(false);
  };

  return (
    <div className="calculator">
      <div className="display">{displayValue}</div>
      <div className="keypad">
        <div className="row">
          <button onClick={() => handleDigitClick('7')}>7</button>
          <button onClick={() => handleDigitClick('8')}>8</button>
          <button onClick={() => handleDigitClick('9')}>9</button>
          <button onClick={() => handleOperatorClick('/')}>/</button>
        </div>
        <div className="row">
          <button onClick={() => handleDigitClick('4')}>4</button>
          <button onClick={() => handleDigitClick('5')}>5</button>
          <button onClick={() => handleDigitClick('6')}>6</button>
          <button onClick={() => handleOperatorClick('*')}>*</button>
        </div>
        <div className="row">
          <button onClick={() => handleDigitClick('1')}>1</button>
          <button onClick={() => handleDigitClick('2')}>2</button>
          <button onClick={() => handleDigitClick('3')}>3</button>
          <button onClick={() => handleOperatorClick('-')}>-</button>
        </div>
        <div className="row">
          <button onClick={() => handleDigitClick('0')}>0</button>
          <button onClick={handleDecimalClick}>.</button>
          <button onClick={handleEqualClick}>=</button>
          <button onClick={() => handleOperatorClick('+')}>+</button>
        </div>
        <div className="row">
          <button onClick={handleClearClick}>Clear</button>
        </div>
      </div>
    </div>
  );
};

export default App;

```

```
Create a new file called src/App.css and add the following CSS code to style the calculator:


.calculator {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.display {
  font-size: 2rem;
  font-weight: bold;
  padding: 1rem;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  width: 200px;
  text-align: right;
}

.keypad {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
}

button {
  padding: 1rem;
  font-size: 1.2rem;
  width: 100%;
  background-color: #fff;
  border: 1px solid #ccc;
  cursor: pointer;
}

button:hover {
  background-color: #f0f0f0;
}

.row:last-child {
  grid-column: span 4;
}

```