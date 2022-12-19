<script>

    // ---------------------------------------------------------------------
    // import components
    // ---------------------------------------------------------------------
    import { fade } from "svelte/transition";

    // ---------------------------------------------------------------------
    // enable parameters to be passed into component
    // ---------------------------------------------------------------------

    // annotation positioning
    export let x;
    export let y;

    // annotation content
    export let text;

</script>



<div
    class="annotation"
    style="background-color: #DDDADB; top:{y}px; left:{x}px"
    in:fade={{ duration: 750 }}
    out:fade={{ duration: 750 }}>
    <p>{@html text}</p>
</div>

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

    p {
        font-family: "Lato", sans-serif;
        text-anchor: middle;
        color: var(--greyMaxEmp);
    }

    .annotation {
        position: absolute;
        padding: 20px;
        max-width: 200px;
        line-height: 1.15rem;
        pointer-events: none;
        border-radius: 25px;
    }

</style>
