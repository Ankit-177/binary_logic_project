---
toc: true
comments: false
layout: post
title: Binary Calculator
description: This the the binary calculator.
type: plans
courses: { compsci: {week: 0} }
---

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 20px;
    }
    input {
      width: 150px;
      padding: 10px;
      margin: 10px;
      text-align: right;
    }
    button {
  padding: 0.6em 0em;
  border: 2px solid rgb(255, 0, 0); /* Initial RGB border (Red) */
  animation: breathing-border 3s infinite alternate;
  background: #111;
  cursor: pointer;
  position: relative;
  z-index: 0;
  border-radius: 10px;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}
  @keyframes breathing-border {
    0% {
      border-color: rgb(255, 0, 0); /* Red */
    }
    50% {
      border-color: rgb(0, 255, 0); /* Green */
    }
    100% {
      border-color: rgb(0, 0, 255); /* Blue */
    }
  }
    .reset-button {
      animation: bounce 1s ease;
    }
    /* Dark mode styles */
    .dark-mode body {
      background-color: #222;
      color: #fff;
    }
    .dark-mode input,
    .dark-mode button {
      background-color: #333;
      color: #fff;
    }
  </style>
</head>
<body>

  <input type="text" id="decimalInput1" placeholder="Decimal Number 1" oninput="validateDecimalInput(this)">
  <input type="text" id="decimalInput2" placeholder="Decimal Number 2" oninput="validateDecimalInput(this)">

  <br>

  <input type="text" id="binaryInput1" placeholder="Binary Number 1" oninput="validateBinaryInput(this)">
  <input type="text" id="binaryInput2" placeholder="Binary Number 2" oninput="validateBinaryInput(this)">

  <br>

  <button onclick="calculate('+')">+</button>
  <button onclick="calculate('-')">-</button>
  <button onclick="calculate('*')">*</button>
  <button onclick="calculate('/')">/</button>

  <br>

  <div id="result">Result: </div>
  <div id="decimalValues">Decimal Values: </div>
  <div id="colorBox"></div>

  <!-- Animation for reset button -->
  <script>
    function resetAnimation() {
      const resetButton = document.querySelector('.reset-button');
      resetButton.classList.remove('reset-button');
      void resetButton.offsetWidth; // Trigger reflow
      resetButton.classList.add('reset-button');
    }
  </script>

  <!-- Dark Mode Toggle Button -->
  <button onclick="toggleDarkMode()">Dark Mode</button>

  <!-- Reset Button -->
  <button onclick="resetCalculator(); resetAnimation();" class="reset-button">Reset</button>

  <script>
    function validateDecimalInput(input) {
      input.value = input.value.replace(/[^\d.]/g, '').replace(/(\..*)\./g, '$1');
    }

    function validateBinaryInput(input) {
      input.value = input.value.replace(/[^01]/g, '');
    }

    function validateInput(input) {
      input.value = input.value.replace(/[^01.]/g, '').replace(/(\..*)\./g, '$1');
    }

    function calculate(operator) {
      const decimalInput1 = document.getElementById('decimalInput1').value;
      const decimalInput2 = document.getElementById('decimalInput2').value;
      const binaryInput1 = document.getElementById('binaryInput1').value;
      const binaryInput2 = document.getElementById('binaryInput2').value;

      if (decimalInput1 !== '' && decimalInput2 !== '') {
        document.getElementById('binaryInput1').value = decimalToBinary(decimalInput1);
        document.getElementById('binaryInput2').value = decimalToBinary(decimalInput2);
      }

      const binaryInput1Value = document.getElementById('binaryInput1').value;
      const binaryInput2Value = document.getElementById('binaryInput2').value;

      if (!isValidBinary(binaryInput1Value) || !isValidBinary(binaryInput2Value)) {
        alert('Please enter valid binary numbers.');
        return;
      }

      const decimal1 = binaryToDecimal(binaryInput1);
      const decimal2 = binaryToDecimal(binaryInput2);

      // BREAK HERE TO PYTHON FILE

      let result;
      switch (operator) {
        case '+':
          result = decimalToBinary(decimal1 + decimal2);
          break;
        case '-':
          result = decimalToBinary(decimal1 - decimal2);
          break;
        case '*':
          result = decimalToBinary(decimal1 * decimal2);
          break;
        case '/':
          if (decimal2 !== 0) {
            result = decimalToBinary(Math.floor(decimal1 / decimal2));
          } else {
            alert('Division by zero is not allowed.');
            return;
          }
          break;
        default:
          alert('Invalid operator.');
          return;
      }

      const resultDecimal = binaryToDecimal(result);

      document.getElementById('result').textContent = 'Result: ' + result + ' (Decimal: ' + resultDecimal + ')';
      document.getElementById('decimalValues').textContent = 'Decimal Values: ' + decimal1 + ', ' + decimal2 + ', ' + resultDecimal;

      const red = decimalToBinary(decimal1 % 256);
      const green = decimalToBinary(decimal2 % 256);
      const blue = decimalToBinary(resultDecimal % 256);
      const rgbColor = `rgb(${binaryToDecimal(red)}, ${binaryToDecimal(green)}, ${binaryToDecimal(blue)})`;

      document.getElementById('colorBox').style.backgroundColor = rgbColor;
    }

    function isValidBinary(value) {
      const binaryRegex = /^[01]+$/;
      return binaryRegex.test(value);
    }

    function binaryToDecimal(binary) {
      return parseInt(binary, 2);
    }

    function decimalToBinary(decimal) {
      return (decimal >>> 0).toString(2);
    }

    function toggleDarkMode() {
      const body = document.body;
      body.classList.toggle('dark-mode');
      const darkModeToggle = document.getElementById('darkModeToggle');
      const isDarkMode = body.classList.contains('dark-mode');
      darkModeToggle.style.backgroundColor = isDarkMode ? '#008000' : '#111';
    }

    function resetCalculator() {
      document.getElementById('decimalInput1').value = '';
      document.getElementById('decimalInput2').value = '';
      document.getElementById('binaryInput1').value = '';
      document.getElementById('binaryInput2').value = '';
      document.getElementById('result').textContent = 'Result: ';
      document.getElementById('decimalValues').textContent = 'Decimal Values: ';
      document.getElementById('colorBox').style.backgroundColor = '';
    }
  </script>

</body>
