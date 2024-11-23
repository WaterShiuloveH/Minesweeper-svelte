<script>
  export let isMine = false;
  export let isRevealed = false;
  export let isFlagged = false;
  export let nearbyMines = 0;

  export let onReveal;
  export let onFlag;

  const handleRightClick = (event) => {
    event.preventDefault();
    if (!isRevealed) onFlag();
  };
</script>

<button
  class="cell"
  class:is-revealed={isRevealed}
  class:is-flagged={isFlagged}
  class:is-mine={isMine && isRevealed}
  on:click={onReveal}
  on:contextmenu={handleRightClick}
>
  {#if isRevealed}
    {#if isMine}
      💣
    {:else if nearbyMines > 0}
      {nearbyMines}
    {/if}
  {/if}
</button>

<style>
  .cell {
    all: unset;
    width: 30px;
    height: 30px;
    border: 1px solid #ccc;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    background-color: #ddd;
    transition: background-color 0.2s ease, box-shadow 0.2s ease;
  }

  .cell.is-revealed {
    background-color: #f5f5f5;
    cursor: default;
    box-shadow: inset 0px 0px 3px #888;
    color: black;
  }

  .cell.is-flagged {
    background-color: #ffeeba;
    border-color: #ffc107;
  }

  .cell.is-mine {
    background-color: #ff6f61;
    border-color: #dc3545;
    animation: shake 0.2s ease-in-out;
  }

  @keyframes shake {
    0% { transform: translateX(0); }
    25% { transform: translateX(-2px); }
    50% { transform: translateX(2px); }
    75% { transform: translateX(-2px); }
    100% { transform: translateX(0); }
  }

  .cell:hover {
    background-color: #e0e0e0;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  }
</style>
