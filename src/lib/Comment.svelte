<style>
    .comment {
        margin-bottom: 1rem;
    }

    .comment-content {
        margin: 0;
    }
</style>

<script>
    export let comment;
    export let isReply = false;

    import ReactionButton from "$lib/ReactionButton.svelte";
    
    async function getReactions(commentId) {
        let response = await fetch(`${import.meta.env.VITE_API_HOST}/api/Reactions/${comment.id}`);

        return response;
    }

    let promise = getReactions(); // promise

    async function react(emoji) {
        if (window.auth.isLoggedIn) {
            promise = await fetch(`${import.meta.env.VITE_API_HOST}/api/Reactions/${comment.id}/${emoji}`, {
                method: 'PUT',
                headers: {
                    sessionid: window.auth.sessionId
                }
            });
        }
        else {
            alert("You need to be signed in to react");
        }
    }

    let showReactionMenu = false;

    window.addEventListener("click", (e) => {
        if (!e.target.classList.contains(`hide-reaction-menu-ignore-${comment.id}`)) {
            showReactionMenu = false;
        }
    });
</script>

<div class="comment" class:reply={isReply}>
    <a href="/users/{comment.author.username}" on:click={() => location.href = `/users/${comment.author.username}`}>
        <img src={comment.author.image} alt={comment.author.username} class="float-left rounded mr-2" style="width: 60px; height: 60px;"/>
    </a>
    <div class="flex flex-col">
        <a href="/users/{comment.author.username}" on:click={() => location.href = `/users/${comment.author.username}`}>
            {comment.author.username}
        </a>
        <div class="comment-body">
            <p class="comment-content">
                {comment.content}
            </p>
            <div class="flex flex-row mt-4">
                <span>
                    <span class="text-tiny">
                        {comment.id}
                    </span>
                </span>
                <div class="flex flex-grow"></div>
                <span>
                    {#await promise then response}
                        {#await response.json() then reactions}
                            <ul class="inline-block">
                                {#each reactions as reaction}
                                    {#if reaction.reactions.length > 0}
                                        <ReactionButton reaction={reaction} click={react}/>
                                    {/if}
                                {/each}
                            </ul>
                            {#if showReactionMenu}
                                <ul class="reaction-menu">
                                    {#each reactions as reaction}
                                        <ReactionButton reaction={reaction} click={react}/>
                                    {/each}
                                </ul>
                            {/if}
                            <button on:click={() => showReactionMenu = !showReactionMenu} class="hide-reaction-menu-ignore-{comment.id} mr-1">
                                😳
                            </button>
                            <!-- svelte-ignore missing-declaration -->
                            <span class="text-tiny">
                                {moment(comment.datetime_created).fromNow()}
                            </span>
                        {/await}
                    {/await}
                </span>
            </div>
        </div>
    </div>
</div>