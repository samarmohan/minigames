<script lang="ts">
    const GRID_SIZE = 9;

    type Cell = 'on' | 'off';

    let clicks = 0;
    let won = false;
    let grid: Cell[][] = [...Array(GRID_SIZE)].map(() =>
        [...Array(GRID_SIZE)].map(() => 'off')
    );

    const addOne = () => {
        clicks += 1;
    };

    const hasWon = () => {
        won = true;
        for (let i = 0; i < GRID_SIZE; i++) {
            for (let j = 0; j < GRID_SIZE; j++) {
                if (grid[i][j] == 'on') {
                    won = false;
                }
            }
        }
    };

    // toggle cells next to, above, below, and to the left of the clicked cell
    const toggleCell = (row: number, col: number) => {
        grid[row][col] = grid[row][col] === 'on' ? 'off' : 'on';
        if (row > 0)
            grid[row - 1][col] = grid[row - 1][col] === 'on' ? 'off' : 'on';
        if (row < GRID_SIZE - 1)
            grid[row + 1][col] = grid[row + 1][col] === 'on' ? 'off' : 'on';
        if (col > 0)
            grid[row][col - 1] = grid[row][col - 1] === 'on' ? 'off' : 'on';
        if (col < GRID_SIZE - 1)
            grid[row][col + 1] = grid[row][col + 1] === 'on' ? 'off' : 'on';
    };

    const genBoard = () => {
        for (let i = 0; i < GRID_SIZE; i++) {
            for (let j = 0; j < GRID_SIZE; j++) {
                if (Math.random() < 0.25) {
                    toggleCell(i, j);
                }
            }
        }
    };

    genBoard();
</script>

<main>
    <h2>Lights Out</h2>
    <em class="clickCount">clicks = {clicks}</em>
    {#if won}
        <h1>You won in {clicks} {clicks == 1 ? 'click' : 'clicks'}</h1>
    {:else}
        <div class="center">
            <div>
                {#each grid as row, i}
                    <div class="row">
                        {#each row as cell, k}
                            <!-- svelte-ignore a11y-click-events-have-key-events -->
                            <!-- svelte-ignore a11y-no-static-element-interactions -->
                            <div
                                on:click={addOne}
                                on:click={() => {
                                    toggleCell(i, k);
                                }}
                                on:click={hasWon}
                                class={`square ${cell}`}
                            />
                        {/each}
                    </div>
                {/each}
            </div>
        </div>
        <p>
            This is a <a target="_blank" href="https://www.logicgamesonline.com/lightsout"
                >lights out</a
            > implementation in Svelte. <br />
            The goal of the game is to turn all the squares black. <br />
            Clicking a square toggles itself and the 4 adjacent squares on or off. 
        </p>
    {/if}
</main>

<style>
    a {
        font-weight: 500;
    }

    .square {
        width: 50px;
        height: 50px;
        border: 1px solid white;
    }

    .center {
        display: flex;
        justify-content: top;
        align-items: center;
    }

    .on {
        background-color: green;
    }

    .off {
        background-color: black;
    }

    .row {
        display: flex;
    }
</style>
