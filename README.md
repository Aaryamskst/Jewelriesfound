# Jewelriesfound
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tic Tac Toe</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <!-- GAME SCREEN -->
  <div class="game-container">
    <h1>Tic Tac Toe</h1>
    <p id="status">Player X's Turn</p>

    <div class="board">
      <div class="cell" data-index="0"></div>
      <div class="cell" data-index="1"></div>
      <div class="cell" data-index="2"></div>
      <div class="cell" data-index="3"></div>
      <div class="cell" data-index="4"></div>
      <div class="cell" data-index="5"></div>
      <div class="cell" data-index="6"></div>
      <div class="cell" data-index="7"></div>
      <div class="cell" data-index="8"></div>
    </div>
  </div>

  <!-- RESULT SCREEN (HIDDEN BY DEFAULT) -->
  <div id="result-screen" class="hidden">
    <div class="result-box">
      <h2 id="result-text"></h2>
      <button onclick="resetGame()">New Game</button>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>
* {
  box-sizing: border-box;
  font-family: Arial, sans-serif;
}

body {
  margin: 0;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #667eea, #764ba2);
}

/* GAME BOX */
.game-container {
  background: #fff;
  padding: 20px;
  border-radius: 15px;
  width: 90%;
  max-width: 350px;
  text-align: center;
}

#status {
  margin: 10px 0;
  font-size: 18px;
}

/* BOARD */
.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 8px;
}

.cell {
  aspect-ratio: 1 / 1;
  border: 2px solid #333;
  font-size: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

/* RESULT SCREEN */
#result-screen {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.6);
  display: flex;
  justify-content: center;
  align-items: center;
}

/* IMPORTANT FIX */
.hidden {
  display: none !important;
}

.result-box {
  background: white;
  padding: 30px;
  border-radius: 15px;
  text-align: center;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
const cells = document.querySelectorAll(".cell");
const statusText = document.getElementById("status");
const resultScreen = document.getElementById("result-screen");
const resultText = document.getElementById("result-text");

let currentPlayer = "X";
let board = ["", "", "", "", "", "", "", "", ""];
let gameActive = true;

const wins = [
  [0,1,2], [3,4,5], [6,7,8],
  [0,3,6], [1,4,7], [2,5,8],
  [0,4,8], [2,4,6]
];

cells.forEach(cell => {
  cell.addEventListener("click", () => {
    const index = cell.dataset.index;

    if (!gameActive || board[index] !== "") return;

    board[index] = currentPlayer;
    cell.textContent = currentPlayer;

    if (checkWin()) {
      showResult(`Player ${currentPlayer} Wins 🎉`);
    } 
    else if (!board.includes("")) {
      showResult("It's a Draw 🤝");
    } 
    else {
      currentPlayer = currentPlayer === "X" ? "O" : "X";
      statusText.textContent = `Player ${currentPlayer}'s Turn`;
    }
  });
});

function checkWin() {
  return wins.some(combo =>
    combo.every(i => board[i] === currentPlayer)
  );
}

function showResult(message) {
  gameActive = false;
  resultText.textContent = message;
  resultScreen.classList.remove("hidden");
}

function resetGame() {
  board = ["", "", "", "", "", "", "", "", ""];
  currentPlayer = "X";
  gameActive = true;
  statusText.textContent = "Player X's Turn";
  cells.forEach(cell => cell.textContent = "");
  resultScreen.classList.add("hidden");
}
