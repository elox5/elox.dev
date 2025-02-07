<script lang="ts">
    import {
        faExternalLink,
        faSpinner,
    } from "@fortawesome/free-solid-svg-icons";
    import Fa from "svelte-fa";

    type ProjectData = {
        name: string;
        source?: string;
        link?: string;
        image: string;
        languages: string[];
    };

    type LanguageData = {
        id: string;
        name: string;
        short: string;
        color: string;
    };

    type Project = {
        name: string;
        source?: string;
        link?: string;
        image: string;
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
                link: data.link,
                image: data.image,
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
    <div class="project-grid">
        {#each projects as project}
            <div
                style="background-image: url('{project.image}');"
                class="project-entry"
            >
                <div class="blur-mask"></div>

                <h2 class="project-title">{project.name}</h2>

                <a
                    class="project-link-overlay"
                    href={project.link ?? project.source ?? ""}
                    target="_blank"
                    aria-label="View project source"
                ></a>

                {#if project.link && project.source}
                    <a
                        class="project-source-link"
                        href={project.source}
                        target="_blank"
                    >
                        Source <Fa icon={faExternalLink} size="xs" />
                    </a>
                {/if}

                <div class="project-languages">
                    {#each project.languages as language}
                        <span
                            style="color: {language.color};"
                            title={language.name}
                        >
                            {language.short}
                        </span>
                    {/each}
                </div>
            </div>
        {/each}
    </div>
{:catch error}
    <p style="color: red">{error}</p>
{/await}

<style>
    a {
        color: light-dark(#222, #ddd);
    }

    .project-grid {
        --bounce-easing: cubic-bezier(0.34, 1.56, 0.64, 1);

        width: 100%;

        height: 100%;
        margin: 3rem;

        display: grid;
        grid-template-columns: repeat(auto-fit, 300px);
        align-content: center;
        justify-content: center;
        gap: 1rem;
    }

    .project-grid:has(:hover) {
        > .project-entry:not(:hover) {
            scale: 0.8;
            opacity: 0.5;
        }
        & .blur-mask {
            --blur-strength: 5px;
            --brightness: 0.8;
        }
    }

    .project-entry {
        position: relative;

        width: 300px;
        height: 200px;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        padding: 1rem;

        background-size: cover;
        background-position: center;

        border-radius: 8px;

        overflow: hidden;

        user-select: none;

        border: 1px solid light-dark(#22222222, #ffffff22);

        transition:
            scale 0.5s var(--bounce-easing),
            opacity 0.3s ease;

        &:hover {
            border-width: 0;

            & > .project-title {
                opacity: 1;
            }

            & > .project-source-link {
                opacity: 1;
            }
        }
    }

    .project-title {
        font-size: 2rem;

        opacity: 0;
        transition: opacity 0.3s ease;

        z-index: 1;

        pointer-events: none;
    }

    .project-link-overlay {
        position: absolute;
        inset: 0;

        &::after {
            content: "";
            background-color: transparent;
        }
    }

    .project-source-link {
        position: absolute;
        top: 1rem;
        right: 1rem;

        opacity: 0;

        transition: opacity 0.3s ease;
    }

    .project-languages {
        position: absolute;
        bottom: 1rem;
        right: 1rem;

        display: flex;
        gap: 0.5rem;

        font-weight: bold;
    }

    .blur-mask {
        position: absolute;
        inset: 0;

        --blur-strength: 0px;
        --brightness: 1;

        backdrop-filter: blur(var(--blur-strength))
            brightness(var(--brightness));

        transition: backdrop-filter 0.3s ease;
    }
</style>
