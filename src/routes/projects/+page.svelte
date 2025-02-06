<script lang="ts">
    import {
        faExternalLink,
        faSpinner,
    } from "@fortawesome/free-solid-svg-icons";
    import Fa from "svelte-fa";

    type ProjectData = {
        name: string;
        source: string;
        languages: string[];
    };

    type LanguageData = {
        id: string;
        name: string;
        color: string;
    };

    type Project = {
        name: string;
        source: string;
        languages: LanguageData[];
    };

    async function fetch_projects(): Promise<Project[]> {
        const languages = (await fetch("/languages.json")
            .then((res) => res.json())
            .then((data) => data.languages)) as LanguageData[];

        const projects = (await fetch("/projects.json")
            .then((res) => res.json())
            .then((data) => data.projects)) as ProjectData[];

        return projects.map((data) => {
            return {
                name: data.name,
                source: data.source,
                languages: data.languages.map((id) => {
                    return languages.find((l) => l.id === id)!;
                }),
            };
        });
    }
</script>

{#await fetch_projects()}
    <p class="loading"><Fa icon={faSpinner} /> Loading projects...</p>
{:then projects}
    {#each projects as project}
        <h2>{project.name}</h2>
        <a href={project.source} target="_blank">
            Source <Fa icon={faExternalLink} size="xs" />
        </a>
        <div class="languages">
            {#each project.languages as language}
                <span style="color: {language.color};">
                    {language.name}
                </span>
            {/each}
        </div>
    {/each}
{:catch error}
    <p style="color: red">{error}</p>
{/await}

<style>
    a {
        color: light-dark(#222, #ddd);
    }

    .languages {
        margin: 1rem;

        display: flex;
        gap: 0.5rem;

        font-weight: bold;
    }
</style>
