<script>
    // ---------------------------------------------------------------------
    // imports
    // ---------------------------------------------------------------------

    // bespoke, local components
    import MilkBottle from "./MilkBottle.svelte";

    // ---------------------------------------------------------------------
    // enable parameters to be passed into component
    // ---------------------------------------------------------------------

    // for tooltip location
    export let xScale;
    export let yScale;
    export let hoveredDate;

    // data to populate tooltip values
    export let data;

    // styling
    export let color;
    export let secondColor;

    // ---------------------------------------------------------------------
    // component logic
    // ---------------------------------------------------------------------

    // get inflation rate at a given date
    const getYValue = (date) =>
        data.filter((d) => new Date(d.date) >= date)[0]?.cpi;

    // get milk price at a given date
    const getMilkPrice = (date) =>
        data.filter((d) => new Date(d.date) >= date)[0]?.price_pence;

    // format data for presentation in tooltip
    function oneDecimalPlace(x) {
        return Number.parseFloat(x).toFixed(1);
    }
</script>

<!-- CPI TOOL TIP - MAIN PLOT -->
<circle
    cx={xScale(hoveredDate)}
    cy={yScale(getYValue(hoveredDate))}
    r="7.5"
    fill={color}
    stroke="#f0f0f0"
    pointer-events="none"
/>
<text
    x={xScale(hoveredDate)}
    dx="12"
    y={yScale(getYValue(hoveredDate))}
    pointer-events="none"
    fill={color}
    font-weight="bold"
    font-size="1.2rem"
    stroke="#EEECED"
    stroke-width="7"
    paint-order="stroke"
>
    {oneDecimalPlace(getYValue(hoveredDate))} %
</text>

<!-- MILK PRICE TOOL TIP - TOP MARGIN -->

<text
    x={xScale(hoveredDate)}
    dx="20"
    y={-10}
    pointer-events="none"
    fill={secondColor}
>
    {Math.round(getMilkPrice(hoveredDate))}p
    <tspan x={xScale(hoveredDate)} dx="20" dy="1.1em">per pint</tspan>
    <tspan x={xScale(hoveredDate)} dx="20" dy="1.1em">of milk</tspan>
</text>
<g pointer-events="none">
    <MilkBottle x={xScale(hoveredDate) - 25} y={-25} colour={secondColor} />
</g>

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
</style>
