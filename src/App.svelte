<script>
	import DatePicker from './components/DatePicker.svelte';

	let selectedDates = ['2024-08-15', '2024-07-20', '2024-07-25'];

	function handleDateSelected(event) {
		console.log('Выбрана дата:', event.detail);
	}
</script>

<DatePicker {selectedDates} on:dateSelected={handleDateSelected} />
