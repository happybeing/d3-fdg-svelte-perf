<script>
	import GraphSvg from './NetworkGraph.svelte';
	import GraphCanvas from './NetworkGraphCanvas.svelte';
	import GraphCanvasIdContext from './NetworkGraphCanvasIdContext.svelte';
	
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

	function handleSubmit () {
		alert(`(${visualisation.text}) with "${visualisation.type}"`);
	}

	$: graph = makeDataSource(dataSource);

	function makeDataSource (input) {
		if (input && input.type === 'json-data') return makeUsingJsonData(input.source);
		if (input && input.type === 'ngraph.generator') return makeUsingNgraphGenerator(input.source)
		return undefined;
	}

	// Json Input Data
	function makeUsingJsonData (jsonData) {
		return jsonData;
	}

	// Graph generator
	function makeUsingNgraphGenerator (generator) {
		let graph = generator.ladder(10);
		return { nodes: makeNodes(graph), links: makeLinks(graph) }
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
	input { display: block; width: 1000px; max-width: 100%; }
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

<form on:submit|preventDefault={handleSubmit}>
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
	<button type=submit>
		Visualise
	</button>
</form>	
<div class="chart">
	{#if dataSource && visualisation && visualisation.type === 'graphSvg'}
	<GraphSvg graph={graph}/>
	{:else if dataSource && visualisation && visualisation.type === 'graphCanvas'}
	<GraphCanvas graph={graph}/>
	{:else if dataSource && visualisation && visualisation.type === 'graphCanvasIdContext'}
	<GraphCanvasIdContext graph={graph}/>
	{/if}
</div>