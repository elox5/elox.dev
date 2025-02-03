<script lang="ts">
    import { onMount } from "svelte";

    let headline: HTMLHeadingElement;

    onMount(() => {
        setTimeout(() => {
            headline.classList.remove("dim-left");
        }, 500);

        setTimeout(() => {
            headline.classList.remove("dim-right");
        }, 1000);
    });
</script>

<h1 bind:this={headline} class="dim-left dim-right">Hey, I'm Michał</h1>

<style>
    @property --gradient-color-left {
        syntax: "<color>";
        initial-value: #77ff77;
        inherits: false;
    }

    @property --gradient-color-right {
        syntax: "<color>";
        initial-value: #77ffff;
        inherits: false;
    }

    h1 {
        --gradient-color-left: #77ff77;
        --gradient-color-right: #77ffff;

        text-transform: uppercase;
        font-size: 10rem;

        background-image: linear-gradient(
            to right,
            var(--gradient-color-left),
            var(--gradient-color-right)
        );
        background-clip: text;
        color: transparent;

        transition-property: --gradient-color-left, --gradient-color-right;
        transition-duration: 1s;
        transition-timing-function: ease-in-out;

        &.dim-left {
            --gradient-color-left: #223333;
        }

        &.dim-right {
            --gradient-color-right: #333344;
        }
    }
</style>
