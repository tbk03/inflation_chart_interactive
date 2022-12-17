<script>
    export let height;
    export let width;
    export let yScale;
    export let hoveredDate;
</script>

<!-- AXIS LABEL -->
<!-- hide if tooltip overlaps -->
{#if hoveredDate > new Date("2020-04-01")}
    <text class="axis-label" dx="-34" font-weight="bold">Inflation rate</text>
{/if}

<line class="axis-baseline" x1={0} y1={height} x2={0} y2={30} />

<!-- AXIS TICKS -->
{#each yScale.ticks(5) as tick, i}
    <text
        class="axis-text"
        x="0"
        dx="-12"
        y={yScale(tick)}
        text-anchor="end"
        dominant-baseline="middle"
        fill="#999"
    >
        {tick}{#if tick == 12}%{/if}
    </text>

    {#if i % 2 == 0}
        <line
            class="major-grid"
            x1="-6"
            x2={width}
            y1={yScale(tick)}
            y2={yScale(tick)}
            stroke="#999"
        />
    {:else}
        <line
            class="minor-grid"
            x1="-6"
            x2={width}
            y1={yScale(tick)}
            y2={yScale(tick)}
            stroke="#999"
        />
    {/if}
{/each}

<style>
    /* axis styling */
       @import url("https://fonts.googleapis.com/css2?family=Lato&display=swap");
    
    /* set component colours*/
    :root {
        --greyMaxEmp: #666666;
        --greyHighEmp: #737373;
        --greyLowEmp: #bfbfbf;
        --greyMinEmp: #e5e5e5;
    }

    text {
        font-family: "Lato", sans-serif;
    }

    .axis-label {
        font-weight: bold;
        font-size: 1.2rem;
    }

    .axis-text {
        font-size: 1rem;
        fill: var(--greyHighEmp);
    }

    .major-grid{
        stroke: var(--greyLowEmp);
        stroke-width: 0.5;
    }

    .minor-grid{
        stroke: var(--greyMinEmp);
        stroke-width: 0.5;
    }

    .axis-baseline {
        stroke: var(--greyLowEmp);
        stroke-width: 1.5;
    }
</style>
