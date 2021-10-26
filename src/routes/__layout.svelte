<script>
    import '../tailwind.css'

    import NavBar from "$lib/NavBar.svelte";

    function isValid(input) {
        return input !== "" && input !== undefined && input !== null;
    }

    async function fetchUser() {
        return await (await fetch(`${import.meta.env.VITE_API_HOST}/api/Users`, {
            headers: {
                sessionid: localStorage.getItem("sessionId")
            }
        })).json()
    }

    async function setAuth() {
        if (typeof window !== "undefined") {
            if (window.auth === undefined) {
                window.auth = {
                    isLoggedIn: isValid(localStorage.getItem("sessionId")),
                    sessionId: localStorage.getItem("sessionId"),
                    user: isValid(localStorage.getItem("sessionId")) ? await fetchUser() : "null"
                };
            }
        }
    }

    let promise = setAuth();
</script>

<svelte:head>
  <title>Magnifier</title>
</svelte:head>

{#await promise then response}
    <NavBar/>
    <div class="mx-4">
        <slot/>
    </div>
{/await}
