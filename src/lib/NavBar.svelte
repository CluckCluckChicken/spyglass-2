<script>
    import NavBarButton from "$lib/NavBarButton.svelte";
</script>

<nav class="flex flex-wrap justify-between bg-darkGrey w-full px-5 py-2">
    <!-- svelte-ignore a11y-missing-content -->
    <a href="/" class="my-auto mr-5">
        <img src="/images/website_wordmark.png" alt="Magnifier" style="max-width: 250px"/>
    </a>
    <NavBarButton href="/browse">Browse</NavBarButton>
    <NavBarButton href="/starred">Starred</NavBarButton>
    <div class="flex-grow" />
    {#if typeof window !== "undefined"}
        {#if window.auth.isLoggedIn}
            <NavBarButton href="/logout">Log out</NavBarButton>
            <a href="/users/{window.auth.user.username}">
                <img src={window.auth.user.author.image} alt={window.auth.user.username} class="rounded mr-2" style="max-width: 250px;"/>
            </a>
        {:else}
            <NavBarButton href="/auth">Log in</NavBarButton>
        {/if}
    {/if}
</nav>