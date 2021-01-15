<script>
  import GameGrid from './GameGrid.svelte';

  let startGame = false;

  function startGameOnClick() {
    startGame = true;
  }
</script>

<main>
  <h1>Start</h1>
  {#if startGame}
    <GameGrid />
  {:else}
    <button on:click={startGameOnClick}>Start</button>
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
