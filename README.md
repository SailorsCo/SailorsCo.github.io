<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Color Picker</title>
    <style>
        body {
            transition: background-color 0.5s;
            text-align: center;
            font-family: Arial, sans-serif;
            padding: 20px;
        }
        .color-btn {
            padding: 10px 20px;
            margin: 5px;
            border: none;
            cursor: pointer;
            font-size: 16px;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <h2>Select a Background Color</h2>
    <button class="color-btn" style="background-color: blue;" onclick="changeColor('blue')">Blue</button>
    <button class="color-btn" style="background-color: black; color: white;" onclick="changeColor('black')">Black</button>
    <button class="color-btn" style="background-color: green;" onclick="changeColor('green')">Green</button>
    <button class="color-btn" style="background-color: white; border: 1px solid #ccc;" onclick="changeColor('white')">White</button>
    <button class="color-btn" style="background-color: orange;" onclick="changeColor('orange')">Orange</button>

    <script>
        function changeColor(color) {
            document.body.style.backgroundColor = color;
            localStorage.setItem('bgColor', color);
        }

        // Set default color or retrieve from localStorage
        document.addEventListener("DOMContentLoaded", function () {
            const savedColor = localStorage.getItem('bgColor') || 'black';
            document.body.style.backgroundColor = savedColor;
        });
    </script>
</body>
</html>
