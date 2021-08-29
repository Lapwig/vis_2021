<script>
    export let datapoints = []

    $: scaleColor= scaleLinear().domain().range(["green","brown"])
	const lineGenerator = function(d){
		return	"M" + d.race + "0" +
				"M" + d.gender + "100" +
	 			"M" + d.signs_of_mental_illness + "200" 
	}
</script>
<style>
    path {
		stroke: blue;
		fill: none;
		opacity: 0.2;
	}
	path:hover{
		stroke: red;
		stroke-opacity: 0.8;
	}
</style>


<svg width=200 height=400>
    {#each [0,100,200,300,400] as y}
        <line x1=0 x2=200 y1={y} y2={y} /> 
    {/each}
    {#each datapoints as datapoint}
        <path d={lineGenerator(datapoint)}
            style="stroke: {scaleColour(datapoint.race)}"/>
    {/each}
</svg>