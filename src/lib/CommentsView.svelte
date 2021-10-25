<script>
    export let url;
    export let id;

    import Comment from "$lib/Comment.svelte";

    async function getComments(page) {
        let response = await fetch(`${url}/${id}/${page}`);

        let commentsArray = await response.json();

        Array.prototype.push.apply(comments, commentsArray);

        return comments;
    }

    let comments = [];

    let page = 1;

    let promise = getComments(page);
</script>
   
{#await promise}
    <h1>
        Loading...
    </h1>
{:then comments}
    <div class="comments-container">
        <h2 class="text-center">
            Comments
        </h2>
        <ul class="mb-4" style="padding: 0;">
            {#each comments as comment}
                <li>
                    <Comment comment={comment}/>
                    <div class="comment-replies">
                        <ul class="ml-16 p-none">
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
        <div class="flex flex-row place-items-center w-full mb-4">
            <div class="flex-grow"/>
            <button on:click={async () => promise = getComments(++page)} class="btn-default primary">Load More</button>
            <div class="flex-grow"/>
        </div>
    </div>
{/await}