<script context="module">
    export async function load(ctx) {
        let data = ctx.page.params;

        return { props: { username: data.username } }
    }
</script>

<script>
    export let username;

    import CommentsView from "$lib/CommentsView.svelte";
    import VerticalSeparator from "$lib/VerticalSeparator.svelte";

    async function getUser() {
        let response = await fetch(`${import.meta.env.VITE_API_HOST}/api/Scratch/users/${username}`);

        return response;
    }

    let promise = getUser();

    async function getOcularProfile() {
        let response = await fetch(`https://my-ocular.jeffalo.net/api/user/${username}`);
        
        return await response.json();
    }

    let ocular = getOcularProfile();
</script>

{#await promise then result}
    {#await result.json() then user}
        <div class="lg:justify-center lg:items-center w-full mb-4">
            <div class="flex flex-col lg:justify-center lg:items-center lg:w-2/3 lg:mx-auto" style="max-width: 900px;">
                <div class="flex flex-wrap my-4 w-full">
                    <a href="/users/{username}" class="mr-2 my-auto">
                        <img src={user.profile.images.big} alt={user.username} class="rounded my-auto" style="max-width: 250px;"/>
                    </a>
                    <div class="flex flex-col flex-grow">
                        <div class="flex flex-wrap">
                            <h1>{user.username}</h1>
                            <div class="flex-grow"></div>
                            <a href="https://scratch.mit.edu/users/{user.username}" target="_blank" class="btn-default primary">View on Scratch</a>    
                        </div>
                        <div class="flex flex-wrap -mt-2">
                            <h6>{moment(user.history.joined).fromNow()}</h6>
                        </div>
                        <div class="flex flex-wrap">
                            <h6>{user.profile.country}</h6>
                            <VerticalSeparator/>
                            {#await ocular then ocularProfile}
                                <h6 class="italic mr-1">{ocularProfile.status}</h6>
                                <span class="align-bottom ml-1 mt-3" style="width: 10px; height: 10px; background-color: {ocularProfile.color}; border-radius: 50%;"></span>
                            {/await}
                        </div>
                    </div>
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