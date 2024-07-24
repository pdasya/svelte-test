<script>
	import { createEventDispatcher } from 'svelte';

	export let date;
	export let isSelected = false;
	export let isPast = false;

	const dispatch = createEventDispatcher();

	function handleClick() {
		if (isSelected) {
			dispatch('dateClick', date);
		}
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="date {isPast ? 'past' : isSelected ? 'selectable' : 'future'}" on:click={handleClick}>
	{date.getDate()}
</div>

<style>
	.date {
		padding: 10px;
		text-align: center;
	}

	.past {
		color: gray;
	}

	.future {
		color: black;
	}

	.selectable {
		color: red;
		cursor: pointer;
	}
</style>
