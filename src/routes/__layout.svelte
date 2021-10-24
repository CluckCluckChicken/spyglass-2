<script>
    import '../tailwind.css'

    import NavBar from "$lib/NavBar.svelte";

    function isValid(input) {
        return input !== "" && input !== undefined && input !== null;
    }

    if (typeof window !== "undefined") {
        if (window.auth === undefined) {
            window.auth = {
                isLoggedIn: isValid(localStorage.getItem("sessionId")),
                sessionId: localStorage.getItem("sessionId")
            };
        }
    }
</script>

<NavBar/>

<slot />