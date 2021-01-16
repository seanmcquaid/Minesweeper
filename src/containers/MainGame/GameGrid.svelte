<script>
  import { onMount } from 'svelte';

  import grid from './grid';
  let gameGrid = [...grid];
  let totalBombs = 8;
  let spacesLeft = 0;
  onMount(() => {
    let totalBombSpaces = 0;
    const bombSpaceIds = [];
    const bombSpacesCoordinates = [];

    while (totalBombSpaces !== totalBombs) {
      const flattenedGameGrid = gameGrid.flat();
      const randomSpaceIndex = Math.floor(
        Math.random() * flattenedGameGrid.length
      );

      if (!flattenedGameGrid[randomSpaceIndex].hasBomb) {
        flattenedGameGrid[randomSpaceIndex].hasBomb = true;
        bombSpaceIds.push(flattenedGameGrid[randomSpaceIndex].id);
        totalBombSpaces++;
      }
    }

    gameGrid = gameGrid;
  });

  function gridSpaceOnClick(gridSpaceInfo) {
    gridSpaceInfo.isSelected = true;
    gameGrid = gameGrid;
  }

  $: spacesLeft =
    gameGrid.flat().filter((gridSpace) => !gridSpace.isSelected).length -
    totalBombs;
</script>

<p>{spacesLeft}</p>

<ol class="gameGrid">
  {#each gameGrid as row, i}
    <ol class="gridRow">
      {#each row as gridSpace}
        <li
          class:selected={gridSpace.isSelected}
          on:click={() => gridSpaceOnClick(gridSpace)}
        >
          {gridSpace.hasBomb ? 'bomb' : gridSpace.spacesNearBomb}
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
    list-style: none;
  }
  li {
    padding: 1rem;
    margin: 0.15rem;
    background-color: black;
    color: black;
    width: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .selected {
    color: white;
  }
</style>
