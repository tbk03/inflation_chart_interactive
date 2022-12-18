<script>
  import data from "./data/data.json";
  // --------------------------------------------------------------------------------------
  // Imports
  // --------------------------------------------------------------------------------------

  // data
  import milk_cpi from "./data/milk_cpi.json";
  console.log(milk_cpi);

  // libraries
  import { scaleLinear, scaleTime } from "d3-scale";

  // bespoke components
  import Line from "./lib/Line.svelte";
  import AxisX from "./lib/AxisX.svelte";
  import AxisY from "./lib/AxisY.svelte";
  import HoverEvents from "./lib/HoverEvents.svelte";
  import Tooltip from "./lib/Tooltip.svelte";
  import Annotation from "./lib/Annotation.svelte";

  // --------------------------------------------------------------------------------------
  // Set up the structure of the plot
  // --------------------------------------------------------------------------------------

  const margin = { top: 30, right: 50, bottom: 30, left: 50 };

  let height = 600;
  let width = 400;

  let innerHeight = height - margin.top - margin.bottom;
  $: innerWidth = width - margin.left - margin.right;

  // --------------------------------------------------------------------------------------
  // Accessors
  // --------------------------------------------------------------------------------------

  let xAccessor = (d) => d.date;
  let yAccessor = (d) => d.cpi;
  let milkPriceAccessor = (d) => d.price_pence;

  // --------------------------------------------------------------------------------------
  // Scales
  // --------------------------------------------------------------------------------------

  const cpiMax = 13;
  const yScale = scaleLinear()
    .domain([0, cpiMax]) // INPUT
    .range([innerHeight, 0]); // OUTPUT

  // x axis max and min
  const minDate = new Date(xAccessor(milk_cpi[0]));
  const maxDate = new Date(xAccessor(milk_cpi[milk_cpi.length - 1]));

  $: xScale = scaleTime()
    .domain([minDate, maxDate]) // INPUT
    .range([0, innerWidth]); // OUTPUT

  const cpiColour = "#C94A54";
  const milkColour = "#374E83";

  let hoveredDate = maxDate;
</script>

<div class="container">
  <h1 style="text-align:center">Inflation chart</h1>
  <div class="outer">
    <div class="chart-container" bind:clientWidth={width}>
      <svg
        {width}
        {height}
        aria-labelledby="chart-title"
        aria-describedby="chart-description"
        role="img"
      >
        <!-- <title id="chart-title">UK inflation 2020-2022</title> -->
        <desc id="chart-description"
          >A dual line chart showing Donald Trump and Joe Biden's likelihood of
          electoral victory diverging over time. At the final point, on November
          3rd, Biden has an 89 in 100 chance of winning, and Trump has 10 in
          100.</desc
        >
        <g transform="translate({margin.left} {margin.top})">
          <!-- Y AXIS -->
          <AxisY
            width={innerWidth}
            height={innerHeight}
            {yScale}
            {hoveredDate}
          />

          <!-- X AXIS -->
          <AxisX
            width={innerWidth}
            height={innerHeight}
            {xScale}
            {hoveredDate}
            isUnhovered={hoveredDate === maxDate}
          />

          <!-- MY DATA  -->
          <Line
            {xScale}
            {yScale}
            data={milk_cpi}
            color={cpiColour}
            {hoveredDate}
          />

          <HoverEvents
            width={innerWidth}
            height={innerHeight}
            {xScale}
            {margin}
            {maxDate}
            bind:hoveredDate
          />

          <!-- only show tooltip if hovered -->
          {#if hoveredDate !== maxDate}
            <Tooltip
              {hoveredDate}
              {xScale}
              {yScale}
              data={milk_cpi}
              color={cpiColour}
              secondColor={milkColour}
            />
          {/if}
        </g>
      </svg>

      <!-- annotation -->
      {#if hoveredDate === maxDate && width > 500}
        <Annotation
          x={xScale(new Date("2020-02-15")) + margin.left}
          y={yScale(12) + margin.top}
          text="<p>If you hover over the chart you will 
              see the inflation rate (on the chart) and the price of a pint of milk (above the chart).</p><br/>
              <p>The price of the pint of milk is there for a little extra context.
                It illustrates how prices have changed for just one household staple.</p><br/>
                The prices of different products change at different rates not neccesarily equal to the over inflation rate.
                
              </p>"
        />
      {/if}
    </div>
  </div>
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins&display=swap");

  .container {
    max-width: 900px;
    min-width: 400px;
    margin-left: auto;
    margin-right: auto;
    padding-left: 6%;
    padding-right: 6%;
  }

  .outer {
    padding: 15px;
    background: #eeeced;
    box-shadow: 2px 2px 6px 0 rgba(0, 0, 0, 0.15);
    border-radius: 3px;
  }

  h1 {
    font-family: "Poppins", sans-serif;
    color: #404040;
    font-size: 3.5rem;
  }

  /* test branch protection */
  /* h2,
  h3,
  p,
  li,
  tbody */

  /* h2 {
    font-size: 1.8rem;
    line-height: 1.1;
    padding-bottom: 8px;
    font-weight: bold;
  }
  .h2-body {
    margin-top: 4rem;
    margin-bottom: 1rem;
    border-bottom: solid 2px #737373;
    text-align: left;
  }
  h3 {
    font-size: 1.2rem;
    text-align: left;
    line-height: 1.5;
    font-weight: normal;
    margin-bottom: 1.5rem;
    margin-top: 1.5rem;
  }
  a {
    color: #355070;
  }
  #last-element {
    padding-bottom: 200px;
  } */
</style>
