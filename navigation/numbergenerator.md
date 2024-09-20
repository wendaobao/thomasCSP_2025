---
layout: post
title: Random Number Generator
permalink: /randomnumber/
toc: true
comments: false
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f5f5f5;
            margin: 0;
            padding: 0;
        }
        .generator-container {
            margin-top: 50px;
        }
        #result {
            font-size: 48px;
            color: #4CAF50;
            margin: 20px 0;
        }
        input {
            padding: 10px;
            margin: 10px;
            font-size: 16px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            background-color: #4caf50;
            color: white;
            box-shadow: 0 0 10px #88bc4c, 0 0 20px #88bc4c;
            transition: all 0.3s ease;
        }
        button:hover {
            background-color: #45a049;
            box-shadow: 0 0 15px #88bc4c, 0 0 25px #88bc4c;
        }
    </style>
</head>
<body>
    <div class="generator-container">
        <h1>Random Number Generator</h1>
        <p>Enter a range and click the button to generate a random number</p>
        <input type="number" id="minValue" placeholder="Min value" value="1">
        <input type="number" id="maxValue" placeholder="Max value" value="100">
        <div id="result">0</div>
        <button onclick="generateRandomNumber()">Generate Number</button>
    </div>

    <script>
        function generateRandomNumber() {
            const min = parseInt(document.getElementById('minValue').value);
            const max = parseInt(document.getElementById('maxValue').value);
            if (isNaN(min) || isNaN(max) || min >= max) {
                alert("Please enter valid minimum and maximum values.");
                return;
            }
            const randomNumber = Math.floor(Math.random() * (max - min + 1)) + min;
            document.getElementById('result').textContent = randomNumber;
        }
    </script>
</body>
</html>
