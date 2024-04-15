<script lang="ts">
    import type { StravaActivity } from '$lib/activities.js'
    import ChartContainer from '$lib/components/charts/chart-primitive.svelte'
    import * as Plot from "@observablehq/plot";
    export let activities: StravaActivity[];

    let chart: HTMLDivElement;

    $: if (activities && activities[0] && chart) {
        chart.innerHTML = '';
        chart?.append( // @ts-ignore
            Plot.rectY(
                activities, 
                Plot.binX(
                    {y: "count" },
                    {x: (d) => Math.sqrt(d.distance || NaN)}
                )
            ).plot({x: {label: "Distance", transform: (x) => `${(x*x) / 1000} km`},}
            )
        );
        chart?.append(
            Plot.rect(
                activities,
                Plot.bin({fill: "count"},
                {
                    x: (d) => Math.sqrt(d.distance || NaN), 
                    y: (d) => Math.sqrt(d.moving_time || NaN)
                })
            ).plot(
                {
                    color: {
                        type:"threshold", 
                        domain: [2, 5, 10, 20, 50, 100], 
                        scheme: "plasma", 
                        legend: true
                    }
                }
            ));

        chart?.append(
            Plot.dot(activities, {
                x: (d) => Math.sqrt(d.distance || NaN),
                y: (d) => Math.sqrt(d.moving_time || NaN),
                stroke: (d) => d.type,
                channels: {name: "name", distance: "distance", moving_time: "moving_time", type: "type", date: "start_date_local"},
                tip: true
            }).plot({color: { scheme: "plasma", legend: true}})
        )
    }
</script>
  
<ChartContainer>
    <svelte:fragment slot="options">
    </svelte:fragment>
    <svelte:fragment slot="title">
        <h2>Rides by </h2>
    </svelte:fragment>
    <svelte:fragment slot="chart">
        <div bind:this={chart}>
        </div>
    </svelte:fragment>
</ChartContainer>