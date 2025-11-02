<script>
	import { createEventDispatcher } from 'svelte';
	import { fade } from 'svelte/transition';
	
	export let content;
	export let ListName;
	export let index;
	export let priority = 'medium';
	export let dueDate = '';
	export let labels = [];
	export let subtasks = [];

	const dispatch = createEventDispatcher();

	function handleDeleteCard() {
		dispatch('deleteCard', { index });
	}
	
	function handleMoveRight(){
		dispatch('moveRight', { index });
	}
	
	function handleMoveLeft(){
		dispatch('moveLeft', { index });
	}
	
	function toggleSubtask(subtaskIndex) {
		subtasks[subtaskIndex].completed = !subtasks[subtaskIndex].completed;
		subtasks = subtasks;
	}
	
	function getDueDateStatus(date) {
		if (!date) return '';
		const today = new Date();
		today.setHours(0, 0, 0, 0);
		const due = new Date(date);
		due.setHours(0, 0, 0, 0);
		
		if (due < today) return 'overdue';
		if (due.getTime() === today.getTime()) return 'today';
		return '';
	}
	
	$: completedSubtasks = subtasks.filter(st => st.completed).length;
	$: subtaskProgress = subtasks.length > 0 ? (completedSubtasks / subtasks.length) * 100 : 0;
	$: dueDateStatus = getDueDateStatus(dueDate);
</script>

<div class="todo-card" transition:fade draggable="true">
	<div class="todo-content">
		{#if ListName != "Tasks"}
			<button 
				class="action-btn move" 
				on:click={handleMoveLeft} 
				title="Pindah ke kiri"
				aria-label="Pindah task ke kiri"
			>
				<i class="fas fa-chevron-left"></i>
			</button>
		{/if}
		
		<div class="todo-details">
			<div class="todo-title">{content}</div>
			
			<div class="todo-meta">
				{#if priority}
					<span class="priority-badge priority-{priority}">
						{priority === 'high' ? '🔴' : priority === 'medium' ? '🟡' : '🟢'} {priority}
					</span>
				{/if}
				
				{#each labels as label}
					<span class="label-tag">{label}</span>
				{/each}
				
				{#if dueDate}
					<span class="due-date {dueDateStatus}">
						<i class="fas fa-calendar"></i>
						{new Date(dueDate).toLocaleDateString('id-ID', { day: 'numeric', month: 'short' })}
					</span>
				{/if}
			</div>
			
			{#if subtasks && subtasks.length > 0}
				<div class="subtask-progress">
					<span style="font-size: 11px; white-space: nowrap;">
						{completedSubtasks}/{subtasks.length}
					</span>
					<div class="progress-bar">
						<div class="progress-fill" style="width: {subtaskProgress}%"></div>
					</div>
				</div>
			{/if}
		</div>
		
		<div class="todo-actions">
			<button 
				class="action-btn delete" 
				on:click={handleDeleteCard} 
				title="Hapus task"
				aria-label="Hapus task"
			>
				<i class="fas fa-trash"></i>
			</button>
			
			{#if ListName != "Done"}
				<button 
					class="action-btn move" 
					on:click={handleMoveRight} 
					title="Pindah ke kanan"
					aria-label="Pindah task ke kanan"
				>
					<i class="fas fa-chevron-right"></i>
				</button>
			{/if}
		</div>
	</div>
</div>
