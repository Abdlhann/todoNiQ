<script>
	import { createEventDispatcher } from 'svelte';
	import TodoCardEnhanced from './TodoCardEnhanced.svelte'
	
	export let cards, listName

	const dispatch = createEventDispatcher();

	let todo = ''
	let priority = 'medium';
	let dueDate = '';
	let showAdvanced = false;
	
	function handleAddCard() {
		if (todo.trim() !== '') {
			dispatch('addCard', { 
				todo, 
				listName,
				priority,
				dueDate,
				labels: [],
				subtasks: []
			});
			todo = ''
			priority = 'medium';
			dueDate = '';
			showAdvanced = false;
		}
	}
	
	function handleDeleteCard(event) {
		let data = event.detail
		dispatch('deleteCard', { index: data.index, listName });
	}

	function handleMoveRight(event){
		let data = event.detail
		dispatch('moveRight', { index: data.index, listName });
	}
	
	function handleMoveLeft(event){
		let data = event.detail
		dispatch('moveLeft', { index: data.index, listName });
	}
</script>

<div class="card">
	<div class="card-header {listName === 'Tasks' ? 'tasks' : listName === 'In Progress' ? 'in-progress' : 'done'}">
		<h2 class="card-header-title">
			{listName}
			{#if cards.length > 0}
				<span class="task-counter">{cards.length}</span>
			{/if}
		</h2>
	</div>
	<div class="card-content">
		<div class="todo-list">
			{#if cards.length === 0}
				<div class="empty-state">
					{#if listName === "Tasks"}
						<div class="empty-state-icon">📝</div>
						<div class="empty-state-text">Belum ada task nih!</div>
						<div class="empty-state-subtext">Yuk mulai tambahkan task baru 🚀</div>
					{:else if listName === "In Progress"}
						<div class="empty-state-icon">⚡</div>
						<div class="empty-state-text">Belum ada yang dikerjakan</div>
						<div class="empty-state-subtext">Pindahkan task dari Tasks untuk mulai! 💪</div>
					{:else}
						<div class="empty-state-icon">🎉</div>
						<div class="empty-state-text">Belum ada yang selesai</div>
						<div class="empty-state-subtext">Semangat menyelesaikan task! ✨</div>
					{/if}
				</div>
			{:else}
				{#each cards as card, index}
					<TodoCardEnhanced 
						content={card.todo} 
						ListName={listName} 
						index={index}
						priority={card.priority || 'medium'}
						dueDate={card.dueDate || ''}
						labels={card.labels || []}
						subtasks={card.subtasks || []}
						on:deleteCard={handleDeleteCard} 
						on:moveRight={handleMoveRight}
						on:moveLeft={handleMoveLeft} 
					/>
				{/each}
			{/if}
		</div>
		<div class="input-section">
			<input 
				type="text" 
				class="input" 
				placeholder="Tambah task baru..." 
				bind:value={todo} 
				on:keydown={(e) => e.key === 'Enter' && handleAddCard()}
			/>
			
			{#if showAdvanced}
				<div style="display: flex; gap: 0.5rem; margin-bottom: 1rem;">
					<select class="form-select" bind:value={priority} style="flex: 1;">
						<option value="low">🟢 Low</option>
						<option value="medium">🟡 Medium</option>
						<option value="high">🔴 High</option>
					</select>
					<input 
						type="date" 
						class="input" 
						bind:value={dueDate}
						style="flex: 1; margin-bottom: 0;"
					/>
				</div>
			{/if}
			
			<div style="display: flex; gap: 0.5rem;">
				<button 
					on:click={handleAddCard} 
					class="button"
					disabled={todo.trim() === ''}
					style="flex: 1;"
				>
					Tambah Task
				</button>
				<button 
					on:click={() => showAdvanced = !showAdvanced}
					class="button"
					style="width: auto; padding: 0 1.25rem; background: var(--card-bg); color: var(--text-primary);"
					title="Advanced Options"
				>
					<i class="fas fa-{showAdvanced ? 'minus' : 'plus'}"></i>
				</button>
			</div>
		</div>
	</div>
</div>
