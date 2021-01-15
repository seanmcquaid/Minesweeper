<script>
  import grid from './grid';
  let gameGrid = grid;
  let totalBombs = 8;
  let spacesLeft = gameGrid.flat().length - totalBombs;
  $: {
    for (let i = 0; i < totalBombs; i++) {
      // choose 8 random spaces for bombs
    }
  }

  function gridSpaceOnClick(gridSpaceInfo) {
    gridSpaceInfo.isSelected = true;
    gameGrid = gameGrid;
  }
</script>

<ol class="gameGrid">
  {#each gameGrid as row, i}
    <ol class="gridRow">
      {#each row as gridSpace}
        <li
          class:selected={gridSpace.isSelected}
          on:click={() => gridSpaceOnClick(gridSpace)}
        >
          {gridSpace.isSelected}
        </li>
      {/each}
    </ol>
  {/each}
</ol>

<style>
  .gameGrid {
    height: 100%;
    list-style: none;
  }
  .gridRow {
    display: flex;
    padding: 1rem;
    list-style: none;
  }
  li {
    padding: 1rem;
  }
  .selected {
    color: blue;
    background-color: red;
  }
</style>
