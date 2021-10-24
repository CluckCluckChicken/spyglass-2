<script>
    export let comments;

    import Comment from "$lib/Comment.svelte";
</script>

{#await comments}
    <h1>
        Loading...
    </h1>
{:then response}
    {#if response.ok}
        {#await response.json() then comments}
            <ul style="padding: 0;" class="comments-container">
                {#each comments as comment}
                    <li>
                        <Comment comment={comment}/>
                        <div class="comment-replies">
                            <ul style="margin-left: 2rem; padding: 0;">
                                {#each comment.replies as reply}
                                    <li>
                                        <Comment comment={reply} isReply/>
                                    </li>
                                {/each}
                            </ul>
                        </div>
                    </li>
                {/each}
            </ul>
        {/await}
    {:else}
        <p>
            Failed to get comments: {response.status}
        </p>
    {/if}
{/await}