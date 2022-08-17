<script lang="ts">
import {createChart, CrosshairMode} from 'lightweight-charts';
import type {IChartApi, ISeriesApi} from 'lightweight-charts';
import {onMount} from "svelte";

const DAY = 1000 * 60 * 60 * 24;

export let data: number[];

let chartContainer: HTMLDivElement;
let chart: IChartApi;
let lineSeries: ISeriesApi<"Line">;

let chartData: { time: string; value: number }[];
$: {
	const startDate = new Date(Date.now() - DAY * 30);
	chartData = data.map((value, index) => ({
		time: new Date(startDate.getTime() + DAY * index).toISOString(),
		value: value
	}));
	if (lineSeries) {
		lineSeries.setData(chartData);
		chart.timeScale().fitContent();
	}
}

onMount(() => {
	chart = createChart(chartContainer, {
		crosshair: {
			mode: CrosshairMode.Magnet,
			vertLine: {
				color: "#DDE0E9",
			},
			horzLine: {
				color: "#DDE0E9",
			},
		},
		grid: {
			horzLines: {
				visible: false,
			},
			vertLines: {
				visible: false,
			},
		},
		handleScale: false,
		handleScroll: false,
		layout: {
			textColor: "#D9D9D9",
		},
		rightPriceScale: {
			borderColor: "#DDE0E9",
		},
		timeScale: {
			borderColor: "#DDE0E9",
		}
	});

	lineSeries = chart.addLineSeries({
		color: "#8A24F3",
	});
});
</script>

<div bind:this={chartContainer}></div>
