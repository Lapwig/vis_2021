<script>
	import { scaleTime, scaleLinear } from 'd3-scale';
	import { extent, max } from 'd3-array';
	import { line } from 'd3-shape';

	import Axis from './Axis.svelte';

	export let data;

	const height = 400;
	const margin = 40;

	let width;

	$: xScale = scaleLinear()
		.domain([0, max(data, d => +d.day_count)])
		.range([margin, width - margin]);

	$: yScale = scaleLinear()
        .domain(extent(data, d => d.shooting))
		.range([height - margin, margin]);

	$: lineGenerator = line()
		.x(d => xScale(+d.day_count))
		.y(d => yScale(d.shooting));

	const reveal = (node, { duration }) => {
		if (!xScale || !yScale) return;
		const length = node.getTotalLength();
		node.style.strokeDasharray = length;
		return {
			duration,
			css: (t, u) => `stroke-dashoffset: ${u * length}`
		};
	}
</script>

<div class='line-chart' bind:clientWidth={width}>
	{#if width}
		<svg width={width} height={height}>
			<Axis {width} {height} {margin} scale={xScale} position='bottom' />
			<Axis {width} {height} {margin} scale={yScale} position='left' />
			<path
				d={lineGenerator(data)}
				stroke='rebeccapurple'
				stroke-width={2}
				stroke-linecap='round'
				fill='none'
				in:reveal={{ duration: 3000 }}
			/>
		</svg>
	{/if}
</div>

<style>
	div {
		width: 100%;
	}
</style>