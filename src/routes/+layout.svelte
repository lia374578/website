<script lang="ts">
import BackgroundEffects from "$lib/components/BackgroundEffects.svelte";
import favicon from "$lib/assets/favicon.ico";
import NavLink from "$lib/components/NavLink.svelte";
import { onMount } from "svelte";

let { children } = $props();
let activeSection = $state("about");
let isMenuOpen = $state(false);

const sections = [
    { id: "about", label: "About" },
    { id: "work", label: "Work" },
    { id: "projects", label: "Projects" },
    { id: "education", label: "Education" },
    { id: "skills", label: "Skills" },
];

const toggleMenu = () => {
    isMenuOpen = !isMenuOpen;
};

const scrollTo = (id: string) => {
    const el = document.getElementById(id);
    el?.scrollIntoView({ behavior: isMenuOpen ? "instant" : "smooth" });
    isMenuOpen = false;
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

<button class="menu-btn" onclick={toggleMenu} aria-label="Toggle Menu">
    <span class="ripple"></span>
    {#if isMenuOpen}
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
        </svg>
    {:else}
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="3" y1="12" x2="21" y2="12"></line>
            <line x1="3" y1="6" x2="21" y2="6"></line>
            <line x1="3" y1="18" x2="21" y2="18"></line>
        </svg>
    {/if}
</button>


<div class="app-container">
    <BackgroundEffects {activeSection} />

    <div class="mobile-overlay" class:expanded={isMenuOpen}>
        <nav class="mobile-nav">
            {#each sections as section}
                <NavLink id={section.id} label={section.label} {activeSection} onScroll={scrollTo}/>
            {/each}
        </nav>
    </div>

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

    <div class="rightbar"></div>
</div>

<style>
	:global(body) {
		margin: 0;
		background: #111;
		color: #fff;
		font-family: sans-serif;
	}
	
	.app-container {
		display: grid;
	    grid-template-columns: 1fr 50% 1fr;
		width: 100%;
		min-height: 100vh;
    }

    .menu-btn {
        display: none;
        position: fixed;
        top: 1rem;
        left: 1rem;
        z-index: 200;
        background: #111;
        border: 1px solid rgba(255, 255, 255, 0.1);
        color: white;
        cursor: pointer;
        width: 55px;
        height: 55px;
        border-radius: 50%;
        overflow: hidden;
        transition: background-color 0.3s ease;
        line-height: 0;
        align-items: center;
        justify-content: center;
    }

    .mobile-overlay {
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 0;
        background: #111;
        z-index: 150;
        overflow: hidden;
        transition: height 0.5s ease-in-out;
    }

    .mobile-overlay.expanded {
        height: 100vh;
    }

    .mobile-nav {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100vh;
        gap: 2rem;
    }

    .mobile-nav :global(.nav-item) {
        font-size: 1.5rem;
        letter-spacing: 0.2em;
    }

    .mobile-nav :global(.nav-item.active) {
        transform: translateX(0);
    }

	.sidebar {
		position: sticky;
		top: 40%;
		height: fit-content;
        padding-right: 2rem;
        justify-content: flex-end;
		display: flex;
	}
	
	.nav-links {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
		align-items: flex-end;
	}
	
	main {
		width: 100%;
		border-left: 1px solid rgba(255, 255, 255, 0.1);
		padding-left: 3rem;
        padding-right: 3rem;
	}

    .rightbar {
	    visibility: hidden;
		pointer-events: none;
    }

    @media (max-width: 48rem) {
        .menu-btn {
            display: flex;
        }

        .menu-btn:hover {
            background-color: #222;
        }

        .menu-btn:active::after {
            content: "";
            position: absolute;
            width: 100%;
            height: 100%;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 50%;
            animation: ripple-effect 0.4s ease-out;
        }

        @keyframes ripple-effect {
            0% {
                transform: scale(0);
                opacity: 1;
            }
            100% {
                transform: scale(2);
                opacity: 0;
            }
        }

        .mobile-overlay {
            display: block;
        }

		.sidebar, .rightbar {
			display: none;
		}

		main {
            width: 100%;
			border-left: none;
			padding-left: 2rem;
			padding-right: 2rem;
            box-sizing: border-box;
		}

		.app-container {
			display: block;
        }
	}
</style>
