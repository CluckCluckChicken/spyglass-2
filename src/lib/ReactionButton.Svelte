<script>
    export let reaction;
    export let click;
</script>

<button on:click={async () => await click(reaction.emoji)}>
    <span>{reaction.emoji}</span>
    <span>{reaction.reactions.length}</span>
</button>