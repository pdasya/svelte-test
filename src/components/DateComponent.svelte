<script>
	import { createEventDispatcher } from 'svelte';

	export let date;
	export let isSelected = false;
	export let isPast = false;
	let isActive = false;

	const dispatch = createEventDispatcher();

	function handleClick() {
		if (isSelected) {
			isActive = !isActive;
			dispatch('dateClick', date);
		}
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	class="date {isPast ? 'past' : isSelected ? 'selectable' : 'future'} {isActive ? 'active' : ''}"
	on:click={handleClick}
>
	{date.getDate()}
</div>

<style>
	.date {
		padding: 10px;
		text-align: center;
		font-weight: bold;
		border: 2px solid transparent;
	}

	.past {
		color: gray;
	}

	.future {
		color: black;
	}

	.selectable {
		box-sizing: border-box;
		color: white;
		cursor: pointer;
		background-color: red;
		border-radius: 10px;
		border: 2px solid transparent;
	}

	.active {
		border: solid 2px rgb(115, 5, 5);
	}
</style>
