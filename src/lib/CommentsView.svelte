<script>
    export let url;
    export let noPage;

    import Comment from "$lib/Comment.svelte";

    async function getComments(page) {
        if (noPage) {
            page = "";
        }

        let response = await fetch(`${url}/${page}`, {
            headers: {
                sessionId: window.auth.sessionId
            }
        });

        let commentsArray = await response.json();

        Array.prototype.push.apply(comments, commentsArray);

        if (window.auth.isLoggedIn) {
            let stars = await fetch(`${import.meta.env.VITE_API_HOST}/api/Stars`, {
                headers: {
                    sessionid: window.auth.sessionId
                }
            });

            userStars = await stars.json();
        }
        else {
            userStars = [];
        }

        return comments;
    }

    let comments = [];

    let page = 1;
    
    let userStars;

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
                    <Comment comment={comment} userStars={userStars}/>
                    <div class="comment-replies">
                        <ul class="ml-16 p-none">
                            {#each comment.replies as reply}
                                <li>
                                    <Comment comment={reply} userStars={userStars} isReply/>
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