<script>
    import NavBarButton from "$lib/NavBarButton.svelte";
</script>

<nav class="flex justify-between bg-darkGrey w-full px-5 py-2">
    <!-- svelte-ignore a11y-missing-content -->
    <a href="/" class="mr-5" style="background-image: url('/images/website_wordmark.png'); background-repeat: no-repeat; background-position: center; background-size: contain; width: 250px;"></a>
    <NavBarButton href="/browse">Browse</NavBarButton>
    <div class="flex-grow" />
    {#if typeof window !== "undefined"}
        {#if window.auth.isLoggedIn}
            <NavBarButton href="/logout">Log out</NavBarButton>
        {:else}
            <NavBarButton href="/auth">Log in</NavBarButton>
        {/if}
    {/if}
</nav>