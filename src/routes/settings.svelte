<script>
    if (!window.auth.isLoggedIn) {
        location.href = "/";
    }
</script>

<h2 class="ml-2">Magnifier Settings:</h2>
<div class="flex flex-col">
    <div class="flex flex-col">
        <h3>Project player</h3>
        <div class="flex flex-row">
            <select value={window.auth.user.settings.projectPlayer}>
                <option value="scratch">Scratch</option>
                <option value="turbowarp">TurboWarp</option>
                <option value="none">None</option>
            </select>
        </div>
    </div>
    <div class="flex flex-wrap">
        <button class="btn-default primary">Save</button>
    </div>
</div>