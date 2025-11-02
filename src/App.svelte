<script>
	import CardList from './CardList.svelte'
	import { onMount } from 'svelte'

	let taskCardsLocalStorage = JSON.parse(localStorage.getItem('taskcards'));
	let inProgressCardsLocalStorage = JSON.parse(localStorage.getItem('inProgressCards'));
	let doneCardsLocalStorage = JSON.parse(localStorage.getItem('doneCards'));

	let taskcards = taskCardsLocalStorage ? taskCardsLocalStorage : []
	let inProgressCards = inProgressCardsLocalStorage ? inProgressCardsLocalStorage : []
	let doneCards = doneCardsLocalStorage ? doneCardsLocalStorage : []

	let currentTime = ''
	let currentDate = ''
	let notification = { show: false, message: '', type: '' }

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
		return () => clearInterval(interval)
	})

	function handleEventAddCard(event) {
		let data = event.detail
		if (data.listName === "Tasks") {
			taskcards = [...taskcards, { todo: data.todo }]
			localStorage.setItem('taskcards', JSON.stringify(taskcards));
			showNotification('📝 Task baru ditambahkan! Siap produktif! 🌟')
		} else if (data.listName === "In Progress") {
			inProgressCards = [...inProgressCards, { todo: data.todo }]
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards));
			showNotification('⚡ Task langsung dikerjakan! Mantap! 🔥')
		} else {
			doneCards = [...doneCards, { todo: data.todo, }]
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
	</header>
	<div class="columns">
		<CardList 
		cards={taskcards} 
		listName={"Tasks"} 
		on:addCard={handleEventAddCard}
		on:deleteCard={handleEventDeleteCard}
		on:moveRight={handleEventMoveRight}
		on:moveLeft={handleEventMoveLeft}
		>
	</CardList>
		<CardList 
		cards={inProgressCards} 
		listName={"In Progress"} 
		on:addCard={handleEventAddCard}
		on:deleteCard={handleEventDeleteCard}
		on:moveRight={handleEventMoveRight}
		on:moveLeft={handleEventMoveLeft}
		>
	</CardList>
		<CardList 
		cards={doneCards} 
		listName={"Done"} 
		on:addCard={handleEventAddCard}
		on:deleteCard={handleEventDeleteCard}
		on:moveLeft={handleEventMoveLeft}>
	</CardList>
	</div>
	
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

