<script>
    export let reaction;
    export let click;
</script>

<button on:click={async () => await click(reaction.emoji)} class="reaction-button" class:reacted={reaction.reactions.find(reaction => reaction.username === "potatophant") !== undefined}>
    <span>{reaction.emoji}</span>
    <span>{reaction.reactions.length}</span>
</button>