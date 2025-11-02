<script>
	import CardListEnhanced from './CardListEnhanced.svelte'
	import Statistics from './Statistics.svelte'
	import TaskModal from './TaskModal.svelte'
	import { onMount } from 'svelte'

	// Load data from localStorage
	let taskCardsLocalStorage = JSON.parse(localStorage.getItem('taskcards'));
	let inProgressCardsLocalStorage = JSON.parse(localStorage.getItem('inProgressCards'));
	let doneCardsLocalStorage = JSON.parse(localStorage.getItem('doneCards'));

	let taskcards = taskCardsLocalStorage ? taskCardsLocalStorage : []
	let inProgressCards = inProgressCardsLocalStorage ? inProgressCardsLocalStorage : []
	let doneCards = doneCardsLocalStorage ? doneCardsLocalStorage : []

	// Theme
	let theme = localStorage.getItem('theme') || 'dark';
	
	// UI State
	let currentTime = ''
	let currentDate = ''
	let notification = { show: false, message: '', type: '' }
	let showModal = false;
	let editingTask = null;
	let editingListName = '';
	
	// Search & Filter
	let searchQuery = '';
	let filterPriority = 'all';
	let filterStatus = 'all';
	
	// Filtered tasks
	$: filteredTaskcards = filterTasks(taskcards, searchQuery, filterPriority);
	$: filteredInProgress = filterTasks(inProgressCards, searchQuery, filterPriority);
	$: filteredDone = filterTasks(doneCards, searchQuery, filterPriority);
	
	function filterTasks(tasks, query, priority) {
		return tasks.filter(task => {
			const matchesSearch = !query || task.todo.toLowerCase().includes(query.toLowerCase());
			const matchesPriority = priority === 'all' || task.priority === priority;
			return matchesSearch && matchesPriority;
		});
	}

	function updateDateTime() {
		const now = new Date()
		currentTime = now.toLocaleTimeString('id-ID', { 
			hour: '2-digit', 
			minute: '2-digit',
			second: '2-digit'
		})
		currentDate = now.toLocaleDateString('id-ID', { 
			weekday: 'long', 
			year: 'numeric', 
			month: 'long', 
			day: 'numeric' 
		})
	}

	onMount(() => {
		updateDateTime()
		const interval = setInterval(updateDateTime, 1000)
		
		// Apply saved theme
		document.documentElement.setAttribute('data-theme', theme);
		
		return () => clearInterval(interval)
	})
	
	function toggleTheme() {
		theme = theme === 'dark' ? 'light' : 'dark';
		document.documentElement.setAttribute('data-theme', theme);
		localStorage.setItem('theme', theme);
		showNotification(`🎨 Tema ${theme === 'dark' ? 'Gelap' : 'Terang'} diaktifkan!`, 'info');
	}

	function handleEventAddCard(event) {
		let data = event.detail
		const newTask = {
			todo: data.todo,
			priority: data.priority || 'medium',
			dueDate: data.dueDate || '',
			labels: data.labels || [],
			subtasks: data.subtasks || [],
			createdAt: new Date().toISOString()
		};
		
		if (data.listName === "Tasks") {
			taskcards = [...taskcards, newTask]
			localStorage.setItem('taskcards', JSON.stringify(taskcards));
			showNotification('📝 Task baru ditambahkan! Siap produktif! 🌟')
		} else if (data.listName === "In Progress") {
			inProgressCards = [...inProgressCards, newTask]
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards));
			showNotification('⚡ Task langsung dikerjakan! Mantap! 🔥')
		} else {
			doneCards = [...doneCards, newTask]
			localStorage.setItem('doneCards', JSON.stringify(doneCards));
			showNotification('✅ Task langsung selesai! Efisien banget! 🏆')
		}
	}

	function handleEventDeleteCard(event) {
		let data = event.detail
		if (data.listName === "Tasks") {
			taskcards.splice(data.index, 1)
			taskcards = taskcards
			localStorage.setItem('taskcards', JSON.stringify(taskcards));
			showNotification('🗑️ Task dihapus! Bersih dan rapi! ✨', 'info')
		} else if (data.listName === "In Progress") {
			inProgressCards.splice(data.index, 1)
			inProgressCards = inProgressCards
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards));
			showNotification('🗑️ Task dihapus! Fokus ke yang lain! 🎯', 'info')
		} else {
			doneCards.splice(data.index, 1)
			doneCards = doneCards
			localStorage.setItem('doneCards', JSON.stringify(doneCards));
			showNotification('🗑️ Task selesai dihapus! Ruang untuk yang baru! 🌱', 'info')
		}
	}

	function showNotification(message, type = 'success') {
		notification = { show: true, message, type }
		setTimeout(() => {
			notification = { show: false, message: '', type: '' }
		}, 3000)
	}

	function handleEventMoveRight(event){
		let data = event.detail

		if (data.listName === "Tasks") {
			let cardToMove = taskcards.splice(data.index, 1)
			inProgressCards = [...inProgressCards, cardToMove[0]]
			taskcards = taskcards
			localStorage.setItem('taskcards', JSON.stringify(taskcards))
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards));
			showNotification('🚀 Task sedang dikerjakan! Semangat ya! 💪')
		} else if (data.listName === "In Progress") {
			let cardToMove = inProgressCards.splice(data.index, 1)
			doneCards = [...doneCards, cardToMove[0]]
			inProgressCards = inProgressCards
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards))
			localStorage.setItem('doneCards', JSON.stringify(doneCards));
			showNotification('🎉 Yeay! Task selesai! Kamu hebat! ✨')
		} 
	}

	function handleEventMoveLeft(event){
		let data = event.detail

		if (data.listName === "In Progress") {
			let cardToMove = inProgressCards.splice(data.index, 1)
			taskcards = [...taskcards, cardToMove[0]]
			inProgressCards = inProgressCards
			localStorage.setItem('taskcards', JSON.stringify(taskcards))
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards));
			showNotification('🔄 Task dikembalikan ke daftar tugas! 📝')
		} else if (data.listName === "Done") {
			let cardToMove = doneCards.splice(data.index, 1)
			inProgressCards = [...inProgressCards, cardToMove[0]]
			doneCards = doneCards
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards))
			localStorage.setItem('doneCards', JSON.stringify(doneCards));
			showNotification('⚡ Task kembali dikerjakan! Ayo selesaikan! 🎯')
		} 
	}
	
	function exportData() {
		const data = {
			taskcards,
			inProgressCards,
			doneCards,
			exportedAt: new Date().toISOString()
		};
		
		const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
		const url = URL.createObjectURL(blob);
		const a = document.createElement('a');
		a.href = url;
		a.download = `todoNiQ-backup-${new Date().toISOString().split('T')[0]}.json`;
		a.click();
		URL.revokeObjectURL(url);
		
		showNotification('📥 Data berhasil diexport!', 'success');
	}
	
	function importData() {
		const input = document.createElement('input');
		input.type = 'file';
		input.accept = 'application/json';
		input.onchange = (e) => {
			const file = e.target.files[0];
			const reader = new FileReader();
			reader.onload = (event) => {
				try {
					const data = JSON.parse(event.target.result);
					taskcards = data.taskcards || [];
					inProgressCards = data.inProgressCards || [];
					doneCards = data.doneCards || [];
					
					localStorage.setItem('taskcards', JSON.stringify(taskcards));
					localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards));
					localStorage.setItem('doneCards', JSON.stringify(doneCards));
					
					showNotification('📤 Data berhasil diimport!', 'success');
				} catch (error) {
					showNotification('❌ Error: File tidak valid!', 'info');
				}
			};
			reader.readAsText(file);
		};
		input.click();
	}

