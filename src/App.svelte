<script>
	import DatePicker from './components/DatePicker.svelte';

	let selectedDates = ['2024-08-15', '2024-07-20', '2024-07-25'];

	let chosenDates = new Set();

	function formatDateToYYYYMMDD(dateString) {
		const date = new Date(dateString);
		const year = date.getFullYear();
		const month = String(date.getMonth() + 1).padStart(2, '0');
		const day = String(date.getDate()).padStart(2, '0');
		return `${year}-${month}-${day}`;
	}

	function handleDateSelected(event) {
		document.body.classList.add('hidden');

		chosenDates.add(formatDateToYYYYMMDD(event.detail));
	}
</script>

<DatePicker {selectedDates} on:dateSelected={handleDateSelected} />
