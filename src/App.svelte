<script lang="ts">
import {onMount} from "svelte";
import TraderButton from "./lib/TraderButton.svelte";
import ValueWithLabelAboveChart from "./lib/ValueWithLabelAboveChart.svelte";
import CustomButton from "./lib/CustomButton.svelte";
import ProfitChart from "./lib/ProfitChart.svelte";
import CarouselDots from "./lib/CarouselDots.svelte";

let traders: TraderData[] = [];
let selectedTraderIndex = 0;

let selectedTrader: TraderData;
$: selectedTrader = traders[selectedTraderIndex];

function randomFromZero(max: number): number {
	return Math.floor(Math.random() * max);
}

function selectTrader(index: number): void {
	selectedTraderIndex = index;
}

function onCopyNowClicked(): void {
	console.log(`
Trader name: ${selectedTrader.name}
Trader capital: ${parseFloat(selectedTrader.capital).toLocaleString('ru-RU')} USD
Monthly profit: ${selectedTrader.monthly_profit}%
Total profit: ${selectedTrader.total_profit}%
`.trim());
}

onMount(async () => {
	const response = await fetch("/input.json");
	const allTraders: TraderData[] = await response.json();
	for (let i = 0; i < 4; i++) {
		traders.push(allTraders.splice(randomFromZero(allTraders.length), 1)[0]);
	}
	traders = traders;
});
</script>

<h1 class="main-header">Copy the best masters</h1>

<main class="main-grid">
	<div class="traders">
		{#each traders as trader, index}
			<TraderButton
				{index}
				name={trader.name}
				active={index === selectedTraderIndex}
				flag={trader.flag}
				percentage={trader.monthly_profit}
				on:click={() => selectTrader(index)}
			/>
		{/each}
	</div>
	{#if selectedTrader != null}
		<div class="full-info-container">
			<TraderButton
				index={selectedTraderIndex}
				name={selectedTrader.name}
				flag={selectedTrader.flag}
				percentage={selectedTrader.monthly_profit}
			/>
			<ValueWithLabelAboveChart
				label="Monthly profit"
				value={selectedTrader.monthly_profit}
				suffix="%"
			/>
			<ValueWithLabelAboveChart
				label="Total profit"
				value={selectedTrader.total_profit}
				suffix="%"
			/>
			<ValueWithLabelAboveChart
				label="In management"
				value={selectedTrader.capital}
				suffix=" USD"
			/>
			<CustomButton on:click={onCopyNowClicked}>Copy Now</CustomButton>
			<ProfitChart data={selectedTrader.chart} />
		</div>
	{/if}
</main>

<div class="carousel-dots">
	<CarouselDots total={traders.length} current={selectedTraderIndex} />
</div>

<style>
.main-header {
	max-width: 1180px;
	font-size: 3rem;
	font-weight: 500;
	line-height: 5.125rem;
	color: #0A071E;
	margin: 2.5rem auto 1.56rem;
	font-family: "Gotham Pro", sans-serif;
}

.main-grid {
	max-width: 1180px;
	display: grid;
	grid-template-columns: 295px 1fr;
	gap: 29px;
	margin: 0 auto;
}

.traders {
	display: flex;
	flex-direction: column;
	align-items: stretch;
}

.full-info-container {
	border: 0.0625rem solid #E8EAED;
	border-radius: 1.25rem;
	display: grid;
	grid-template-columns: repeat(2, 1fr) repeat(2, 1.5fr);
	grid-template-rows: auto 1fr;
	gap: 1rem;
	padding: 1.75rem 1.5rem 1.5rem 1.5rem;
}

/* trader on mobile */
.full-info-container > :global(:nth-child(1)) {
	display: none;
}

/* chart */
.full-info-container > :global(:last-child) {
	grid-column: 1 / span 4;
}

.carousel-dots {
	display: none;
	margin: 1.25rem 0;
}

@media (max-width: 768px) {
	.main-header, .main-grid {
		max-width: 768px;
	}

	.main-header {
		font-size: 1.75rem;
		line-height: 2rem;
	}

	.traders {
		display: none;
	}

	.main-grid {
		grid-template-columns: 1fr;
	}

	.full-info-container {
		grid-template-columns: repeat(2, 1fr);
	}

	/* trader on mobile */
	.full-info-container > :global(:nth-child(1)) {
		display: flex;
		grid-column: 1 / span 2;
	}

	/* in management */
	.full-info-container > :global(:nth-child(4)) {
		display: none;
	}

	/* button */
	.full-info-container > :global(:nth-child(5)) {
		grid-column: 1 / span 2;
		grid-row: 4;
	}

	/* chart */
	.full-info-container > :global(:last-child) {
		grid-column: 1 / span 2;
	}

	.carousel-dots {
		display: block;
	}
}
</style>
