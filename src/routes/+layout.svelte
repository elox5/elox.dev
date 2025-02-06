<script lang="ts">
    import Fa from "svelte-fa";
    import { faMoon, faSun } from "@fortawesome/free-solid-svg-icons";

    import { onMount } from "svelte";
    import "../app.css";

    const { children } = $props();

    let currentTheme = $state("light");

    onMount(() => {
        currentTheme = window.matchMedia("(prefers-color-scheme: dark)").matches
            ? "dark"
            : "light";

        document.documentElement.setAttribute("data-theme", currentTheme);
    });

    function toggleTheme() {
        currentTheme = currentTheme === "light" ? "dark" : "light";

        document.documentElement.setAttribute("data-theme", currentTheme);
    }
</script>

<div class="app">
    <nav>
        <a href="/">Home</a>
        <a href="/contact">Contact</a>
        <div class="spacer"></div>
        <button
            class="theme-toggle"
            onclick={toggleTheme}
            aria-label="Toggle theme"
        >
            {#if currentTheme === "light"}
                <Fa icon={faMoon} size="2x" class="theme-icon" />
            {:else}
                <Fa icon={faSun} size="2x" class="theme-icon" />
            {/if}
        </button>
        <p>&copy; {new Date().getFullYear()}</p>
    </nav>
    <main>
        {@render children()}
    </main>
</div>

<style>
    .app {
        height: 100vh;

        display: flex;
        flex-direction: row;
        align-items: stretch;
        justify-content: center;
    }

    nav {
        height: 100%;
        width: 10rem;
        flex-shrink: 0;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: start;
        gap: 1rem;

        padding: 2rem;

        backdrop-filter: invert(1) blur(10px);

        font-size: 1.2rem;

        color: light-dark(#ddd, #222);
    }

    main {
        height: 100%;
        flex-grow: 1;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        padding: 50px;
    }

    a {
        color: inherit;
    }

    p {
        margin: 0;
    }

    .spacer {
        flex-grow: 1;
    }

    .theme-toggle {
        border: none;
        background: none;
        cursor: pointer;

        width: 2rem;
        height: 2rem;

        padding: 0;

        color: inherit;

        transition: color 0.1s ease;

        &:hover {
            color: #77ff77;
        }
    }

    :global(.theme-icon) {
        width: 100%;
        height: 100%;

        transition: scale 0.2s cubic-bezier(0.34, 1.56, 0.64, 1);

        scale: 1;

        &:hover {
            scale: 1.2;
        }
    }
</style>
