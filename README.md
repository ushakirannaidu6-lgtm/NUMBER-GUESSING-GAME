# NUMBER-GUESSING-GAME
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Advanced Number Guessing Game</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #ff416c, #ff4b2b);
      color: white;
      text-align: center;
      padding-top: 50px;
    }

    .container {
      background: rgba(0, 0, 0, 0.8);
      padding: 25px;
      border-radius: 15px;
      width: 320px;
      margin: auto;
      box-shadow: 0 0 20px rgba(0,0,0,0.5);
      animation: fadeIn 1s ease-in-out;
    }

    h2 {
      margin-bottom: 10px;
    }

    select, input {
      padding: 10px;
      width: 85%;
      margin: 10px 0;
      border-radius: 5px;
      border: none;
      outline: none;
    }

    button {
      padding: 10px 20px;
      margin: 5px;
      border: none;
      border-radius: 5px;
      background: #00c6ff;
      color: white;
      font-weight: bold;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      background: #0072ff;
      transform: scale(1.05);
    }

    #message {
      font-size: 18px;
      font-weight: bold;
      margin-top: 15px;
    }

    #attempts, #bestScore {
      margin-top: 10px;
    }

    @keyframes fadeIn {
      from {opacity: 0; transform: translateY(20px);}
      to {opacity: 1; transform: translateY(0);}
    }
  </style>
</head>
<body>

  <div class="container">
    <h2>🎯 Guess The Number</h2>

    <label>Select Difficulty:</label><br>
    <select id="difficulty" onchange="setDifficulty()">
      <option value="100">Easy (1-100)</option>
      <option value="50">Medium (1-50)</option>
      <option value="20">Hard (1-20)</option>
    </select>

    <p id="rangeText">Range: 1 - 100</p>

    <input type="number" id="guessInput" placeholder="Enter your guess">
    <br>

    <button onclick="checkGuess()">Guess</button>
    <button onclick="restartGame()">Restart</button>

    <p id="message"></p>
    <p id="attempts"></p>
    <p id="bestScore"></p>
  </div>

  <script>
    let maxRange = 100;
    let randomNumber = Math.floor(Math.random() * maxRange) + 1;
    let attempts = 0;
    let maxAttempts = 10;
    let bestScore = localStorage.getItem("bestScore") || null;

    document.getElementById("bestScore").textContent =
      bestScore ? "Best Score: " + bestScore : "Best Score: --";

    function setDifficulty() {
      maxRange = Number(document.getElementById("difficulty").value);
      document.getElementById("rangeText").textContent = "Range: 1 - " + maxRange;
      restartGame();
    }

    function checkGuess() {
      const userGuess = Number(document.getElementById("guessInput").value);
      const message = document.getElementById("message");
      const attemptsText = document.getElementById("attempts");

      if (!userGuess) {
        message.textContent = "⚠ Enter a valid number!";
        return;
      }

      attempts++;

      if (userGuess === randomNumber) {
        message.textContent = "🎉 Correct! You Win!";
        if (!bestScore || attempts < bestScore) {
          localStorage.setItem("bestScore", attempts);
          document.getElementById("bestScore").textContent = "Best Score: " + attempts;
        }
      } else if (attempts >= maxAttempts) {
        message.textContent = "❌ Game Over! Number was " + randomNumber;
      } else if (userGuess > randomNumber) {
        message.textContent = "📉 Too High!";
      } else {
        message.textContent = "📈 Too Low!";
      }

      attemptsText.textContent = "Attempts: " + attempts + " / " + maxAttempts;
    }

    function restartGame() {
      randomNumber = Math.floor(Math.random() * maxRange) + 1;
      attempts = 0;
      document.getElementById("message").textContent = "";
      document.getElementById("attempts").textContent = "";
      document.getElementById("guessInput").value = "";
    }
  </script>

</body>
</html>
