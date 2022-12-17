<script>
    export let x;
    export let y;
</script>

<svg x={x} y={y} width="200" height="100">
    <rect
        width="200"
        height="100"
        fill="#DDDADB"
        rx="15"
    />
    <text  dominant-baseline="middle" text-anchor="middle"
        >
        <tspan  dy=".6em">tspan line 1</tspan>
        <tspan  dy="1.2em">tspan line 2</tspan>
        <tspan  dy="1.2em">tspan line 3</tspan>
        </text
    >
</svg>

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
        fill: var(--greyMaxEmp);
    }

</style>
