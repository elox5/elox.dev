<script lang="ts">
    import "../app.css";

    const { children } = $props();
</script>

<div class="app">
    <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
        <a href="/settings">Settings</a>
    </nav>
    <main>
        {@render children()}
    </main>
</div>

<style>
    .app {
        display: flex;
        flex-direction: row;
        align-items: center;
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
    }

    main {
        height: 100%;
        flex-grow: 1;
    }
</style>
