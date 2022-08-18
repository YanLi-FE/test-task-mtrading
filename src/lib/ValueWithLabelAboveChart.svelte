<script lang="ts">
export let label: string;
export let value: number | string;
export let suffix: string = "";

let formattedValue: string;
$: formattedValue = Math.round(parseFloat(value.toString())).toLocaleString('ru-RU') + suffix;
</script>

<div class="value-with-label-above-chart">
	<div class="label">{label}</div>
	<div class="value">{formattedValue}</div>
</div>

<style>
.label {
	font-size: 0.875rem;
	color: #81818D;
}

.value {
	font-size: 1.75rem;
	margin-top: 0.5rem;
	font-family: "Gotham Pro", sans-serif;
}
</style>
