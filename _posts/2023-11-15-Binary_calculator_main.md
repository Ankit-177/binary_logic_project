---
toc: true
comments: false
layout: post
title: Binary Calculator
description: This the the binary calculator.
type: plans
courses: { compsci: {week: 0} }
---

<!DOCTYPE html>
<html lang="en">
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
      padding: 0.6em 2em;
      border: none;
      outline: none;
      color: rgb(255, 255, 255);
      background: #111;
      cursor: pointer;
      position: relative;
      z-index: 0;
      border-radius: 10px;
      user-select: none;
      -webkit-user-select: none;
      touch-action: manipulation;
    }
    .button:before {
      content: "";
      background: linear-gradient(
        45deg,
        #ff0000,
        #ff7300,
        #fffb00,
        #48ff00,
        #00ffd5,
        #002bff,
        #7a00ff,
        #ff00c8,
        #ff0000
      );
      position: absolute;
      top: -2px;
      left: -2px;
      background-size: 400%;
      z-index: -1;
      filter: blur(5px);
      -webkit-filter: blur(5px);
      width: calc(100% + 4px);
      height: calc(100% + 4px);
      animation: glowing-button 20s linear infinite;
      transition: opacity 0.3s ease-in-out;
      border-radius: 10px;
    }
    @keyframes glowing-button {
      0% {
        background-position: 0 0;
      }
      50% {
        background-position: 400% 0;
      }
      100% {
        background-position: 0 0;
      }
    }
    .button:after {
      z-index: -1;
      content: "";
      position: absolute;
      width: 100%;
      height: 100%;
      background: #222;
      left: 0;
      top: 0;
      border-radius: 10px;
    }
    #result {
      font-size: 24px;
      margin-top: 10px;
    }
    #decimalValues {
      margin-top: 10px;
    }
    #colorBox {
      width: 100px;
      height: 100px;
      margin: 20px auto;
      border: 2px solid #000;
    }
    /* Animation for reset button */
    @keyframes bounce {
      0%, 20%, 50%, 80%, 100% {
        transform: translateY(0);
      }
      40% {
        transform: translateY(-15px);
      }
      60% {
        transform: translateY(-10px);
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
  <button onclick="toggleDarkMode()" id="darkModeToggle" class="button">T</button>

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
</html>
