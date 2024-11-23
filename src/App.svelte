<script>
  import Cell from "./components/Cell.svelte";

  const GRID_SIZE = 8;
  const MINE_COUNT = 10;

  let grid = [];
  let gameOver = false;
  let gameWon = false;

  const initializeGrid = () => {
    grid = Array.from({ length: GRID_SIZE }, (_, row) =>
      Array.from({ length: GRID_SIZE }, (_, col) => ({
        isMine: false,
        isRevealed: false,
        isFlagged: false,
        nearbyMines: 0,
        row,
        col,
      }))
    );

    gameOver = false;
    gameWon = false;

    // Place mines randomly
    let minesPlaced = 0;
    while (minesPlaced < MINE_COUNT) {
      const row = Math.floor(Math.random() * GRID_SIZE);
      const col = Math.floor(Math.random() * GRID_SIZE);
      if (!grid[row][col].isMine) {
        grid[row][col].isMine = true;
        minesPlaced++;
      }
    }

    // Calculate nearby mines
    for (let row = 0; row < GRID_SIZE; row++) {
      for (let col = 0; col < GRID_SIZE; col++) {
        if (grid[row][col].isMine) continue;

        grid[row][col].nearbyMines = getNeighbors(row, col).filter(
          ({ isMine }) => isMine
        ).length;
      }
    }
  };

  const getNeighbors = (row, col) => {
    const neighbors = [];
    for (let r = -1; r <= 1; r++) {
      for (let c = -1; c <= 1; c++) {
        if (
          (r === 0 && c === 0) ||
          row + r < 0 ||
          row + r >= GRID_SIZE ||
          col + c < 0 ||
          col + c >= GRID_SIZE
        ) {
          continue;
        }
        neighbors.push(grid[row + r][col + c]);
      }
    }
    return neighbors;
  };

  const checkWinCondition = () => {
    const allNonMineCellsRevealed = grid.every((row) =>
      row.every((cell) => (cell.isMine ? true : cell.isRevealed))
    );

    if (allNonMineCellsRevealed) {
      gameWon = true;
    }
  };

  const revealCell = (cell) => {
    if (gameOver || gameWon || cell.isRevealed || cell.isFlagged) return;

    // Mark the cell as revealed
    cell.isRevealed = true;
    grid = [...grid]; // Trigger reactivity

    if (cell.isMine) {
      gameOver = true;
      return;
    }

    // Auto-reveal neighbors if no nearby mines
    if (cell.nearbyMines === 0) {
      getNeighbors(cell.row, cell.col).forEach(revealCell);
    }

    checkWinCondition(); // Check win condition
  };

  const toggleFlag = (cell) => {
    if (gameOver || cell.isRevealed) return;
    cell.isFlagged = !cell.isFlagged;
    grid = [...grid];
    checkWinCondition();
  };

  initializeGrid();
</script>

<main>
  <h1>Minesweeper</h1>
  <button on:click={initializeGrid}>Restart</button>
  {#if gameOver}
    <p class="status game-over">💥 Game Over!</p>
  {/if}
  {#if gameWon}
    <p class="status game-won">🎉 You Win!</p>
  {/if}
  <div class="grid" style="grid-template-columns: repeat({GRID_SIZE}, 1fr);">
    {#each grid as row}
      {#each row as cell}
        <Cell
          {...cell}
          onReveal={() => revealCell(cell)}
          onFlag={() => toggleFlag(cell)}
        />
      {/each}
    {/each}
  </div>
</main>

<style>
  main {
    text-align: center;
    font-family: Arial, sans-serif;
  }
  .grid {
    display: grid;
    gap: 2px;
    margin: 20px auto;
    width: fit-content;
  }
  .status {
    margin: 10px;
    font-size: 18px;
    font-weight: bold;
  }
  .status.game-over {
    color: red;
  }
  .status.game-won {
    color: green;
  }
</style>
