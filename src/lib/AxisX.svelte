<script>
    export let height;
    export let width;
    export let xScale;
    export let hoveredDate;
    export let isUnhovered;

    // Format our ticks as short date strings
    import { timeFormat } from "d3-time-format";
    const dateFormat = timeFormat("%b %y");

    const TICK_LENGTH = 6;

    $: ticks = isUnhovered ? xScale.ticks(6) : [hoveredDate];
</script>

<!-- AXIS BASELINE -->
<line class="axis-baseline" x1={0} y1={height} x2={width} y2={height} />

<!-- AXIS TICKS -->
{#each ticks as tick, i}
    {#if i % 2 == 0}
        <line
            class="major-grid"
            x1={xScale(tick)}
            x2={xScale(tick)}
            y1={height}
            y2={height + TICK_LENGTH}
        />
        <text
            class = "axis-text"
            x={xScale(tick)}
            y={height + TICK_LENGTH}
            dy="6"
            dominant-baseline="hanging"
        >
            {dateFormat(tick)}
        </text>
    {/if}

    {#if i % 2 != 0 && width > 700}
        <line
            class="minor-grid"
            x1={xScale(tick)}
            x2={xScale(tick)}
            y1={height}
            y2={height + TICK_LENGTH}
        />
        <text
            class = "axis-text"
            x={xScale(tick)}
            y={height + TICK_LENGTH}
            dy="6"
            dominant-baseline="hanging"
        >
            {dateFormat(tick)}
        </text>
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
        text-anchor: middle;
    }

    .axis-baseline {
        stroke: var(--greyMaxEmp);
        stroke-width: 1.5;
    }

    .major-grid{
        stroke: var(--greyHighEmp);
        stroke-width: 1;
    }

    .minor-grid{
        stroke: var(--greyLowEmp);
        stroke-width: 0.75;
    }

    .axis-text {
        font-size: 1.1rem;
        fill: var(--greyHighEmp);
    }

    @media (max-width: 600px) {
        .axis-text {
        font-size: 1rem;
    }
}

</style>
