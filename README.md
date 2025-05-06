<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>10-Second Timer</title>
</head>
<body>

  <h1>10-Second Timer</h1>
  <p id="timer">10</p>

  <audio id="endSound" src="https://www.soundjay.com/button/beep-07.wav" preload="auto"></audio>

  <script>
    let timeLeft = 10;
    const timerElement = document.getElementById('timer');
    const sound = document.getElementById('endSound');

    const countdown = setInterval(function() {
      timeLeft--;
      timerElement.innerText = timeLeft;

      if (timeLeft <= 0) {
        clearInterval(countdown);  // Stop the countdown
        sound.play();  // Play the sound when the timer ends
      }
    }, 1000);
  </script>

</body>
</html>
