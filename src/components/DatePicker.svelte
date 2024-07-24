<script>
	import { onMount } from 'svelte';
	import DateComponent from './DateComponent.svelte';

	export let selectedDates = [];

	let currentDate = new Date();
	let currentMonth = currentDate.getMonth();
	let currentYear = currentDate.getFullYear();
	let daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();

	let calendarDays = [];

	function getMonthName(monthNumber) {
		const date = new Date();
		date.setMonth(monthNumber);
		return date.toLocaleString('default', { month: 'long' });
	}

	let currentMonthName = getMonthName(currentMonth);

	onMount(() => {
		generateCalendarDays();
	});

	function generateCalendarDays() {
		calendarDays = [];
		for (let i = 1; i <= daysInMonth; i++) {
			let date = new Date(currentYear, currentMonth, i);
			calendarDays.push({
				date: date,
				isSelected: selectedDates.some((selectedDate) => {
					return date.toDateString() === new Date(selectedDate).toDateString();
				}),
				isPast: date < new Date()
			});
		}
	}

	function handleDateClick(date) {
		dispatch('dateSelected', date);
	}
</script>

<main class="calendarWrapper">
	<p class="calendarMonthYear">{currentMonthName} {currentYear}</p>
	<div class="calendar">
		{#each calendarDays as { date, isSelected, isPast }}
			<DateComponent {date} {isSelected} {isPast} on:dateClick={handleDateClick} />
		{/each}
	</div>
</main>

<style>
	.calendar {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
		gap: 5px;
		width: 400px;
	}

	.calendarWrapper {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
	}
</style>
