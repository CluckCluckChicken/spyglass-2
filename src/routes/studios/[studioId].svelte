<script context="module">
    export async function load(ctx) {
        let data = ctx.page.params;

        return { props: { studioId: data.studioId } }
    }
</script>

<script>
    export let studioId;

    import CommentsView from "$lib/CommentsView.svelte";

    async function getStudio() {
        let response = await fetch(`${import.meta.env.VITE_API_HOST}/api/Scratch/studios/${studioId}`);

        return response;
    }

    let promise = getStudio();
</script>

{#await promise then result}
    {#await result.json() then studio}
        <div class="justify-center items-center w-full mb-4">
            <div class="flex flex-col justify-center items-center w-2/3 mx-auto" style="max-width: 900px;">
                <div class="flex flex-row my-4 w-full">
                    <!-- svelte-ignore a11y-missing-content -->
                    <a href={`/studios/${studioId}`} class="rounded mr-2" style={`background-image: url(${studio.image}); background-repeat: no-repeat; background-position: center; background-size: contain; width: 72px;")`}/>
                    <h1>{studio.title}</h1>
                    <div class="flex-grow"></div>
                    <a href="https://scratch.mit.edu/studios/{studioId}" target="_blank" class="btn-default primary">View on Scratch</a>
                </div>
            </div>
        </div>
    {/await}
{/await}

<CommentsView url="{import.meta.env.VITE_API_HOST}/api/Comments/sapi/studios" id={studioId}/>