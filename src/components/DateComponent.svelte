<script>
	import { createEventDispatcher } from 'svelte';
	import '../styles/DateComponent.css';

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
