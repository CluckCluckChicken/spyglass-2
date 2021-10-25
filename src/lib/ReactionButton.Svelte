<script>
    export let reaction;
    export let click;
</script>

<button on:click={async () => await click(reaction.emoji)} class="reaction-button" class:reacted={window.auth.isLoggedIn ? reaction.reactions.find(reaction => reaction.username === window.auth.user.username) !== undefined : false}>
    <span>{reaction.emoji}</span>
    <span>{reaction.reactions.length}</span>
</button>