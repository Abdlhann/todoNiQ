<script>
	export let taskcards = [];
	export let inProgressCards = [];
	export let doneCards = [];

	$: totalTasks = taskcards.length + inProgressCards.length + doneCards.length;
	$: completionRate = totalTasks > 0 ? Math.round((doneCards.length / totalTasks) * 100) : 0;
	$: inProgressCount = inProgressCards.length;
	$: pendingCount = taskcards.length;
</script>

<div class="stats-container">
	<h2 style="margin: 0 0 1rem 0; color: var(--text-primary); font-size: 20px;">📊 Statistik Produktivitas</h2>
	<div class="stats-grid">
		<div class="stat-card">
			<div class="stat-icon">📝</div>
			<div class="stat-value">{totalTasks}</div>
			<div class="stat-label">Total Tasks</div>
		</div>
		<div class="stat-card">
			<div class="stat-icon">⚡</div>
			<div class="stat-value">{inProgressCount}</div>
			<div class="stat-label">In Progress</div>
		</div>
		<div class="stat-card">
			<div class="stat-icon">✅</div>
			<div class="stat-value">{doneCards.length}</div>
			<div class="stat-label">Completed</div>
		</div>
		<div class="stat-card">
			<div class="stat-icon">🎯</div>
			<div class="stat-value">{completionRate}%</div>
			<div class="stat-label">Completion Rate</div>
		</div>
	</div>
</div>
