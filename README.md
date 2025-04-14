<!DOCTYPE html>yyy
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Congrats Popup</title>
  <style>
    #popup {
      display: none;
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: white;
      border: 2px solid #ccc;
      padding: 30px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      z-index: 1000;
      border-radius: 10px;
      text-align: center;
    }

    #overlay {
      display: none;
      position: fixed;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      background: rgba(0, 0, 0, 0.5);
      z-index: 999;
    }

    button {
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    button:hover {
      background-color: #45a049;
    }
  </style>
</head>
<body>

  <div id="overlay"></div>
  <div id="popup">
    <h2>Congrats!</h2>
    <p>You made it to 1 minute 🎉</p>
    <button onclick="closePopup()">Thanks</button>
  </div>

  <script>
    function showPopup() {
      document.getElementById('overlay').style.display = 'block';
      document.getElementById('popup').style.display = 'block';
    }

    function closePopup() {
      document.getElementById('overlay').style.display = 'none';
      document.getElementById('popup').style.display = 'none';
      startTimer(); // restart the timer after closing
    }

    function startTimer() {
      setTimeout(showPopup, 10000); // 10000ms = 1 minute
    }

    // Start the initial timer
    startTimer();
  </script>

</body>
</html>
