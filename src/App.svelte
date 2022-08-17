<script lang="ts">
import {onMount} from "svelte";
import TraderButton from "./lib/TraderButton.svelte";
import ValueWithLabelAboveChart from "./lib/ValueWithLabelAboveChart.svelte";
import CustomButton from "./lib/CustomButton.svelte";
import ProfitChart from "./lib/ProfitChart.svelte";

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

<main>
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

<style>
main {
	width: 1180px;
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

.full-info-container > :global(:last-child) {
	grid-column: 1 / span 4;
}
</style>
