<script>
import CommentsView from "$lib/CommentsView.svelte";

</script>
<div class="ml-2">
    <h2>Starred Comments:</h2>
    {#if auth.isLoggedIn}
    <h3>You can star comments by clicking ⭐ in the footer of the comment.</h3>
    {:else}
    <h3>You need to log in to star comments. After that, you can star comments by clicking ⭐ in the footer of the comment.</h3>
    {/if}
</div>

<CommentsView url="{import.meta.env.VITE_API_HOST}/api/Stars" noPage/>