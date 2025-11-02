<script>
	import { createEventDispatcher } from 'svelte';
	
	export let show = false;
	export let task = null;
	export let listName = 'Tasks';
	
	const dispatch = createEventDispatcher();
	
	let title = '';
	let priority = 'medium';
	let dueDate = '';
	let labels = [];
	let newLabel = '';
	let subtasks = [];
	let newSubtask = '';
	
	$: if (task) {
		title = task.todo || '';
		priority = task.priority || 'medium';
		dueDate = task.dueDate || '';
		labels = task.labels || [];
		subtasks = task.subtasks || [];
	}
	
	function handleSubmit() {
		const taskData = {
			todo: title,
			priority,
			dueDate,
			labels: [...labels],
			subtasks: [...subtasks],
			listName
		};
		
		if (task) {
			dispatch('update', taskData);
		} else {
			dispatch('add', taskData);
		}
		
		closeModal();
	}
	
	function closeModal() {
		show = false;
		resetForm();
	}
	
	function resetForm() {
		title = '';
		priority = 'medium';
		dueDate = '';
		labels = [];
		newLabel = '';
		subtasks = [];
		newSubtask = '';
	}
	
	function addLabel() {
		if (newLabel.trim() && !labels.includes(newLabel.trim())) {
			labels = [...labels, newLabel.trim()];
			newLabel = '';
		}
	}
	
	function removeLabel(label) {
		labels = labels.filter(l => l !== label);
	}
	
	function addSubtask() {
		if (newSubtask.trim()) {
			subtasks = [...subtasks, { text: newSubtask.trim(), completed: false }];
			newSubtask = '';
		}
	}
	
	function removeSubtask(index) {
		subtasks = subtasks.filter((_, i) => i !== index);
	}
	
	function handleKeydown(e, action) {
		if (e.key === 'Enter') {
			e.preventDefault();
			action();
		}
	}
</script>

{#if show}
<div class="modal-overlay" on:click={closeModal}>
	<div class="modal" on:click|stopPropagation>
		<div class="modal-header">
			<h2 class="modal-title">{task ? '✏️ Edit Task' : '➕ Tambah Task Baru'}</h2>
			<button class="modal-close" on:click={closeModal}>
				<i class="fas fa-times"></i>
			</button>
		</div>
		
		<div class="modal-body">
			<div class="form-group">
				<label class="form-label" for="task-title">Judul Task</label>
				<input
					id="task-title"
					type="text"
					class="input"
					placeholder="Masukkan judul task..."
					bind:value={title}
					style="margin-bottom: 0;"
				/>
			</div>
			
			<div class="form-group">
				<label class="form-label" for="task-priority">Priority</label>
				<select id="task-priority" class="form-select" bind:value={priority}>
					<option value="low">🟢 Low Priority</option>
					<option value="medium">🟡 Medium Priority</option>
					<option value="high">🔴 High Priority</option>
				</select>
			</div>
			
			<div class="form-group">
				<label class="form-label" for="task-due-date">Due Date</label>
				<input
					id="task-due-date"
					type="date"
					class="input"
					bind:value={dueDate}
					style="margin-bottom: 0;"
				/>
			</div>
			
			<div class="form-group">
				<label class="form-label">Labels</label>
				<div style="display: flex; gap: 0.5rem;">
					<input
						type="text"
						class="input"
						placeholder="Tambah label..."
						bind:value={newLabel}
						on:keydown={(e) => handleKeydown(e, addLabel)}
						style="margin-bottom: 0; flex: 1;"
					/>
					<button class="button" on:click={addLabel} style="width: auto; padding: 0 1.5rem;">
						Tambah
					</button>
				</div>
				{#if labels.length > 0}
					<div class="label-input-group">
						{#each labels as label}
							<div class="label-chip">
								{label}
								<button on:click={() => removeLabel(label)}>×</button>
							</div>
						{/each}
					</div>
				{/if}
			</div>
			
			<div class="form-group">
				<label class="form-label">Subtasks</label>
				<div style="display: flex; gap: 0.5rem;">
					<input
						type="text"
						class="input"
						placeholder="Tambah subtask..."
						bind:value={newSubtask}
						on:keydown={(e) => handleKeydown(e, addSubtask)}
						style="margin-bottom: 0; flex: 1;"
					/>
					<button class="button" on:click={addSubtask} style="width: auto; padding: 0 1.5rem;">
						Tambah
					</button>
				</div>
				{#if subtasks.length > 0}
					<div class="subtasks-list">
						{#each subtasks as subtask, index}
							<div class="subtask-item">
								<span class="subtask-text">{subtask.text}</span>
								<button 
									class="action-btn delete" 
									on:click={() => removeSubtask(index)}
									style="width: 28px; height: 28px; min-width: 28px; min-height: 28px;"
								>
									<i class="fas fa-trash"></i>
								</button>
							</div>
						{/each}
					</div>
				{/if}
			</div>
		</div>
		
		<div style="display: flex; gap: 0.75rem;">
			<button class="button" on:click={handleSubmit} disabled={!title.trim()}>
				{task ? 'Update Task' : 'Tambah Task'}
			</button>
			<button 
				class="button" 
				on:click={closeModal}
				style="background: var(--input-bg); color: var(--text-primary);"
			>
				Batal
			</button>
		</div>
	</div>
</div>
{/if}
