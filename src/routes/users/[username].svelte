<script context="module">
    export async function load(ctx) {
        let data = ctx.page.params;

        return { props: { username: data.username } }
    }
</script>

<script>
    export let username;

    import CommentsView from "$lib/CommentsView.svelte";

    async function getUser() {
        let response = await fetch(`${import.meta.env.VITE_API_HOST}/api/Scratch/users/${username}`);

        return response;
    }

    let promise = getUser();
</script>

{#await promise then result}
    {#await result.json() then user}
        <div class="justify-center items-center w-full mb-4">
            <div class="flex flex-col justify-center items-center w-2/3 mx-auto" style="max-width: 900px;">
                <div class="flex flex-row my-4 w-full">
                    <!-- svelte-ignore a11y-missing-content -->
                    <a href={`/users/${username}`} class="rounded mr-2" style={`background-image: url(${user.profile.images.big}); background-repeat: no-repeat; background-position: center; background-size: contain; width: 72px;")`}/>
                    <h1>{user.username}</h1>
                    <div class="flex-grow"></div>
                    <a href="https://scratch.mit.edu/users/{user.username}" target="_blank" class="btn-default primary">View on Scratch</a>
                </div>
                <div class="flex flex-row w-full">
                    <div class="flex flex-col max-w-sm ml-4 -mt-4">
                        <div class="flex flex-col">
                            <h3>About me</h3>
                            <div class="overflow-y-auto break-words max-h-44">{user.profile.bio}</div>
                        </div>
                        <div class="flex flex-col">
                            <h3>What I'm working on</h3>
                            <div class="overflow-y-auto break-words max-h-44">{user.profile.status}</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    {/await}
{/await}

<CommentsView url="{import.meta.env.VITE_API_HOST}/api/Comments/legacy/user/{username}"/>