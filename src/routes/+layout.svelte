<script lang="ts">
import favicon from "$lib/assets/favicon.svg";
import NavLink from "$lib/components/NavLink.svelte";
import { onMount } from "svelte";

let { children } = $props();
let activeSection = $state("about");

const sections = [
    { id: "about", label: "About" },
    { id: "work", label: "Work" },
    { id: "projects", label: "Projects" },
    { id: "skills", label: "Skills" },
];

const scrollTo = (id: string) => {
    const el = document.getElementById(id);
    el?.scrollIntoView({ behavior: "smooth" });
};

onMount(() => {
    const observer = new IntersectionObserver(
        (entries) => {
            entries.forEach((entry) => {
                if (entry.isIntersecting) {
                    activeSection = entry.target.id;
                }
            });
        },
        { threshold: 0.5 },
    );

    sections.forEach((s) => {
        const el = document.getElementById(s.id);
        if (el) observer.observe(el);
    });

    return () => observer.disconnect();
});
</script>

<svelte:head>
	<link rel="icon" href={favicon}>
</svelte:head>

<div class="app-container">
	<aside class="sidebar">
		<nav class="nav-links">
			{#each sections as section}
			<NavLink id={section.id} label={section.label} {activeSection} onScroll={scrollTo}/>
			{/each}
		</nav>
	</aside>

	<main>
		{@render children()}
	</main>
</div>

<style>
	:global(body) {
		margin: 0;
		background: #111;
		color: #fff;
		font-family: sans-serif;
	}
	
	.app-container {
		display: flex;
		justify-content: center;
		max-width: 1200px;
		margin: 0 auto;
		position: relative;
	}
	
	.sidebar {
		position: sticky;
		top: 40%;
		height: fit-content;
		padding-right: 2rem;
	}
	
	.nav-links {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
		align-items: flex-end;
	}
	
	main {
		flex: 1;
		border-left: 1px solid rgba(255, 255, 255, 0.1);
		padding-left: 3rem;
	}
</style>
