<script lang="ts">
export let flag: string;
export let index: number;
export let name: string;
export let percentage: number;
export let active: boolean = false;

let number: number;
$: number = index + 1;

let percentageSign: string;
$: percentageSign = percentage > 0 ? '+' : '-';

let formattedPercentage: string;
$: formattedPercentage = `${percentageSign} ${percentage.toLocaleString('ru-RU')}%`;

let flagAlt: string;
$: flagAlt = `${name}'${name.endsWith('s') ? '' : 's'} flag`
</script>

<button class="trader-button" class:active on:click>
	<img class="trader-button-flag" src={flag} alt={flagAlt}>
	<span class="trader-button-text">
		<span class="trader-button-name">
			{number}. {name}
		</span>
		<span
			class="trader-button-value"
			class:positive={percentage > 0}
			class:negative={percentage < 0}
		>{formattedPercentage}</span>
	</span>
</button>

<style>
.trader-button {
	all: unset;
	padding: 1.625rem 1.75em 1.625rem 1.5rem;
	display: flex;
	gap: 1rem;
	background-color: transparent;
	border-radius: 1.5625rem;
	user-select: none;
	cursor: pointer;
	transition: background-color 0.1s ease-out;
}

.trader-button.active {
	background-color: #F5F7F9;
}

.trader-button-flag {
	width: 3rem;
	height: 3rem;
	object-fit: cover;
	border-radius: 50%;
}

.trader-button-text {
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	flex: 1;
	font-size: 1.25rem;
	font-weight: 500;
	line-height: 1em;
}

.trader-button-value.positive {
	color: #34B428;
}

.trader-button-value.negative {
	color: #E53935;
}

@media (max-width: 768px) {
	.trader-button-text {
		font-size: 1rem;
		line-height: 1.25rem;
	}
}
</style>
