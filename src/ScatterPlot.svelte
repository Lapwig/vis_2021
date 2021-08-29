<script>
	import { scaleLinear, scaleTime, scaleSqrt } from 'd3-scale';
	import { extent } from 'd3-array';
	import { select } from 'd3-selection';

	import Axis from './Axis.svelte';

	export let data;

	const height = 500;
	const margin = 40;

	let width;

	$: xScale = scaleLinear()
		.domain(extent(data, d => new Date(+d.month)))
		.range([margin, width - margin])
        ;

	$: yScale = scaleLinear()
		.domain(extent(data, d => d.year))
		.range([height - margin, margin]);

	$: radiusScale = scaleSqrt()
		.domain(extent(data, d => d.shooting))
		.range([0, 30]);

	const reveal = (node, { duration }) => {
		const radius = select(node).attr('r');
		return {
			duration,
			tick: (t) => select(node).attr('r', t * radius)
		};
	}
</script>

<div class='scatter-plot' bind:clientWidth={width}>
	{#if width}
		<svg width={width} height={height}>
			<Axis {width} {height} {margin} scale={xScale} position='bottom' />
			<Axis {width} {height} {margin} scale={yScale} position='left' />
			{#each data as d}
				<circle
					cx={xScale(+d.month)}
					cy={yScale(d.year)}
					r={radiusScale(d.shooting)}
					fill='grey'
					in:reveal={{ duration: 10000 }}
				/>
			{/each}
		</svg>
	{/if}
</div>

<style>
	div {
		width: 100%;
	}

	circle {
		opacity: 0.5;
	}
</style>