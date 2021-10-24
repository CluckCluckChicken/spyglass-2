<style>
    ul {
        list-style-type: none;
    }
 </style>

<script context="module">
    export async function load(ctx) {
        let data = ctx.page.params;

        return { props: { username: data.username } }
    }
</script>

<script>
    export let username;

    import Comment from "$lib/Comment.svelte";

    async function getComments() {
        let response = await fetch(`https://localhost:5001/api/Comments/legacy/user/${username}/1`);

        return response;
    }

    let comments = getComments(); // promise
</script>

<h1>
    Comments
</h1>

{#await comments}
    <h1>
        Loading...
    </h1>
    {:then response}
    {#if response.ok}
        {#await response.json() then comments}
            <ul style="padding: 0;">
                {#each comments as comment}
                    <li>
                        <Comment comment={comment}/>
                        <div class="comment-replies">
                            <ul style="margin-left: 2rem; padding: 0;">
                                {#each comment.Replies as reply}
                                    <li>
                                        <Comment comment={reply}/>
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