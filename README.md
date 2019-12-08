# D3 Performance of Graph Visualisation in Svelte

**STATUS: this is work in progress, not yet working (December 2019)!**
This project contains Svelte JS versions of the [D3 Force Directed Graph example](https://observablehq.com/@d3/force-directed-graph)
modified to allow testing on networks of arbitrary size. Three different ways of 
implementing the graph are included so that their performance can be
compared.

The implementations are based on [d3-fdg-svelte](https://github.com/theWebalyst/d3-fdg-svelte) repository (commit [1e23a49](https://github.com/theWebalyst/d3-fdg-svelte/commit/1e23a49a3203a5228afbffec66adff67304665b7)). If you want
to understand the approach and contrast their implementations, that is the place to look!

The approaches compared use:
1. `SVG` elements
2. `canvas` with D3 hit detection
3. `canvas` with a second context for hit detection

These can be tested with different sizes and topographies created with [ngraph](https://github.com/anvaka/ngraph) generators.

## Get started
This repository was based on the default [sveltejs/template](https://github.com/sveltejs/template), so refer to that for more information. 
Note though, I have modified it to use `yarn` rather than `npm`, so the 
essential commands are given below.

> *Note that you will need to have [Node.js](https://nodejs.org) and `yarn` installed.*

Get the code...
```bash
git clone https://github.com/theWebalyst/d3-fdg-svelte-perf
```

Install the dependencies...

```bash
cd d3-fdg-svelte-perf
yarn
```

...then start [Rollup](https://rollupjs.org):

```bash
yarn dev
```

Navigate to [localhost:5000](http://localhost:5000). You should see the app running.

## More information 
For more information about the template used to create this app, see the README at https://github.com/sveltejs/template.
