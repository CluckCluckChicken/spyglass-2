<script>
    function logOut() {
        localStorage.setItem("sessionId", "");

        location.href = "/";
    }
</script>

<h1>Do you want to log out of Magnifier?</h1>

{#if typeof window !== "undefined"}
    <button on:click={logOut} class="btn-default primary">Yes</button>
    <button on:click={() => location.href = "/"} class="btn-default">No</button>
{/if}