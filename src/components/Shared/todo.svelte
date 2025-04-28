<script>
	/**
	 * @type {any[]}
	 */
	let input = $state([]);
	let count = $state(1);
	let note = $state('');
	let isEditing = $state(false);
	let editingId = $state(null);

	function add() {
		if (note.trim() === '') return;

		if (isEditing) {
			input = input.map((item) => {
				if (item.id === editingId) {
					return { ...item, notes: note };
				}
				return item;
			});
			isEditing = false;
			editingId = null;
		} else {
			input.push({ id: count++, notes: note });
		}

		note = '';
	}

	/**
	 * @param {number} id
	 */
	function deleteElement(id) {
		input = input.filter((e) => e.id !== id);
		if (isEditing && editingId === id) {
			isEditing = false;
			editingId = null;
			note = '';
		}
	}

	/**
	 * @param {{ notes: string; id: null; }} element
	 */
	function editElement(element) {
		note = element.notes;
		isEditing = true;
		editingId = element.id;
	}
</script>

<div class="container">
	<h1>📝 Notes App</h1>
	<div class="input-section">
		<input type="text" bind:value={note} placeholder="Type your note..." />
		<button on:click={add}>{isEditing ? 'Update' : 'Add Note'}</button>
	</div>

	<div class="notes-grid">
		{#each input as element (element.id)}
			<div class="note-card">
				<div class="note-id"># {element.id}</div>
				<div class="note-text">
					<span>
						{element.notes}
					</span>

					<div class="actions">
						<button class="edit-btn" on:click={() => editElement(element)}>✏️ Edit</button>
						<button class="delete-btn" on:click={() => deleteElement(element.id)}>🗑️ Delete</button>
					</div>
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
		background: #f4f4f9;
	}

	.container {
		max-width: 700px;
		margin: 40px auto;
		padding: 20px;
		background: white;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
		border-radius: 12px;
	}

	h1 {
		text-align: center;
		color: #333;
		margin-bottom: 20px;
	}

	.input-section {
		display: flex;
		gap: 10px;
		margin-bottom: 30px;
	}

	input[type='text'] {
		flex: 1;
		padding: 12px 16px;
		border: 2px solid #ddd;
		border-radius: 8px;
		font-size: 16px;
		transition: border-color 0.3s;
	}

	input[type='text']:focus {
		outline: none;
		border-color: #6c63ff;
	}

	button {
		padding: 12px 20px;
		border: none;
		border-radius: 8px;
		cursor: pointer;
		font-weight: bold;
		transition: background 0.3s;
	}

	button:hover {
		opacity: 0.9;
	}

	button:active {
		transform: scale(0.97);
	}

	button.edit-btn {
		background-color: #6c63ff;
		color: white;
	}

	button.delete-btn {
		background-color: #ff6b6b;
		color: white;
	}

	.input-section button {
		background-color: #00b894;
		color: white;
	}

	.notes-grid {
		display: flex;
		flex-direction: column;
		gap: 20px;
		max-height: 55vh;
		overflow: auto;
	}

	.note-card {
		background-color: #fff;
		padding: 16px;
		border-radius: 10px;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		gap: 12px;
		transition: transform 0.2s;
	}

	.note-id {
		font-weight: bold;
		color: #888;
	}

	.note-text {
		display: flex;
		text-align: center;
		align-items: center;
		justify-content: space-between;
	}

	.note-text > span {
		font-size: 24px;
		color: #333;
		word-wrap: break-word;
	}
	.actions {
		display: flex;
		justify-content: space-between;
		gap: 32px;
	}
</style>
