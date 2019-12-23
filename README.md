# D3 Graph Visualisation Performance in Svelte

This project contains Svelte JS versions of the [D3 Force Directed Graph example](https://observablehq.com/@d3/force-directed-graph)
modified to allow testing on networks of arbitrary size. Different ways of 
implementing the graph are included so that their performance can be
compared.

The implementations are based on [d3-fdg-svelte](https://github.com/theWebalyst/d3-fdg-svelte) repository (commit [1e23a49](https://github.com/theWebalyst/d3-fdg-svelte/commit/bc982a59d9b1fa9340c4680615dddda2066bf182)). If you want
to understand the approach and contrast their implementations, that is the place to look!

The approaches compared use:
1. `SVG` elements created/updated by D3
2. `SVG` elements created/updated by Svelte
3. `canvas` with D3 hit detection
4. `canvas` with a second context for hit detection

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
