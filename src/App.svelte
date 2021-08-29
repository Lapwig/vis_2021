<script>
	import Papa from 'papaparse';
	import ScatterPlot from './ScatterPlot.svelte';
	import LineChart from './LineChart.svelte';
	//import scaleLinear from 'd3-scale';


	let scatterPlotData = [];
	Papa.parse("count_2.csv", {
	  header: true,
	  download: true,
	  complete: function(results) {
		scatterPlotData = results.data
	  }
	})

	let datapoints = [];
	Papa.parse("fatal-police-shootings-data.csv", {
	  header: true,
	  download: true,
	  complete: function(results) {
		datapoints = results.data
	  }
	})


	$: console.log(datapoints)
	const rescale = function(x, domain_min, domain_max, range_min, range_max) {
    	return ((range_max - range_min)*(x-domain_min))/(domain_max-domain_min) + range_min
  	}
</script>
<main>
	initial attempt
    <ScatterPlot data={scatterPlotData}/>
    
</main>

//<LineChart data={scatterPlotData} />



<style>
	main {
		display: flex;
		flex-direction: column;
		align-items: center;
		max-width: 48rem;
		margin: 0 auto;
	}
	svg {
	  background-color: whitesmoke;
	}

	circle {
	  opacity: 0.3;
	  fill: steelblue;
	}
	circle.violence {
	fill:red;
	}
  </style>





<svg width=800 height=400>
	{#each datapoints as datapoint}
	  <circle cx={rescale(datapoint.longitude, -170, 70, 0, 800)}
			  cy={rescale(datapoint.latitude, -40, 70, 400, 0)}
			  r=3 
			  class:violence={datapoint.threat_level == 'attack'}/>
	{/each}
  </svg>