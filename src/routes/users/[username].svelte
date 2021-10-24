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

    import CommentsView from "$lib/CommentsView.svelte";

    async function getComments() {
        let response = await fetch(`https://localhost:5001/api/Comments/legacy/user/${username}/1`);

        return response;
    }

    let comments = getComments(); // promise
</script>

<CommentsView comments={comments}/>