---
layout: page
permalink: /calculator/
toc: true
comments: false
---

<!-- 
Updated calculator design with improved CSS for a modern look.
-->

<!-- 
HTML implementation of the calculator.
-->

<!-- 
    Style and Action are aligned with HTML class definitions.
    style.css contains the majority of style definitions (number, operation, clear, and equals)
    - The div calculator container sets 4 elements to a row.
    The background is credited to Vanta JS and is implemented at the bottom of this page.
-->
<style>
  body {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background: #f0f0f0;
    margin: 0;
  }
  #animation {
    position: relative;
    width: 100%;
    height: 100%;
  }
  .calculator-container {
    display: grid;
    grid-template-columns: repeat(4, 80px);
    grid-gap: 10px;
    background: #333;
    border-radius: 15px;
    padding: 15px;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .calculator-output {
    grid-column: span 4;
    background: #222;
    color: #fff;
    border-radius: 10px;
    padding: 15px;
    font-size: 24px;
    text-align: right;
    border: 2px solid #444;
  }
  .calculator-number, .calculator-operation, .calculator-clear, .calculator-equals {
    background: #444;
    color: #fff;
    border-radius: 8px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;
    cursor: pointer;
    transition: background 0.3s;
  }
  .calculator-number:hover, .calculator-operation:hover, .calculator-clear:hover, .calculator-equals:hover {
    background: #555;
  }
  .calculator-clear {
    grid-column: span 2;
    background: #e74c3c;
  }
  .calculator-equals {
    grid-column: span 2;
    background: #2ecc71;
  }
</style>

<!-- Add a container for the animation -->
<div id="animation">
  <div class="calculator-container">
    <!-- result -->
    <div class="calculator-output" id="output">0</div>
    <!-- row 1 -->
    <div class="calculator-number">1</div>
    <div class="calculator-number">2</div>
    <div class="calculator-number">3</div>
    <div class="calculator-operation">+</div>
    <!-- row 2 -->
    <div class="calculator-number">4</div>
    <div class="calculator-number">5</div>
    <div class="calculator-number">6</div>
    <div class="calculator-operation">-</div>
    <!-- row 3 -->
    <div class="calculator-number">7</div>
    <div class="calculator-number">8</div>
    <div class="calculator-number">9</div>
    <div class="calculator-operation">*</div>
    <!-- row 4 -->
    <div class="calculator-clear">A/C</div>
    <div class="calculator-number">0</div>
    <div class="calculator-number">.</div>
    <div class="calculator-equals">=</div>
  </div>
</div>

<!-- JavaScript (JS) implementation of the calculator. -->
<script>
// Initialize important variables to manage calculations
var firstNumber = null;
var operator = null;
var nextReady = true;

// Build objects containing key elements
const output = document.getElementById("output");
const numbers = document.querySelectorAll(".calculator-number");
const operations = document.querySelectorAll(".calculator-operation");
const clear = document.querySelectorAll(".calculator-clear");
const equals = document.querySelectorAll(".calculator-equals");

// Number buttons listener
numbers.forEach(button => {
  button.addEventListener("click", function() {
    number(button.textContent);
  });
});

// Number action
function number(value) {
  if (value != ".") {
    if (nextReady == true) {
      output.innerHTML = value;
      if (value != "0") {
        nextReady = false;
      }
    } else {
      output.innerHTML = output.innerHTML + value;
    }
  } else {
    if (output.innerHTML.indexOf(".") == -1) {
      output.innerHTML = output.innerHTML + value;
      nextReady = false;
    }
  }
}

// Operation buttons listener
operations.forEach(button => {
  button.addEventListener("click", function() {
    operation(button.textContent);
  });
});

// Operator action
function operation(choice) {
  if (firstNumber == null) {
    firstNumber = parseInt(output.innerHTML);
    nextReady = true;
    operator = choice;
    return;
  }
  firstNumber = calculate(firstNumber, parseFloat(output.innerHTML));
  operator = choice;
  output.innerHTML = firstNumber.toString();
  nextReady = true;
}

// Calculator
function calculate(first, second) {
  let result = 0;
  switch (operator) {
    case "+":
      result = first + second;
      break;
    case "-":
      result = first - second;
      break;
    case "*":
      result = first * second;
      break;
    case "/":
      result = first / second;
      break;
    default:
      break;
  }
  return result;
}

// Equals button listener
equals.forEach(button => {
  button.addEventListener("click", function() {
    equal();
  });
});

// Equal action
function equal() {
  firstNumber = calculate(firstNumber, parseFloat(output.innerHTML));
  output.innerHTML = firstNumber.toString();
  nextReady = true;
}

// Clear button listener
clear.forEach(button => {
  button.addEventListener("click", function() {
    clearCalc();
  });
});

// A/C action
function clearCalc() {
  firstNumber = null;
  output.innerHTML = "0";
  nextReady = true;
}
</script>

<!-- 
Vanta animations just for fun, load JS onto the page
-->
<script src="{{site.baseurl}}/assets/js/three.r119.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.halo.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.birds.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.net.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.rings.min.js"></script>

<script>
// Setup Vanta scripts as functions
var vantaInstances = {
  halo: VANTA.HALO,
  birds: VANTA.BIRDS,
  net: VANTA.NET,
  rings: VANTA.RINGS
};

// Obtain a random Vanta function
var vantaInstance = vantaInstances[Object.keys(vantaInstances)[Math.floor(Math.random() * Object.keys(vantaInstances).length)]];

// Run the animation
vantaInstance({
  el: "#animation",
  mouseControls: true,
  touchControls: true,
  gyroControls: false
});
</script>
