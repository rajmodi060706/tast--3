<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tic-Tac-Toe Game</title>

<style>
    body {
        font-family: Arial, sans-serif;
        text-align: center;
        background: #f5f5f5;
        margin: 0;
        padding: 40px;
    }

    h1 {
        margin-bottom: 20px;
    }

    .game-container {
        display: grid;
        grid-template-columns: repeat(3, 120px);
        grid-template-rows: repeat(3, 120px);
        gap: 10px;
        justify-content: center;
        margin: 0 auto;
    }

    .cell {
        background: white;
        border-radius: 10px;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 60px;
        font-weight: bold;
        cursor: pointer;
        box-shadow: 0 4px 10px rgba(0,0,0,0.15);
        transition: 0.3s;
    }

    .cell:hover {
        background: #eee;
    }

    .winner {
        background: #4caf50 !important;
        color: white;
    }

    .controls {
        margin-top: 30px;
    }

    button {
        padding: 10px 20px;
        margin: 10px;
        border: none;
        font-size: 18px;
        border-radius: 8px;
        cursor: pointer;
        transition: 0.3s;
    }

    #resetBtn { background: #f44336; color: white; }
    #aiBtn { background: #2196f3; color: white; }

    button:hover {
        opacity: 0.85;
    }

    #status {
        margin-top: 20px;
        font-size: 22px;
        font-weight: bold;
    }
</style>
</head>
<body>

<h1>Tic-Tac-Toe</h1>

<div class="game-container" id="board">
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

<div id="status">Player X's turn</div>

<div class="controls">
    <button id="resetBtn">Reset Game</button>
    <button id="aiBtn">Play vs AI (Toggle)</button>
</div>

<script>
    const cells = document.querySelectorAll(".cell");
    const status = document.getElementById("status");

    let board = ["", "", "", "", "", "", "", "", ""];
    let currentPlayer = "X";
    let gameActive = true;
    let vsAI = false;

    const winPatterns = [
        [0,1,2], [3,4,5], [6,7,8],     // rows
        [0,3,6], [1,4,7], [2,5,8],     // columns
        [0,4,8], [2,4,6]               // diagonals
    ];

    function checkWinner() {
        for (let pattern of winPatterns) {
            let [a, b, c] = pattern;
            if (board[a] && board[a] === board[b] && board[a] === board[c]) {
                highlightWinner(pattern);
                status.textContent = `Player ${board[a]} Wins!`;
                gameActive = false;
                return true;
            }
        }
        if (!board.includes("")) {
            status.textContent = "Draw!";
            gameActive = false;
            return true;
        }
        return false;
    }

    function highlightWinner(pattern) {
        pattern.forEach(i => {
            cells[i].classList.add("winner");
        });
    }

    function handleClick(e) {
        const index = e.target.getAttribute("data-index");

        if (!gameActive || board[index] !== "") return;

        board[index] = currentPlayer;
        e.target.textContent = currentPlayer;

        if (checkWinner()) return;

        currentPlayer = currentPlayer === "X" ? "O" : "X";
        status.textContent = `Player ${currentPlayer}'s turn`;

        if (vsAI && currentPlayer === "O") {
            setTimeout(aiMove, 500);
        }
    }

    function aiMove() {
        let emptyCells = board
            .map((val, idx) => val === "" ? idx : null)
            .filter(val => val !== null);

        let randomIndex = emptyCells[Math.floor(Math.random() * emptyCells.length)];

        board[randomIndex] = "O";
        cells[randomIndex].textContent = "O";

        if (checkWinner()) return;

        currentPlayer = "X";
        status.textContent = "Player X's turn";
    }

    function resetGame() {
        board = ["", "", "", "", "", "", "", "", ""];
        currentPlayer = "X";
        gameActive = true;
        status.textContent = "Player X's turn";

        cells.forEach(cell => {
            cell.textContent = "";
            cell.classList.remove("winner");
        });
    }

    document.getElementById("resetBtn").addEventListener("click", resetGame);

    document.getElementById("aiBtn").addEventListener("click", function() {
        vsAI = !vsAI;
        resetGame();
        this.textContent = vsAI ? "AI Mode: ON" : "Play vs AI (Toggle)";
    });

    cells.forEach(cell => {
        cell.addEventListener("click", handleClick);
    });
</script>

</body>
</html>
