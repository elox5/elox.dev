<script lang="ts">
    import "../app.css";

    const { children } = $props();
</script>

<div class="app">
    <nav>
        <a href="/">Home</a>
        <a href="#contact">Contact</a>
        <a href="#about">About</a>
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

        background-color: #fff;

        font-size: 1.2rem;
    }

    main {
        height: 100%;
        flex-grow: 1;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    a {
        text-decoration: none;

        color: #000;

        &::after {
            content: "";
            display: block;

            width: 100%;
            height: 2px;

            background-color: #000;

            transform: scaleX(0);
            transform-origin: bottom right;
            transition: transform 0.2s ease-out;
        }

        &:hover::after {
            transform: scaleX(1);
            transform-origin: bottom left;
        }
    }
</style>
