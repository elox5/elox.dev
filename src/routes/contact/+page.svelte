<script lang="ts">
    import Fa from "svelte-fa";
    import { faDiscord, faGithub } from "@fortawesome/free-brands-svg-icons";
    import { faEnvelope } from "@fortawesome/free-solid-svg-icons";
</script>

<h2>
    <a href="https://github.com/elox5" target="_blank">
        <Fa icon={faGithub} />
        github.com/elox5
    </a>
</h2>
<h2>
    <a href="mailto:mrys5@proton.me">
        <Fa icon={faEnvelope} />
        mrys5@proton.me
    </a>
</h2>
<h2>
    <p>
        <Fa icon={faDiscord} />
        .elox
    </p>
</h2>

<style>
    h2 {
        font-weight: normal;
    }

    a,
    p {
        display: flex;
        align-items: center;
        gap: 0.5rem;

        color: light-dark(#222, #ddd);
    }

    p {
        margin: 0;
    }
</style>
