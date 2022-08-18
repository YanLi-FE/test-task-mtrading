<script lang="ts">
export let total: number = 0;
export let current: number = 0;

let pagesForIteration: undefined[];
$: pagesForIteration = Array(total);
</script>

<div class="dot-container">
	{#each pagesForIteration as page, index}
		<div class="dot" class:dot-active={current === index}></div>
	{/each}
</div>

<style>
.dot-container {
	display: flex;
	justify-content: center;
	gap: 0.5rem;
}

.dot {
	width: 0.5rem;
	height: 0.5rem;
	border-radius: 0.25rem;
	background-color: #D9D9D9;
	transition: width 0.2s ease-out, background-color 0.2s ease-out;
}

.dot-active {
	background-color: #C8C8D5;
	width: 1.25rem;
}
</style>
