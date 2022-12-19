<script>
  // --------------------------------------------------------------------------------------
  // Imports
  // --------------------------------------------------------------------------------------

  // data
  import milk_cpi from "./data/milk_cpi.json";

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
  // Scales (from data values to pixels)
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

  // --------------------------------------------------------------------------------------
  // Misc plot parameters
  // --------------------------------------------------------------------------------------
  const cpiColour = "#C94A54";
  const milkColour = "#737373";

  // sets tooltip to not show by default
  let hoveredDate = maxDate;
</script>

<div class="container">
  <!-- EXPLANATORY TEXT -->
  <h1 style="text-align:center">UK Inflation and the Price of Milk</h1>
  <p class="author"><b>By Chris Martin</b></p>
  <p>
    This interactive chart was produced as part of a personal project
    completed in 2022. In the project I explored and developed novel ways to
    visualise data on some of the alarming economic trends in the UK.
  </p>
  <p>
    In this interactive chart I wanted to help viewers make connections between
    the rising (but fairly abstract) rate of inflation, and some real world
    impacts. The chart is intended to be viewed and explored by a general
    audience. People without a particular interest or background in finance or
    economics.
  </p>
  <p>
    I started by producing hand-drawn sketches of chart ideas. Having settled on
    a core idea, I developed the chart using web technologies including
    Javascript, html, css, D3 and Svelte . Throughout the design and development
    process I tested ideas with members of the target audience.
  </p>
  <p>
    The finished chart and text allow the viewer to explore the increases in
    inflation over the last two years. As they explore, the viewer can put the
    inflation rate in context as they also notice how the price of a household
    stable (milk) has changed too.
  </p>

  <!-- THE CHART -->
  <div class="outer">
    <div class="chart-container" bind:clientWidth={width}>
      <svg
        {width}
        {height}
        aria-labelledby="chart-title"
        aria-describedby="chart-description"
        role="img"
      >
        <!-- for accessiblity  -->
        <desc id="chart-description"
          >An interative line chart showing the inflation rate in the UK
          (2020-2022). The price of a pint of milk is shown in the tooltip for
          some extra real world context</desc
        >
        <!-- move elements within chart margins -->
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

          <!-- THE LINE  -->
          <Line
            {xScale}
            {yScale}
            data={milk_cpi}
            color={cpiColour}
            {hoveredDate}
          />

          <!-- TOOLTIP  -->
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

      <!-- ANNOTATION -->
      {#if hoveredDate === maxDate && width > 500}
        <Annotation
          x={xScale(new Date("2020-02-15")) + margin.left}
          y={yScale(12) + margin.top}
          text="<p>If you hover over the chart you will 
              see the inflation rate (on the chart) and the price of a pint of milk (above the chart).</p><br/>
              <p>The price of the pint of milk is there for a little extra context.
                It illustrates how prices have changed for just one household staple.</p><br/>
                The prices of different products change at different rates not neccesarily equal to the overall inflation rate.
                
              </p>"
        />
      {/if}
    </div>
    <br/><br/>
    <p class="sources" style="text-align:right;">
      <b>Data sources:</b>
      <a
        href="https://www.ons.gov.uk/economy/inflationandpriceindices/timeseries/d7g7/mm23"
        >CPI inflation rate,</a
      >
      <a
        href="https://www.ons.gov.uk/economy/inflationandpriceindices/timeseries/cznt/mm23"
        >Milk prices.</a
      >
    </p>
  </div>
  <p id="last-element" />
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:b&display=swap");
  @import url("https://fonts.googleapis.com/css2?family=Lato:b&display=swap");

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
    margin-top: 3rem;
    margin-bottom: 1rem;
  }

  h1 {
    font-family: "Poppins", sans-serif;
    color: #404040;
    font-size: 3rem;
    padding-bottom: 3rem;
    padding-top: 3rem;
    font-weight: bold !important;
    text-transform: uppercase;
  }

  p {
    font-family: "Poppins", sans-serif;
    color: #404040;
    font-size: 1.1rem;
    margin-bottom: 1rem;
    padding-left: 6%;
    padding-right: 6%;
    line-height: 1.3rem;
  }

  #last-element {
    padding-bottom: 200px;
  }

  b {
    font-weight: bold !important;
  }

  * {
    -webkit-font-smoothing: antialiased;
  }

  .sources {
    font-family: "Lato", sans-serif;
    font-size: 1rem;
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
