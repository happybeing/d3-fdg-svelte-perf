<script>
	import { writable } from 'svelte/store';

	import GraphSvg from './NetworkGraph.svelte';
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

	let visualisation;
	let visualisationOptions = [
		{ text: `Force Graph (SVG)`, type: 'graphSvg' },
		{ text: `Force Graph (Canvas + d3 find)`, type: 'graphCanvas' },
		{ text: `Force Graph (Canvas + id Context)`, type: 'graphCanvasIdContext' }
	];

	$: makeDataSource(dataSource);

	let resettingChart = false; 
	function makeDataSource (input) {
		let data;
		if (input && input.type === 'json-data') data = makeUsingJsonData(input.source);
		if (input && input.type === 'ngraph.generator') data = makeUsingNgraphGenerator(input.source)
		graph.update(() => data);
	}

	// Json Input Data
	function makeUsingJsonData (jsonData) {
		return jsonData;
	}

	// Graph generator
	function makeUsingNgraphGenerator (generator) {
		let g = generator.ladder(10);
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

<form>
	<label>Type:</label>
	<select bind:value={visualisation} title='Type'>
		{#each visualisationOptions as option}
			<option value={option}>
				{option.text}
			</option>
		{/each}
	</select>

	<label>Choose Source:</label>
	<select bind:value={dataSource} title='Type'>
		{#each dataOptions as option}
			<option value={option}>
				{option.text}
			</option>
		{/each}
	</select>
</form>	

{#if $graph}
	<div class="chart">
		{#if visualisation.type === 'graphSvg'}
			<GraphSvg {graph}/>
		{:else if visualisation.type === 'graphCanvas'}
			<GraphCanvas {graph}/>
		{:else if visualisation.type === 'graphCanvasIdContext'}
			<GraphCanvasIdContext {graph}/>
		{/if}
	</div>
{/if}
