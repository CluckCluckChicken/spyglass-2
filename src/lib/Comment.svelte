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
    export let userStars;
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

    async function toggleStarred() {
        if (window.auth.isLoggedIn) {
            let response = await fetch(`${import.meta.env.VITE_API_HOST}/api/Stars`, {
                method: 'PUT',
                headers: {
                    sessionid: window.auth.sessionId,
                    "Accept": "application/json",
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(comment)
            });

            userStars = await response.json();
        }
        else {
            alert("You need to be signed in to star comments");
        }
    }
    
    function toggleReactionMenu(newValue) {        
        if (newValue) {
            showReactionMenu = true;

            reactionMenuFadeIn = true;

            setTimeout(() => reactionMenuFadeIn = false, 200);
        }
        else {
            reactionMenuFadeOut = true;

            setTimeout(() => { reactionMenuFadeOut = false; showReactionMenu = false; }, 200);
        }
    }

    let showReactionMenu = false;
    let reactionMenuFadeIn = false;
    let reactionMenuFadeOut = false;
    
    document.body.addEventListener("click", (e) => {
        if (!e.target.classList.contains(`hide-reaction-menu-ignore-${comment.id}`)) {
            toggleReactionMenu(false);
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
                {@html comment.content}
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
                            <ul class="reaction-menu" class:invisible={!showReactionMenu} class:fade-in={reactionMenuFadeIn} class:fade-out={reactionMenuFadeOut}>
                                {#each reactions as reaction}
                                    <ReactionButton reaction={reaction} click={react}/>
                                {/each}
                            </ul>
                            <button on:click={() => toggleReactionMenu(!showReactionMenu)} class="hide-reaction-menu-ignore-{comment.id} mr-1">
                                😳
                            </button>
                            <button on:click={toggleStarred} class="filter" class:grayscale={userStars.find(c => c.id === comment.id) === undefined}>
                                ⭐
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