<script>
    async function getAuthCode() {
        let response = await fetch(`${import.meta.env.VITE_API_HOST}/api/Auth/code`);

        return response;
    }

    async function completeAuth(authCode) {
        let response = await fetch(`${import.meta.env.VITE_API_HOST}/api/Auth/token?code=${authCode}`);
        let sessionId = await response.text();

        window.auth = {
            isLoggedIn: true,
            sessionId: sessionId,
            user: await (await fetch(`${import.meta.env.VITE_API_HOST}/api/Users`, {
                headers: {
                    sessionid: sessionId
                }
            })).json()
        }

        localStorage.setItem("sessionId", window.auth.sessionId);

        location.href = "/";
    }

    let promise = getAuthCode();
</script>

<h2>Log in to Magnifier</h2>
<h3>Your auth code is:</h3>
{#await promise}
    <h1>
        Loading...
    </h1>
{:then response} 
    {#await response.text() then authCode}
        <div class="flex flex-wrap">
            <h5>{authCode}</h5>
            <button on:click={() => navigator.clipboard.writeText(authCode)} class="btn-default">Copy</button>
        </div>
        <h3>Comment the code in the authentication Scratch project, then press "Done".</h3>
        <div class="flex flex-row ml-2">
            <button on:click={() => window.open("https://scratch.mit.edu/projects/534514916/", "Auth Project", "width=512,height=402")} class="btn-default primary">Show Project</button>
            <button on:click={() => completeAuth(authCode)} class="btn-default primary">Done</button>
        </div>
    {/await}
{/await}