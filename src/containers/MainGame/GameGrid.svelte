<script>
  import { onMount } from 'svelte';
  import grid from './grid';

  let gameGrid = [...grid];
  let totalBombs = 8;
  let spacesLeft = 0;
  let isGameOver = false;

  onMount(() => {
    const bombSpaceIds = [];
    const flattenedGameGrid = gameGrid.flat();

    while (bombSpaceIds.length !== totalBombs) {
      const randomSpaceIndex = Math.floor(
        Math.random() * flattenedGameGrid.length
      );

      if (!bombSpaceIds.includes(flattenedGameGrid[randomSpaceIndex].id)) {
        bombSpaceIds.push(flattenedGameGrid[randomSpaceIndex].id);
      }
    }

    const rows = [
      {
        rowName: 'A',
        rowIndex: 0,
      },
      {
        rowName: 'B',
        rowIndex: 1,
      },
      {
        rowName: 'C',
        rowIndex: 2,
      },
      {
        rowName: 'D',
        rowIndex: 3,
      },
      {
        rowName: 'E',
        rowIndex: 4,
      },
      {
        rowName: 'F',
        rowIndex: 5,
      },
      {
        rowName: 'G',
        rowIndex: 6,
      },
      {
        rowName: 'H',
        rowIndex: 7,
      },
    ];

    for (let i = 0; i < bombSpaceIds.length; i++) {
      const rowIndex = rows.find((row) => bombSpaceIds[i].includes(row.rowName))
        .rowIndex;
      const columnIndex =
        Number.parseInt(bombSpaceIds[i].replace(/^\D+/g, '')) - 1;

      // set the bomb
      gameGrid[rowIndex][columnIndex] = {
        ...gameGrid[rowIndex][columnIndex],
        hasBomb: true,
      };

      //above
      if (rowIndex > 0) {
        gameGrid[rowIndex - 1][columnIndex] = {
          ...gameGrid[rowIndex - 1][columnIndex],
          spacesNearBomb: 1,
        };
      }

      //below
      if (rowIndex < 7) {
        gameGrid[rowIndex + 1][columnIndex] = {
          ...gameGrid[rowIndex + 1][columnIndex],
          spacesNearBomb: 1,
        };
      }

      //left
      if (columnIndex > 0) {
        gameGrid[rowIndex][columnIndex - 1] = {
          ...gameGrid[rowIndex][columnIndex - 1],
          spacesNearBomb: 1,
        };
      }

      //right
      if (columnIndex < 7) {
        gameGrid[rowIndex][columnIndex + 1] = {
          ...gameGrid[rowIndex][columnIndex + 1],
          spacesNearBomb: 1,
        };
      }

      // top left
      if (rowIndex > 0 && columnIndex > 0) {
        gameGrid[rowIndex - 1][columnIndex - 1] = {
          ...gameGrid[rowIndex - 1][columnIndex - 1],
          spacesNearBomb: 1,
        };
      }

      // top right
      if (rowIndex > 0 && columnIndex < 7) {
        gameGrid[rowIndex - 1][columnIndex + 1] = {
          ...gameGrid[rowIndex - 1][columnIndex + 1],
          spacesNearBomb: 1,
        };
      }

      // // bottom right
      if (rowIndex < 7 && columnIndex < 7) {
        gameGrid[rowIndex + 1][columnIndex + 1] = {
          ...gameGrid[rowIndex + 1][columnIndex + 1],
          spacesNearBomb: 1,
        };
      }

      // bottom left
      if (rowIndex < 7 && columnIndex > 0) {
        gameGrid[rowIndex + 1][columnIndex - 1] = {
          ...gameGrid[rowIndex + 1][columnIndex - 1],
          spacesNearBomb: 1,
        };
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
  $: isGameOver =
    gameGrid
      .flat()
      .filter((gridSpace) => gridSpace.isSelected && gridSpace.hasBomb).length >
    0;
</script>

<p>{isGameOver ? 'GAME OVER' : `${spacesLeft} empty spaces left`}</p>

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
    margin: 0;
    padding: 0;
  }
  .gridRow {
    display: flex;
    list-style: none;
    margin: 0;
    padding: 0;
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
