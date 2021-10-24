<style>
    ul {
        list-style-type: none;
    }
 </style>

<script context="module">
    export async function load(ctx) {
        let data = ctx.page.params;

        return { props: { studioId: data.studioId } }
    }
</script>

<script>
    export let studioId;

    import CommentsView from "$lib/CommentsView.svelte";

    async function getComments() {
        let response = await fetch(`https://localhost:5001/api/Comments/sapi/studios/${studioId}/1`);

        return response;
    }

    let comments = getComments(); // promise
</script>

<CommentsView comments={comments}/>