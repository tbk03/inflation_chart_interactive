<script>
    // ---------------------------------------------------------------------
    // enable parameters to be passed into component
    // ---------------------------------------------------------------------

    // dimensions of hover area
    export let xScale;
    export let margin;
    export let width;
    export let height;

    // for locating tooltip
    export let hoveredDate;
    export let maxDate;

    // ---------------------------------------------------------------------
    // Handle hover events
    // ---------------------------------------------------------------------
    // hoveredDate is updated in App.svelte too through bindings

    const handleMousemove = function (e) {
        hoveredDate = xScale.invert(e.offsetX - margin.left);
    };

    const handleMouseleave = function () {
        hoveredDate = maxDate;
    };
</script>

<!-- transparent hover area over chart -->
<rect
    class="hover-listener"
    fill="transparent"
    {width}
    {height}
    on:mousemove={handleMousemove}
    on:mouseleave={handleMouseleave}
/>

<!-- vertical line which forms part of tooltip -->
<line
    x1={xScale(hoveredDate)}
    x2={xScale(hoveredDate)}
    y1={25}
    y2={height}
    stroke="#666666"
    stroke-dasharray="2, 2"
    pointer-events="none"
/>

<style>
    rect {
        cursor: crosshair;
    }
</style>
