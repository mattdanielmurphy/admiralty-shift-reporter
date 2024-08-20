<script lang="ts">
	let date: string = getTodayDateString()
	let name = ''
	let weather = ''
	let overtime = false
	let timeIn = ''
	let timeOut = ''
	let details = ''
	let tasks = [{ time: '', description: '' }]
	let isTodaySelected = true
	let typing = false
	let typingTimeout: number | undefined

	// Return today's date in yyyy-mm-dd format
	function getTodayDateString(): string {
		const today = new Date()
		const year = today.getFullYear()
		const month = String(today.getMonth() + 1).padStart(2, '0')
		const day = String(today.getDate()).padStart(2, '0')
		return `${year}-${month}-${day}`
	}

	// Add a new empty task to the list
	function addTask(): void {
		tasks = [...tasks, { time: '', description: '' }]
	}

	// Submit the report by logging the values to the console
	function submitReport(): void {
		console.log({ date, name, weather, overtime, timeIn, timeOut, details, tasks })
	}

	// Validate the selected date and revert if necessary
	function validateDate(selectedDate: string): void {
		const todayDate = getTodayDateString()
		if (selectedDate > todayDate) {
			alert("Selected date is in the future. Reverting to today's date.")
			date = todayDate
		} else {
			date = selectedDate
			isTodaySelected = date === todayDate
		}
	}

	// Handle the change event for the date input
	function handleDateChange(e: Event): void {
		const target = e.target as HTMLInputElement
		if (!typing) {
			validateDate(target.value)
		}
	}

	// Handle typing input in the date field
	function handleDateInput(): void {
		typing = true
		clearTimeout(typingTimeout)
		typingTimeout = setTimeout(() => {
			typing = false
		}, 500)
	}

	// Handle blur event for the date input
	function handleDateBlur(e: Event): void {
		const target = e.target as HTMLInputElement
		if (typing) {
			validateDate(target.value)
		}
	}
</script>

<form on:submit|preventDefault={submitReport} class="shift-report-form">
	<div class="form-section">
		<div class="form-item">
			<label for="date-input">Date</label>
			<div class="date-container">
				<input
					id="date-input"
					type="date"
					bind:value={date}
					on:change={handleDateChange}
					on:input={handleDateInput}
					on:blur={handleDateBlur}
					class:is-today-selected={isTodaySelected}
				/>
				{#if isTodaySelected}
					<span class="date-indicator">today</span>
				{/if}
			</div>
		</div>
		<div class="form-item">
			<label for="name-input">Name</label>
			<input id="name-input" type="text" bind:value={name} />
		</div>
		<div class="form-item">
			<label for="weather-input">Weather</label>
			<input id="weather-input" type="text" bind:value={weather} />
		</div>
		<div class="form-item">
			<label for="time-in-input">Time In</label>
			<input id="time-in-input" type="time" bind:value={timeIn} />
		</div>
		<div class="form-item">
			<label for="time-out-input">Time Out</label>
			<input id="time-out-input" type="time" bind:value={timeOut} />
		</div>
		<div class="form-item">
			<label for="details-input">Details</label>
			<textarea id="details-input" bind:value={details}></textarea>
		</div>
	</div>
	<div class="form-section">
		<h3>Tasks</h3>
		{#each tasks as task, index}
			<div class="task-entry">
				<input type="time" bind:value={task.time} placeholder="Time" />
				<input type="text" bind:value={task.description} placeholder="Description" />
			</div>
		{/each}
		<button type="button" on:click={addTask}>Add Task</button>
	</div>
	<div class="form-item">
		<label for="overtime-input">Overtime?</label>
		<input id="overtime-input" type="checkbox" bind:value={overtime} />
	</div>
	<button type="submit">Submit</button>
</form>

<style>
	.shift-report-form {
		max-width: 600px;
		margin: 0 auto;
		padding: 20px;
		border: 1px solid #ccc;
		border-radius: 0.5rem;
		background-color: #f9f9f9;
	}
	.form-section {
		margin-bottom: 20px;
	}
	.form-item {
		display: flex;
		align-items: center;
		margin-bottom: 1.3rem;
	}
	.form-item label {
		width: 5rem;
		margin-right: 10px;
		text-align: center;
	}
	input,
	textarea {
		flex: 1;
		padding: 0.5rem;
		border: 1px solid #ccc;
		border-radius: 0.25rem;
	}
	textarea {
		height: 80px;
	}
	.task-entry {
		display: flex;
		gap: 10px;
		margin-bottom: 10px;
	}
	button {
		padding: 10px 15px;
		border: none;
		border-radius: 0.25rem;
		background-color: #007bff;
		color: white;
		cursor: pointer;
	}
	button[type='button'] {
		background-color: #6c757d;
	}
	.date-container {
		display: flex;
		align-items: center;
	}
	.date-indicator {
		font-style: italic;
		color: #007bff;
		margin-left: 10px;
	}
	#overtime-input {
		width: 2rem;
		flex: 0;
	}
</style>
