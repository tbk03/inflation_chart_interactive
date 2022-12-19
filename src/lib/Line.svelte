<script>
    // ---------------------------------------------------------------------
    // imports
    // ---------------------------------------------------------------------

    // d3 libraries
    import { line } from "d3-shape";

    // ---------------------------------------------------------------------
    // enable parameters to be passed into component
    // ---------------------------------------------------------------------

    // core data for drawing the line
    export let data;

    // plot dimensions
    export let xScale;
    export let yScale;

    // styling / aesthetics
    export let color;

    // for highlighting the line before/after the tooltip
    export let hoveredDate;

    // ---------------------------------------------------------------------
    // component logic
    // ---------------------------------------------------------------------

    // for highlighting the line before/after the tooltip
    $: dataBeforeHover = data.filter((d) => new Date(d.date) <= hoveredDate);

    // create the line from the data
    $: lineGenerator = line()
        .x((d) => xScale(new Date(d.date)))
        .y((d) => yScale(d.cpi));
</script>

<!-- Lower emphasis on the line after the tooltip -->
<path
    d={lineGenerator(data)}
    stroke={color}
    fill="transparent"
    stroke-width="4"
    opacity=".35"
/>

<!-- Highlight the line before the tooltip -->
<path
    d={lineGenerator(dataBeforeHover)}
    stroke={color}
    fill="transparent"
    stroke-width="4"
/>
