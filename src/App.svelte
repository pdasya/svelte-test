<script>
	import DatePicker from './components/DatePicker.svelte';
	import { formatDateToYYYYMMDD } from './utils/helpers';
	import { selectedDates } from './utils/selectedDates';

	let chosenDates = new Set();

	function handleDateSelected(event) {
		document.body.classList.add('hidden');

		chosenDates.add(formatDateToYYYYMMDD(event.detail));
	}
</script>

<DatePicker {selectedDates} on:dateSelected={handleDateSelected} />
