<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Juego de X y O</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background-color: #222;
    color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
  }
  h1 {
    color: #0f0;
  }
  .board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-gap: 5px;
  }
  .cell {
    width: 100px;
    height: 100px;
    background-color: #333;
    color: #0f0;
    font-size: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    border: 2px solid #0f0;
  }
  .cell.taken {
    pointer-events: none;
  }
  .result {
    margin-top: 20px;
    font-size: 24px;
  }
  button {
    margin-top: 20px;
    padding: 10px 20px;
    font-size: 16px;
    background-color: #0f0;
    color: #222;
    border: none;
    cursor: pointer;
    border-radius: 5px;
  }
  button:hover {
    background-color: #0a0;
  }
</style>
</head>
<body>
  <h1>Juego de X y O</h1>
  <div class="board" id="board"></div>
  <div class="result" id="result"></div>
  <button onclick="resetGame()">Reiniciar Juego</button>

<script>
  const board = document.getElementById('board');
  const result = document.getElementById('result');
  let currentPlayer = 'X';
  let gameActive = true;
  let gameState = ['', '', '', '', '', '', '', '', ''];

  const winningConditions = [
    [0, 1, 2], // Fila superior
    [3, 4, 5], // Fila central
    [6, 7, 8], // Fila inferior
    [0, 3, 6], // Columna izquierda
    [1, 4, 7], // Columna central
    [2, 5, 8], // Columna derecha
    [0, 4, 8], // Diagonal principal
    [2, 4, 6], // Diagonal secundaria
  ];

  function createBoard() {
    board.innerHTML = '';
    gameState.forEach((cell, index) => {
      const cellElement = document.createElement('div');
      cellElement.classList.add('cell');
      cellElement.dataset.index = index;
      cellElement.addEventListener('click', handleCellClick);
      cellElement.textContent = cell;
      board.appendChild(cellElement);
    });
  }

  function handleCellClick(event) {
    const cellIndex = event.target.dataset.index;

    if (gameState[cellIndex] !== '' || !gameActive) {
      return;
    }

    gameState[cellIndex] = currentPlayer;
    event.target.textContent = currentPlayer;
    event.target.classList.add('taken');

    if (checkWinner()) {
      result.textContent = `¡El jugador ${currentPlayer} gana! 🎉`;
      gameActive = false;
    } else if (!gameState.includes('')) {
      result.textContent = '¡Es un empate! 🤝';
      gameActive = false;
    } else {
      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
    }
  }

  function checkWinner() {
    return winningConditions.some(condition => {
      return condition.every(index => gameState[index] === currentPlayer);
    });
  }

  function resetGame() {
    gameActive = true;
    currentPlayer = 'X';
    gameState = ['', '', '', '', '', '', '', '', ''];
    result.textContent = '';
    createBoard();
  }

  createBoard();
</script>
</body>
</html>
