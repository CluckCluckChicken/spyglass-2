<style>
    .comment {
        margin-bottom: 1rem;
    }

    .comment-body {
        border: 1px solid black;
    }

    .comment-content {
        margin: 0;
    }
</style>

<script>
    export let comment;

    import ReactionButton from "$lib/ReactionButton.svelte";
    
    async function getReactions(commentId) {
        let response = await fetch(`https://localhost:5001/api/Reactions/${comment.id}`);

        return response;
    }

    let promise = getReactions(); // promise

    async function react(emoji) {
        promise = await fetch(`https://localhost:5001/api/Reactions/${comment.id}/${emoji}`, {
            method: 'PUT',
            headers: {
                sessionid: "M:S:eaa555e5-055b-4f97-bcdc-c20a77c6eed4"
            }
        });
    }

    let showReactionMenu = false;
</script>

<div class="comment">
    <div>
        {comment.author.username}
    </div>
    <div class="comment-body">
        <p class="comment-content">
            {comment.content}
        </p>
        <div>
            <span>
                {comment.id}
            </span>
            <span style="float: right;">
                {#await promise then response}
                    {#await response.json() then reactions}
                        <span>
                            <ul>
                                {#each reactions as reaction}
                                    {#if reaction.reactions.length > 0}
                                        <ReactionButton reaction={reaction} click={react}/>
                                    {/if}
                                {/each}

                            </ul>
                        </span>
                        <span>
                            {#if showReactionMenu}
                                <div>
                                    <ul>
                                        {#each reactions as reaction}
                                            <ReactionButton reaction={reaction} click={react}/>
                                        {/each}
                                    </ul>
                                </div>
                            {/if}
                            <button on:click={() => showReactionMenu = !showReactionMenu}>
                                😳
                            </button>
                        </span>
                    {/await}
                {/await}
                <span>
                    {comment.datetime_created}
                </span>
            </span>
        </div>
    </div>
</div>