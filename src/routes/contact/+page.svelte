<script lang="ts">
    import Fa from "svelte-fa";
    import {
        faDiscord,
        faGithub,
        faItchIo,
    } from "@fortawesome/free-brands-svg-icons";
    import { faEnvelope } from "@fortawesome/free-solid-svg-icons";
</script>

<h2>Contact</h2>
<a class="underline-hover" href="mailto:mrys5@proton.me">
    <Fa icon={faEnvelope} />
    mrys5@proton.me
</a>
<p>
    <Fa icon={faDiscord} />
    .elox
</p>

<h2>Links</h2>
<a class="underline-hover" href="https://github.com/elox5" target="_blank">
    <Fa icon={faGithub} />
    github.com/elox5
</a>
<a class="underline-hover" href="https://elox5.itch.io" target="_blank">
    <Fa icon={faItchIo} />
    el0x.itch.io
</a>

<style>
    h2 {
        font-size: 2rem;
        margin-bottom: 0;
    }

    a,
    p {
        font-size: 1.2rem;
        margin: 1rem;

        display: flex;
        align-items: center;
        gap: 0.5rem;

        color: light-dark(#222, #ddd);

        transition: color 0.2s ease;
    }
</style>
