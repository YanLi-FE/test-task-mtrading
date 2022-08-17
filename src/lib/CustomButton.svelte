<button class="custom-button" on:click>
	<slot />
</button>

<style>
.custom-button {
	all: unset;
	user-select: none;
	padding: 1rem;
	border-radius: 0.375rem;
	background-color: #8A24F3;
	color: #FFFFFF;
	font-family: Roboto, sans-serif;
	font-weight: 700;
	text-align: center;
	cursor: pointer;
	transition: background-color 0.1s ease-out;
}

.custom-button:hover {
	background-color: #701BC8;
}

.custom-button:active {
	background-color: #4F118F;
}
</style>
