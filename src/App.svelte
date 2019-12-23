<script>
	import { writable } from 'svelte/store';

	import GraphD3SVG from './NetworkGraphD3SVG.svelte';
	import GraphSvelteSVG from './NetworkGraphSvelteSVG.svelte';
	import GraphCanvas from './NetworkGraphCanvas.svelte';
	import GraphCanvasIdContext from './NetworkGraphCanvasIdContext.svelte';

	const graph = writable(0);
	
	import lesMisData from './data-les-miserables.js';

	import ngraphGenerator from 'ngraph.generators'
	let dataSource;
	let dataOptions = [
		{ text: 'Les Miserables (json)', type: 'json-data', source: lesMisData },
		{ text: 'Graph Generator', type: 'ngraph.generator', source: ngraphGenerator }
	];

	// ngraph.generator controls
	const minNodes = 1, maxNodes = 2000;

	let generatorNodes = minNodes;
	let generatorType;
	let generatorOptions = [
		{ text: 'Ladder', function: 'ladder' },
		{ text: 'Complete', function: 'complete' },
		{ text: 'Complete Bipartite', function: 'completeBipartite' },
		{ text: 'Binary Tree (balanced)', function: 'balancedBinTree' },
		{ text: 'Path', function: 'path' },
		{ text: 'Ladder (circular)', function: 'circularLadder' },
		{ text: 'Grid', function: 'grid' },
		{ text: '3D Grid', function: 'grid3' },
		{ text: 'Nodes (no links)', function: 'noLinks' },
		{ text: 'Circle Cliques (5)', function: 'cliqueCircle', param2: 5 },
		{ text: 'Watts Strogatz (4, 0.02)', function: 'wattsStrogatz', param2: 4, param3: 0.02 },
	];

	let visualisation;
	let visualisationOptions = [
		{ text: 'Force Graph (D3 SVG)', type: 'graphD3Svg' },	
		{ text: 'Force Graph (Svelte SVG)', type: 'graphSvelteSvg' },	
		{ text: 'Force Graph (Canvas + d3 find)', type: 'graphCanvas' },
		{ text: 'Force Graph (Canvas + id Context)', type: 'graphCanvasIdContext' }
	];

	$: makeDataSource(dataSource, generatorNodes, generatorType);

	let resettingChart = false; 
	function makeDataSource (input, genNodes, genType) {
		let data;
		if (input && input.type === 'json-data') data = makeUsingJsonData(input.source);
		if (input && input.type === 'ngraph.generator') data = makeUsingNgraphGenerator(input.source, genNodes, genType)
		graph.update(() => data);
	}

	// Json Input Data
	function makeUsingJsonData (jsonData) {
		return jsonData;
	}

	// Graph generator
	function makeUsingNgraphGenerator (generator, genNodes, genType) {
		const p1 = genNodes;
		const p2 = (genType.param2 ? genType.param2 : genNodes );
		const p3 = (genType.param3 ? genType.param3 : genNodes );
		let g = generator[genType.function].call(generator, p1, p2, p3);
		return { nodes: makeNodes(g), links: makeLinks(g) }
	}
	
	function makeLinks(g) {
		let links = [];
		g.forEachLink(function(link) {
			links.push({source: link.fromId, target: link.toId, value: 1});
			});
		return links;
	}

	function makeNodes(g) {
		let nodes = [];
		g.forEachNode(function(node) {
			nodes.push({id: node.id, group: 1});
			});
		return nodes;
	}

</script>

<style>
	.chart {
		width: 100%;
		max-width: 640px;
		height: calc(100% - 4em);
		min-height: 280px;
		max-height: 480px;
		margin: 0 auto;
	}
</style>

<h2>D3 Visualisation</h2>
<p>TODO: modify to use svelte:component (see <a href='https://svelte.dev/repl/svelte-component'>REPL</a>)</p>
	<label>Visualise Using:</label>
	<select bind:value={visualisation} title='Type'>
		{#each visualisationOptions as option}
			<option value={option}>
				{option.text}
			</option>
		{/each}
	</select>

	<label>Data Source:</label>
	<select bind:value={dataSource} title='Data Source'>
		{#each dataOptions as option}
			<option value={option}>
				{option.text}
			</option>
		{/each}
	</select>

	<label>Generate:</label>
	<select bind:value={generatorType} disabled={dataSource && dataSource.type !== 'ngraph.generator'} title='Genertor Type'>
		{#each generatorOptions as option}
			<option value={option}>
				{option.text}
			</option>
		{/each}
	</select><label >Nodes: <input disabled={dataSource && dataSource.type !== 'ngraph.generator'} type=number bind:value={generatorNodes} min={minNodes} max={maxNodes}>
		<input disabled={dataSource && dataSource.type !== 'ngraph.generator'} type=range bind:value={generatorNodes} min={minNodes} max={maxNodes}>
	</label>

{#if $graph}
	<div class="chart">
		{#if visualisation.type === 'graphD3Svg'}
			<GraphD3SVG {graph}/>
		{:else if visualisation.type === 'graphSvelteSvg'}
			<GraphSvelteSVG {graph}/>
		{:else if visualisation.type === 'graphCanvas'}
			<GraphCanvas {graph}/>
		{:else if visualisation.type === 'graphCanvasIdContext'}
			<GraphCanvasIdContext {graph}/>
		{/if}
	</div>
{/if}
