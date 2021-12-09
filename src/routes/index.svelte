<script>
    if (auth.isLoggedIn) {
        window.location.href = "/browse";
    }
</script>

{#if !auth.isLoggedIn}
<h1 class="font-dosis">
    Magnifier
</h1>
<p class="text-3xl font-didactGothic">Magnifier is a comment reaction tool for <a href="https://scratch.mit.edu/">Scratch</a>. You can <a href="/auth">log in</a> to react to comments, and star them so that you can <a href="/starred">view them later</a>.</p>
{/if}
