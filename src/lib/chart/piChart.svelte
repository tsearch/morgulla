<script lang="ts">
	import { onMount } from 'svelte';
	import { COLORS, percentageFormatter } from '$lib/chart/Util';
	import { Chart, registerables } from 'chart.js/dist/chart.esm';
	import ChartDataLabels from 'chartjs-plugin-datalabels';

	import type { Dataset } from '$lib/chart/dataset';

	Chart.register(...registerables);

	export let chartTitle: string;
	export let labels: string[];
	export let dataset: Dataset;

	let portfolio;
	const config = {
		plugins: [ChartDataLabels],
		type: 'pie',
		data: {
			labels: labels,
			datasets: [
				{
					label: dataset.name,
					data: dataset.data,
					backgroundColor: COLORS
				}
			]
		},
		options: {
			borderRadius: '10',
			plugins: {
				title: {
					display: true,
					text: chartTitle
				},
				datalabels: {
					formatter: percentageFormatter,
					color: '#000',
					display: 'auto',
				}
			},
			responsive: true
		}
	};
	onMount(() => {
		const ctx = portfolio.getContext('2d');
		// Initialize chart using default config set
		var monthChart = new Chart(ctx, config);
	});
</script>

<div class="aspect-video">
	<canvas bind:this={portfolio} />
</div>
