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
  <title>Binary Calculator</title>
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
      width: 50px;
      height: 50px;
      margin: 5px;
      font-size: 18px;
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
  </style>
</head>
<body>

  <h2>Binary Calculator</h2>

  <input type="text" id="binaryInput1" placeholder="Binary Number 1" oninput="validateInput(this)">
  <input type="text" id="binaryInput2" placeholder="Binary Number 2" oninput="validateInput(this)">

  <br>

  <button onclick="calculate('+')">+</button>
  <button onclick="calculate('-')">-</button>
  <button onclick="calculate('*')">*</button>
  <button onclick="calculate('/')">/</button>

  <br>

  <div id="result">Result: </div>

  <div id="decimalValues">Decimal Values: </div>

  <div id="colorBox"></div>

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
  </script>

</body>


