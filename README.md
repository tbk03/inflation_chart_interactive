# An interactive chart showing UK inflation rates (2020-2022): Svelte + D3
Chris Martin

This is an interative chart showing UK inflation rates (2020-2022). You can see the chart in action [here](https://tbk03.github.io/inflation_chart_interactive/).  

The code here adapts, and develops on, Connor Rothchild's [Svelte + D3 chart template](https://github.com/connorrothschild/svelte-d3-iib-workshop).

## Repo structure

* `/src/App.svelte`: The main page which includes the chart.
* `/src/lib/*`: The components used in the chart (axis, tooltips etc.).
* `/src/data/milk_cpi.json`: The data shown in the chart.

## Running the code

### 1. Clone the repo

In your terminal, run the following command:

```bash
git clone https://github.com/tbk03/inflation_chart_interactive.git
```

Then, `cd` into that directory.

### 2. Install dependencies

Run `npm install` or `pnpm install` to install the dependencies.

### 3. Start the development server

Run `npm run dev` to start the development server.
