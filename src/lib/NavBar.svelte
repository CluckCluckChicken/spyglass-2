<script>
    import NavBarButton from "$lib/NavBarButton.svelte";
</script>

<nav class="flex justify-between bg-darkGrey w-full px-5 py-2">
    <!-- svelte-ignore a11y-missing-content -->
    <a href="/" class="mr-5" style="background-image: url('/images/website_wordmark.png'); background-repeat: no-repeat; background-position: center; background-size: contain; width: 250px;"></a>
    <NavBarButton href="/browse">Browse</NavBarButton>
    <NavBarButton href="/starred">Starred</NavBarButton>
    <div class="flex-grow" />
    {#if typeof window !== "undefined"}
        {#if window.auth.isLoggedIn}
            <NavBarButton href="/logout">Log out</NavBarButton>
            <!-- svelte-ignore a11y-missing-content -->
            <a href={`/users/${window.auth.user.username}`} class="rounded" style={`background-image: url(${window.auth.user.author.image}); background-repeat: no-repeat; background-position: center; background-size: contain; width: 96px;")`}/>
        {:else}
            <NavBarButton href="/auth">Log in</NavBarButton>
        {/if}
    {/if}
</nav>