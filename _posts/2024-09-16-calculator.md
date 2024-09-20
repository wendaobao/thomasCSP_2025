---
layout: page
permalink: /calculator/
toc: true
comments: false
---

<!-- Calculator Layout -->
<style>
  body {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background: #f0f0f0;
    margin: 0;
  }
  .calculator-container {
    display: grid;
    grid-template-columns: repeat(4, 80px);
    grid-gap: 10px;
    background: #333;
    border-radius: 15px;
    padding: 15px;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
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
  .calculator-button {
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
  .calculator-button:hover {
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

<div class="calculator-container">
  <!-- result -->
  <div class="calculator-output">0</div>
  <!-- row 1 -->
  <div class="calculator-button">1</div>
  <div class="calculator-button">2</div>
  <div class="calculator-button">3</div>
  <div class="calculator-button">+</div>
  <!-- row 2 -->
  <div class="calculator-button">4</div>
  <div class="calculator-button">5</div>
  <div class="calculator-button">6</div>
  <div class="calculator-button">-</div>
  <!-- row 3 -->
  <div class="calculator-button">7</div>
  <div class="calculator-button">8</div>
  <div class="calculator-button">9</div>
  <div class="calculator-button">*</div>
  <!-- row 4 -->
  <div class="calculator-clear">A/C</div>
  <div class="calculator-button">0</div>
  <div class="calculator-button">.</div>
  <div class="calculator-equals">=</div>
</div>
