<script lang="ts">
let { id, label, activeSection, onScroll } = $props();
</script>

<button 
	onclick={() => onScroll(id)} 
	class:active={activeSection === id}
	class="nav-item"
>
	{label}
</button>

<style>
	.nav-item {
		background: none;
		border: none;
		padding: 0;
		color: #555;
		cursor: pointer;
		font-size: 0.9rem;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		transition: all 0.3s ease;
		white-space: nowrap;
	}

	.nav-item.active {
		color: #fff;
		transform: translateX(-5px);
	}

	.nav-item:hover {
		color: #aaa;
	}
</style>
