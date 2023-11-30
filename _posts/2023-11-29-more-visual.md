---
toc: true
comments: false
layout: post
title: Binary calculator with more visual appealing
description: binary calculatore revised ver. 
type: plans
courses: { compsci: {week: 0} }
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Binary Calculator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 20px;
      background-color: #f5f5f5; /* Set a light background color */
    }

    input {
      width: 150px;
      padding: 10px;
      margin: 10px;
      text-align: right;
    }

    button {
      width: 50px;
      height: 50px;
      margin: 5px;
      font-size: 18px;
      border: none;
      border-radius: 10px; /* Add border-radius for a rounded look */
      background-color: #4caf50; /* Green color for the buttons */
      color: #fff; /* White text color */
      cursor: pointer;
      transition: background-color 0.3s ease, transform 0.2s ease, box-shadow 0.3s ease; /* Add smooth transitions */
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Add a subtle shadow effect */
    }

    button:hover {
      background-color: #45a049; /* Darker green color on hover */
      transform: scale(1.05); /* Scale up on hover */
      box-shadow: 0 8px 12px rgba(0, 0, 0, 0.2); /* Increase shadow on hover */
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

    /* Dark Mode Styles */
    body.dark-mode {
      background-color: #1a1a1a;
      color: #ffffff;
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

  </style>
</head>
<body>

  <h2>Binary Calculator</h2>

  <input type="text" id="binaryInput1" placeholder="Binary Number 1" oninput="validateInput(this)">
  <input type="text" id="binaryInput2" placeholder="Binary Number 2" oninput="validateInput(this)">

  <br>

  <button onclick="calculate('+'); playClickSound()">+</button>
  <button onclick="calculate('-'); playClickSound()">-</button>
  <button onclick="calculate('*'); playClickSound()">*</button>
  <button onclick="calculate('/'); playClickSound()">/</button>

  <br>

  <div id="result">Result: </div>

  <div id="decimalValues">Decimal Values: </div>

  <div id="colorBox"></div>

  <!-- Dark Mode Toggle Button -->
  <button onclick="toggleDarkMode()">Toggle Dark Mode</button>

  <!-- Reset Button -->
  <button onclick="resetCalculator()" class="reset-button">Reset</button>

  <script>
    function validateInput(input) {
      input.value = input.value.replace(/[^01]/g, '');
    }

    function calculate(operator) {
      const binaryInput1 = document.getElementById('binaryInput1').value;
      const binaryInput2 = document.getElementById('binaryInput2').value;

      if (!isValidBinary(binaryInput1) || !isValidBinary(binaryInput2)) {
        alert('Please enter valid binary numbers.');
        return;
      }

      // BREAK HERE AND REDIRECT TO LOGIC PYTHON FILE
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

      // Play sound for calculation completion
      playCalculationSound();
    }

    function resetCalculator() {
      document.getElementById('binaryInput1').value = '';
      document.getElementById('binaryInput2').value = '';
      document.getElementById('result').textContent = 'Result: ';
      document.getElementById('decimalValues').textContent = 'Decimal Values: ';
      document.getElementById('colorBox').style.backgroundColor = ''; // Reset color box

      // Play sound for reset
      playResetSound();
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
    }

    function playClickSound() {
      // Load and play the sound for button clicks
      const clickSound = new Audio('path-to-click-sound.mp3'); // Replace with the actual path
      clickSound.play();
    }

    function playCalculationSound() {
      // Load and play the sound for calculation completion
      const calculationSound = new Audio('path-to-calculation-sound.mp3'); // Replace with the actual path
      calculationSound.play();
    }

    function playResetSound() {
      // Load and play the sound for reset
      const resetSound = new Audio('path-to-reset-sound.mp3'); // Replace with the actual path
      resetSound.play();
    }

  </script>

</body>
</html>

