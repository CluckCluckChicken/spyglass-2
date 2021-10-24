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

    window.addEventListener("click", (e) => {
        if (!e.target.classList.contains(`hide-reaction-menu-ignore-${comment.id}`)) {
            showReactionMenu = false;
        }
    });
</script>

<div class="comment" class:reply={isReply}>
    <a href="https://scratch.mit.edu/users/{comment.author.username}">
        <img src={comment.author.image} alt={comment.author.username} class="float-left rounded mr-2" style="width: 60px; height: 60px;"/>
    </a>
    <div class="flex flex-col">
        <a href="https://scratch.mit.edu/users/{comment.author.username}">
            {comment.author.username}
        </a>
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
                            <button on:click={() => showReactionMenu = !showReactionMenu} class="hide-reaction-menu-ignore-{comment.id}">
                                😳
                            </button>
                        {/await}
                    {/await}
                    <span>
                        {comment.datetime_created}
                    </span>
                </span>
            </div>
        </div>
    </div>
</div>