<style>
    ul {
        list-style-type: none;
    }
 </style>

<script context="module">
    export async function load(ctx) {
        let data = ctx.page.params;

        return { props: { projectId: data.projectId } }
    }
</script>

<script>
    export let projectId;

    import CommentsView from "$lib/CommentsView.svelte";

    async function getComments() {
        let response = await fetch(`https://localhost:5001/api/Comments/sapi/potatophant/projects/${projectId}/1`);

        return response;
    }

    let comments = getComments(); // promise
</script>

<CommentsView comments={comments}/>