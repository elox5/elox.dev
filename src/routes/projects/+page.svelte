<script lang="ts">
    import {
        faExternalLink,
        faSpinner,
        faStar,
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
        {#each projects as project, i}
            <div
                style="background-image: url('{project.image}');"
                class="project-entry"
                data-index={i}
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

                {#if i === 0}
                    <div class="featured-indicator">
                        <Fa
                            class="star-icon"
                            icon={faStar}
                            color="#ffdd7f"
                            size="lg"
                        />
                        <span class="featured-indicator-text">
                            Featured Project
                        </span>
                    </div>
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
        --ease-out-easing: cubic-bezier(0.22, 1, 0.36, 1);

        width: 100%;
        height: 100%;

        padding: 24px;

        display: grid;
        grid-template-columns: repeat(auto-fit, 300px);
        align-content: start;
        justify-content: center;
        gap: 1rem;

        overflow-y: scroll;

        mask-image: linear-gradient(
            to bottom,
            transparent 0px,
            black 24px,
            black calc(100% - 24px),
            transparent 100%
        );
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

            & .featured-indicator-text {
                width: 100%;
            }
        }
    }

    .project-entry[data-index="0"] {
        grid-column: span 2;
        grid-row: span 2;

        width: 100%;
        height: 100%;

        & > .project-title {
            font-size: 4rem;
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

    .featured-indicator {
        position: absolute;
        top: 1rem;
        left: 1rem;

        display: flex;
        flex-direction: row;
        align-items: end;
        gap: 10px;

        text-shadow: 2px 2px 2px #000000;
    }

    .featured-indicator :global(.star-icon) {
        filter: drop-shadow(2px 2px 2px #000000);
    }

    .featured-indicator-text {
        width: 0;
        white-space: nowrap;

        overflow: hidden;

        font-size: 1.1rem;

        transition: width 0.5s var(--ease-out-easing);
    }

    .project-languages {
        position: absolute;
        bottom: 1rem;
        right: 1rem;

        display: flex;
        gap: 0.5rem;

        font-weight: bold;

        text-shadow: 2px 2px 2px #000000;
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
