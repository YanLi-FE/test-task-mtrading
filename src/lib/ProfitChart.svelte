<script lang="ts">
import {createChart, CrosshairMode} from 'lightweight-charts';
import type {IChartApi, ISeriesApi} from 'lightweight-charts';
import {onMount} from "svelte";

const DAY = 1000 * 60 * 60 * 24;
const CHART_HEIGHT = 252;
const CHART_SECONDARY_LINE_COLOR = "#DDE0E9";
const CHART_PRIMARY_LINE_COLOR = "#8A24F3";
const CHART_TEXT_COLOR = "#D9D9D9";

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
				color: CHART_SECONDARY_LINE_COLOR,
			},
			horzLine: {
				color: CHART_SECONDARY_LINE_COLOR,
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
			textColor: CHART_TEXT_COLOR,
		},
		rightPriceScale: {
			borderColor: CHART_SECONDARY_LINE_COLOR,
		},
		timeScale: {
			borderColor: CHART_SECONDARY_LINE_COLOR,
		},
		height: CHART_HEIGHT,
	});

	lineSeries = chart.addLineSeries({
		color: CHART_PRIMARY_LINE_COLOR,
	});

	const observer = new ResizeObserver(entries => {
		const div = entries[0];
		chart.resize(div.contentRect.width, chartContainer.clientHeight, true);
		chart.timeScale().fitContent();
	});

	observer.observe(chartContainer.parentElement);

	return () => observer.disconnect();
});
</script>

<div bind:this={chartContainer}></div>
