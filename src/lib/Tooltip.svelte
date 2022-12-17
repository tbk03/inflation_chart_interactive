<script>
    // bespoke components
    import MilkBottle from "./MilkBottle.svelte";

    export let xScale;
    export let yScale;
    export let hoveredDate;
    export let data;
    export let color;

    const getYValue = (date) =>
        data.filter((d) => new Date(d.date) >= date)[0]?.cpi;

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
    stroke="#f0f0f0"
    stroke-width="5"
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
    fill={color}
    stroke="#f0f0f0"
    stroke-width="5"
    paint-order="stroke"
>
    {Math.round(getMilkPrice(hoveredDate))}p
    <tspan x={xScale(hoveredDate)} dx="20" dy="1.1em">per pint</tspan>
    <tspan x={xScale(hoveredDate)} dx="20" dy="1.1em">of milk</tspan>
</text>
<g pointer-events="none">
    <MilkBottle 
        x={xScale(hoveredDate) - 25}
        y={-25}
        colour="blue"/>
</g>

<style>
    text {
        font-weight: 600;
    }
</style>
