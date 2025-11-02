<script>
	import { createEventDispatcher } from 'svelte';

    import TodoCard from './TodoCard.svelte'
    export let cards, listName

    const dispatch = createEventDispatcher();

    let todo = ''
	function handleAddCard() {
		if (todo.trim() !== '') {
			dispatch('addCard', { todo, listName });
			todo = ''
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

<style></style>

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
					<TodoCard 
					content={card.todo} 
					ListName={listName} 
					index={index} 
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
			<button 
				on:click={handleAddCard} 
				class="button"
				disabled={todo.trim() === ''}
			>
				Tambah Task
			</button>
		</div>
	</div>
</div>