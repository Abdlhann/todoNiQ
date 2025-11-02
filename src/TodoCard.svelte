<script>
	import { createEventDispatcher } from 'svelte';
	import { fade } from 'svelte/transition';
    export let content, ListName, index;

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
</script>

<div class="todo-card" transition:fade>
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
		
		<div class="todo-text">{content}</div>
		
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