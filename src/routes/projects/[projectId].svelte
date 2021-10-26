<script context="module">
    export async function load(ctx) {
        let data = ctx.page.params;

        return { props: { projectId: data.projectId } }
    }
</script>

<script>
    export let projectId;

    import CommentsView from "$lib/CommentsView.svelte";

    async function getProject() {
        let response = await fetch(`${import.meta.env.VITE_API_HOST}/api/Scratch/projects/${projectId}`);

        return response;
    }

    let promise = getProject();
</script>

{#await promise then result}
    {#await result.json() then project}
        <div class="justify-center items-center w-full mb-4">
            <div class="flex flex-col justify-center items-center w-2/3 mx-auto" style="max-width: 900px;">
                <div class="flex flex-row my-4 w-full">
                    <!-- svelte-ignore a11y-missing-content -->
                    <a href={`/users/${project.author.username}`} class="rounded mr-2" style={`background-image: url(${project.author.profile.images.big}); background-repeat: no-repeat; background-position: center; background-size: contain; width: 72px;")`}/>
                    <h3>{project.title}</h3>
                    <div class="flex-grow"></div>
                    <a href="https://scratch.mit.edu/projects/{projectId}" target="_blank" class="btn-default primary">View on Scratch</a>
                </div>
                <div class="flex flex-row w-full">
                    <iframe src="https://scratch.mit.edu/projects/{projectId}/embed" class="rounded-lg" title="Project" allowtransparency="true" width="485" height="402" frameborder="0" scrolling="no" allowfullscreen></iframe>
                    <div class="flex flex-col max-w-sm ml-4 -mt-4">
                        {#if project.instructions !== ""}
                            <div class="flex flex-col">
                                <h3>Instructions</h3>
                                <div class="overflow-y-auto break-words max-h-44">{project.instructions}</div>
                            </div>
                        {/if}
                        <div class="flex flex-col">
                            <h3>Notes and Credits</h3>
                            <div class="overflow-y-auto break-words max-h-44" class:max-h-86={project.instructions === ""}>{project.description}</div>
                        </div>
                    </div>
                </div>
                <div class="flex flex-row w-full">
                    <img src="https://scratch.mit.edu/svgs/project/love-gray.svg" alt="Loves" class="w-8 mr-2">
                    <h6 class="mr-8 font-bold">{project.stats.loves}</h6>
                    <img src="https://scratch.mit.edu/svgs/project/fav-gray.svg" alt="Favourites" class="w-8 mr-2">
                    <h6 class="mr-8 font-bold">{project.stats.favourites}</h6>
                    <img src="https://scratch.mit.edu/svgs/project/remix-gray.svg" alt="Favourites" class="w-8 mr-2">
                    <h6 class="mr-8 font-bold">{project.stats.remixes}</h6>
                    <img src="https://scratch.mit.edu/svgs/project/views-gray.svg" alt="Views" class="w-8 mr-2">
                    <h6 class="mr-8 font-bold">{project.stats.views}</h6>
                    <div class="flex-grow"></div>
                    <!-- svelte-ignore missing-declaration -->
                    <h6>&copy; {moment(project.history.shared).format('MMMM D, Y')}</h6>
                </div>
            </div>
        </div>
        <CommentsView url="{import.meta.env.VITE_API_HOST}/api/Comments/sapi/{project.author.username}/projects/{projectId}"/>
    {/await}
{/await}