</script>

<div class="app-container">
	{#if notification.show}
		<div class="notification notification-{notification.type}">
			{notification.message}
		</div>
	{/if}
	
	<header class="app-header">
		<div class="logo">
			<img src="/images/logo.jpeg" alt="TodoNiQ Logo" class="logo-image" />
			<h1 class="app-title">TodoNiQ</h1>
		</div>
		<div class="center-message">
			<div class="motivational-text">
				✨ Produktivitas dimulai dari sini! 🚀
			</div>
			<div class="sub-text">
				Organisir tugasmu dengan style 😎
			</div>
		</div>
		<div class="header-controls">
			<button class="theme-toggle" on:click={toggleTheme} title="Toggle Theme">
				{#if theme === 'dark'}
					<i class="fas fa-sun"></i>
				{:else}
					<i class="fas fa-moon"></i>
				{/if}
			</button>
			<div class="datetime-info">
				<div class="current-time">
					<i class="fas fa-clock"></i>
					{currentTime}
				</div>
				<div class="current-date">
					<i class="fas fa-calendar-alt"></i>
					{currentDate}
				</div>
			</div>
		</div>
	</header>
	
	<Statistics 
		taskcards={taskcards}
		inProgressCards={inProgressCards}
		doneCards={doneCards}
	/>
	
	<div class="toolbar">
		<div class="search-container">
			<i class="fas fa-search search-icon"></i>
			<input
				type="text"
				class="search-input"
				placeholder="🔍 Cari task..."
				bind:value={searchQuery}
			/>
		</div>
		<div class="filter-group">
			<button 
				class="filter-btn {filterPriority === 'all' ? 'active' : ''}"
				on:click={() => filterPriority = 'all'}
			>
				Semua
			</button>
			<button 
				class="filter-btn {filterPriority === 'high' ? 'active' : ''}"
				on:click={() => filterPriority = 'high'}
			>
				🔴 High
			</button>
			<button 
				class="filter-btn {filterPriority === 'medium' ? 'active' : ''}"
				on:click={() => filterPriority = 'medium'}
			>
				🟡 Medium
			</button>
			<button 
				class="filter-btn {filterPriority === 'low' ? 'active' : ''}"
				on:click={() => filterPriority = 'low'}
			>
				🟢 Low
			</button>
		</div>
	</div>
	
	<div class="utility-buttons">
		<button class="utility-btn" on:click={exportData}>
			<i class="fas fa-download"></i>
			Export Data
		</button>
		<button class="utility-btn" on:click={importData}>
			<i class="fas fa-upload"></i>
			Import Data
		</button>
	</div>
	
	<div class="columns">
		<CardListEnhanced 
			cards={filteredTaskcards} 
			listName={"Tasks"} 
			on:addCard={handleEventAddCard}
			on:deleteCard={handleEventDeleteCard}
			on:moveRight={handleEventMoveRight}
			on:moveLeft={handleEventMoveLeft}
		/>
		<CardListEnhanced 
			cards={filteredInProgress} 
			listName={"In Progress"} 
			on:addCard={handleEventAddCard}
			on:deleteCard={handleEventDeleteCard}
			on:moveRight={handleEventMoveRight}
			on:moveLeft={handleEventMoveLeft}
		/>
		<CardListEnhanced 
			cards={filteredDone} 
			listName={"Done"} 
			on:addCard={handleEventAddCard}
			on:deleteCard={handleEventDeleteCard}
			on:moveLeft={handleEventMoveLeft}
		/>
	</div>
	
	<TaskModal 
		bind:show={showModal}
		task={editingTask}
		listName={editingListName}
		on:add={handleEventAddCard}
		on:update={handleEventAddCard}
	/>
	
	<footer class="app-footer">
		<div class="footer-content">
			<div class="footer-text">
				Made with <span class="footer-heart">❤️</span> by TodoNiQ Team
			</div>
			<div class="footer-text">
				© 2024 TodoNiQ - Manage your tasks with style ✨
			</div>
		</div>
	</footer>
</div>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
	<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</svelte:head>
