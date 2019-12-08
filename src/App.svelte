<script>
	import GraphSvg from './NetworkGraph.svelte';
	import GraphCanvas from './NetworkGraphCanvas.svelte';
	import GraphCanvasIdContext from './NetworkGraphCanvasIdContext.svelte';
	
	import data from './data.js';

	import generator from 'ngraph.generators'
	let data2 = generator.ladder(10);

	let questions = [
		{ id: 1, text: `Force Graph (SVG)`, type: 'graphSvg' },
		{ id: 2, text: `Force Graph (Canvas + d3 find)`, type: 'graphCanvas' },
		{ id: 3, text: `Force Graph (Canvas + id Context)`, type: 'graphCanvasIdContext' }
	];

	let selected;

	let answer = '';

	function handleSubmit() {
		alert(`chose ${selected.id} (${selected.text}) with "${selected.type}"`);
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
	<select bind:value={selected} on:change="{() => answer = ''}" title='Type'>
		{#each questions as question}
			<option value={question}>
				{question.text}
			</option>
		{/each}
	</select>
	<button type=submit>
		Visualise
	</button>
</form>	
<div class="chart">
	{#if selected && selected.type === 'graphSvg'}
	<GraphSvg graph={data2}/>
	{:else if selected && selected.type === 'graphCanvas'}
	<GraphCanvas graph={data2}/>
	{:else}
	<GraphCanvasIdContext graph={data2}/>
	{/if}
</div>