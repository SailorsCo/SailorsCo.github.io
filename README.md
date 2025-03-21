Made by your boy Salaarski
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cookie Clicker</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #fce4b6;
            font-family: Arial, sans-serif;
            position: relative;
        }
        button {
            padding: 15px 30px;
            font-size: 20px;
            border: none;
            border-radius: 10px;
            background-color: #ffcc66;
            cursor: pointer;
            transition: 0.3s;
        }
        button:hover {
            background-color: #ffb347;
        }
        #cookieImg {
            position: absolute;
            left: 20px;
            top: 50%;
            transform: translateY(-50%);
            display: none;
            width: 100px;
        }
    </style>
</head>
<body>
    <img id="cookieImg" src="https://png.pngtree.com/png-clipart/20240320/original/pngtree-chocolate-chip-cookie-png-image_14636259.png" alt="Cookie">
    <button onclick="handleClick()">1 Click Equals One Cookie</button>

    <script>
        let clickCount = 0;

        function handleClick() {
            clickCount++;
            if (clickCount === 1) {
                document.getElementById('cookieImg').style.display = 'block';
            } else if (clickCount === 2) {
                window.open('','_self').close();
            }
        }
    </script>
</body>
</html>
