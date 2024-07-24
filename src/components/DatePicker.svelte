<script>
	import { onMount, createEventDispatcher } from 'svelte';
	import DateComponent from './DateComponent.svelte';
	import { getMonthName } from '../utils/helpers';
	import '../styles/DatePicker.css';

	export let selectedDates = [];

	const dispatch = createEventDispatcher();

	let currentDate = new Date();
	let currentMonth = currentDate.getMonth();
	let currentYear = currentDate.getFullYear();
	let daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();

	let calendarDays = [];
	const weekDays = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];

	let currentMonthName = getMonthName(currentMonth);

	onMount(() => {
		generateCalendarDays();
	});

	function generateCalendarDays() {
		calendarDays = [];
		let firstDay = new Date(currentYear, currentMonth, 1).getDay();

		for (let i = 0; i < firstDay; i++) {
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

	function getPreviousMonth() {
		if (currentMonth === 0) {
			currentMonth = 11;
			currentYear--;
		} else {
			currentMonth--;
		}
		currentMonthName = getMonthName(currentMonth);
		daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();
		generateCalendarDays();
	}

	function getNextMonth() {
		if (currentMonth === 11) {
			currentMonth = 0;
			currentYear++;
		} else {
			currentMonth++;
		}
		currentMonthName = getMonthName(currentMonth);
		daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();
		generateCalendarDays();
	}

	function handleDateClick(event) {
		dispatch('dateSelected', event.detail);
	}
</script>

<main class="calendarWrapper">
	<div class="calendarHeader">
		<button on:click={getPreviousMonth} class="previousButton">&#10140;</button>
		<p class="calendarMonthYear">{currentMonthName} {currentYear}</p>
		<button on:click={getNextMonth}>&#10140;</button>
	</div>
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
