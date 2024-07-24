<script>
	import DatePicker from './components/DatePicker.svelte';
	import { formatDateToYYYYMMDD } from './utils/helpers';

	let selectedDates = ['2024-08-15', '2024-07-20', '2024-07-25'];

	let chosenDates = new Set();

	function handleDateSelected(event) {
		document.body.classList.add('hidden');

		chosenDates.add(formatDateToYYYYMMDD(event.detail));
	}
</script>

<DatePicker {selectedDates} on:dateSelected={handleDateSelected} />
