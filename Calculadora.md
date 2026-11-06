<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Simple Calculator</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(135deg, #f0f4f8, #d9e2ec);
    }

    .calculator {
      width: 320px;
      background: #1f2937;
      border-radius: 18px;
      padding: 20px;
      box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
    }

    .display {
      width: 100%;
      height: 70px;
      background: #111827;
      color: white;
      border: none;
      border-radius: 12px;
      font-size: 2rem;
      text-align: right;
      padding: 10px 14px;
      margin-bottom: 20px;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
    }

    button {
      border: none;
      border-radius: 12px;
      padding: 18px 0;
      font-size: 1.2rem;
      cursor: pointer;
      transition: 0.2s ease;
    }

    button:hover {
      opacity: 0.9;
      transform: translateY(-1px);
    }

    .operator {
      background: #f59e0b;
      color: white;
    }

    .number {
      background: #e5e7eb;
      color: #111827;
    }

    .clear {
      background: #ef4444;
      color: white;
    }

    .equals {
      background: #10b981;
      color: white;
      grid-column: span 2;
    }
  </style>
</head>
<body>
  <div class="calculator">
    <input class="display" id="display" type="text" disabled />

    <div class="buttons">
      <button class="clear" onclick="clearDisplay()">C</button>
      <button class="operator" onclick="appendOperator('/')">/</button>
      <button class="operator" onclick="appendOperator('*')">×</button>
      <button class="operator" onclick="deleteLast()">⌫</button>

      <button class="number" onclick="appendNumber('7')">7</button>
      <button class="number" onclick="appendNumber('8')">8</button>
      <button class="number" onclick="appendNumber('9')">9</button>
      <button class="operator" onclick="appendOperator('-')">−</button>

      <button class="number" onclick="appendNumber('4')">4</button>
      <button class="number" onclick="appendNumber('5')">5</button>
      <button class="number" onclick="appendNumber('6')">6</button>
      <button class="operator" onclick="appendOperator('+')">+</button>

      <button class="number" onclick="appendNumber('1')">1</button>
      <button class="number" onclick="appendNumber('2')">2</button>
      <button class="number" onclick="appendNumber('3')">3</button>
      <button class="equals" onclick="calculateResult()">=</button>

      <button class="number" onclick="appendNumber('0')" style="grid-column: span 2;">0</button>
      <button class="number" onclick="appendDecimal()">.</button>
    </div>
  </div>

  <script>
    const display = document.getElementById('display');
    let currentValue = '';
    let storedValue = null;
    let operator = null;
    let shouldReset = false;

    function updateDisplay() {
      display.value = currentValue || '0';
    }

    function appendNumber(value) {
      if (shouldReset) {
        currentValue = '';
        shouldReset = false;
      }

      if (currentValue === '0' && value === '0') return;
      if (currentValue === '0' && value !== '0') {
        currentValue = value;
      } else {
        currentValue += value;
      }

      updateDisplay();
    }

    function appendDecimal() {
      if (shouldReset) {
        currentValue = '0';
        shouldReset = false;
      }

      if (!currentValue.includes('.')) {
        currentValue += currentValue ? '.' : '0.';
      }

      updateDisplay();
    }

    function appendOperator(nextOperator) {
      if (currentValue === '') return;

      if (storedValue !== null && operator !== null && !shouldReset) {
        const result = operate(storedValue, parseFloat(currentValue), operator);
        currentValue = String(result);
        storedValue = result;
      } else {
        storedValue = parseFloat(currentValue);
      }

      operator = nextOperator;
      shouldReset = true;
      updateDisplay();
    }

    function calculateResult() {
      if (storedValue === null || operator === null || currentValue === '') return;

      const result = operate(storedValue, parseFloat(currentValue), operator);
      currentValue = String(result);
      storedValue = null;
      operator = null;
      shouldReset = false;
      updateDisplay();
    }

    function deleteLast() {
      if (currentValue.length <= 1) {
        currentValue = '';
      } else {
        currentValue = currentValue.slice(0, -1);
      }
      updateDisplay();
    }

    function clearDisplay() {
      currentValue = '';
      storedValue = null;
      operator = null;
      shouldReset = false;
      updateDisplay();
    }

    function operate(a, b, op) {
      switch (op) {
        case '+':
          return a + b;
        case '-':
          return a - b;
        case '*':
          return a * b;
        case '/':
          return b === 0 ? 'Error' : a / b;
        default:
          return b;
      }
    }

    updateDisplay();
  </script>
</body>
</html>
