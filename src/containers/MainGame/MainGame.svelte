<script>
  import GameGrid from './GameGrid.svelte';

  let isGameStarted = false;

  function startGameButtonOnClick() {
    isGameStarted = true;
  }
</script>

<main>
  <h1>Minesweeper</h1>
  {#if isGameStarted}
    <GameGrid />
  {:else}
    <button on:click={startGameButtonOnClick}>Start</button>
  {/if}
</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
</style>
