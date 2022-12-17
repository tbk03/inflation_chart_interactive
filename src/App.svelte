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

  const margin = { top: 30, right: 50, bottom: 30, left: 60 };

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

<div class="outer">
  <div class="chart-container" bind:clientWidth={width}>
    <h1>Hello</h1>
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
        3rd, Biden has an 89 in 100 chance of winning, and Trump has 10 in 100.</desc
      >
      <g transform="translate({margin.left} {margin.top})">
        <!-- Y AXIS -->
        <AxisY width={innerWidth} height={innerHeight} {yScale} {hoveredDate} />

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

        <!-- annotation -->
        <Annotation
          x="100"
          y="100"/>
      </g>
    </svg>
  </div>
</div>

<style>
  .outer {
    padding: 15px;
    background: #EEECED;
    box-shadow: 2px 2px 6px 0 rgba(0, 0, 0, 0.15);
    border-radius: 3px;
  }
  h1 {
    font-size: 28px;
    line-height: 34px;
    color: #222;
    margin-bottom: 10px;
    text-align: center;
    font-weight: 600;
  }
</style>
