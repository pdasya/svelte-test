<script>
	import { onMount, createEventDispatcher } from 'svelte';
	import DateComponent from './DateComponent.svelte';

	export let selectedDates = [];

	const dispatch = createEventDispatcher();

	let currentDate = new Date();
	let currentMonth = currentDate.getMonth();
	let currentYear = currentDate.getFullYear();
	let daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();

	let calendarDays = [];
	const weekDays = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];

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
		let firstDay = new Date(currentYear, currentMonth, 1).getDay();

		let offset = firstDay === 1 ? 1 : 7 - firstDay;

		for (let i = 0; i < offset; i++) {
			calendarDays.push(null);
		}

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

	function handleDateClick(event) {
		dispatch('dateSelected', event.detail);
	}
</script>

<main class="calendarWrapper">
	<p class="calendarMonthYear">{currentMonthName} {currentYear}</p>
	<div class="calendar">
		<div class="weekDays">
			{#each weekDays as day}
				<div class="weekDay">{day}</div>
			{/each}
		</div>
		<div class="calendarGrid">
			{#each calendarDays as day}
				{#if day}
					<DateComponent
						date={day.date}
						isSelected={day.isSelected}
						isPast={day.isPast}
						on:dateClick={handleDateClick}
					/>
				{:else}
					<div class="empty-day"></div>
				{/if}
			{/each}
		</div>
	</div>
</main>

<style>
	.calendarWrapper {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
	}

	.calendarMonthYear {
		font-size: 1.5rem;
		margin-bottom: 1rem;
	}

	.calendar {
		width: 400px;
	}

	.weekDays {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
		gap: 5px;
		padding: 10px 0;
	}

	.weekDay {
		text-align: center;
		font-weight: bold;
	}

	.calendarGrid {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
		gap: 5px;
	}

	.empty-day {
		width: 100%;
		height: 100%;
	}
</style>
