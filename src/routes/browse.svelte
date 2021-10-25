<script>
    function viewResource() {
        let resourceType = document.querySelector("#resource-type").value;
        let resourceId = document.querySelector("#resource-id").value;

        if (resourceId === "") {
            alert("Enter a valid id or username");
            return;
        }

        location.href = `/${resourceType}/${resourceId}`;
    }
</script>

<h2>Browse Comments:</h2>

<select id="resource-type">
    <option value="users">Users</option>
    <option value="projects">Projects</option>
    <option value="studios">Studios</option>
</select>
<input type="text" id="resource-id" placeholder="id or username"/>
<button on:click={viewResource} class="btn-default primary">View</button>