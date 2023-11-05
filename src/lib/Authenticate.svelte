<script>
    let email = ''
    let password = ''
    let confirmPass = ''
    let error = false
    let register = false

    function handleAuthenticate() {
        if (!email || !password || (register && !confirmPass)) {
            error = true
            return
        }
    }

    function toggleRegister() {
        register = !register
    }
</script>

<div class="authContainer">
    <form>
        <h1>{register ? 'Register' : 'Login'}</h1>
        {#if error}
        <p class="error">The information is not correct</p>
        {/if}
        <label> 
            <p>email</p>
            <input type="email" placeholder="Email" bind:value={email}>
        </label>
        <label>
            <p>Password</p>
            <input type="password" placeholder="password" bind:value={password}>
        </label>
        {#if register}
        <label>
            <p>Confirm Password</p>
            <input type="password" placeholder="Confirm Password" bind:value={confirmPass}>
        </label>
        {/if}
        <button type="button">Submit</button>
    </form>
    <div class='options'>
        {#if register}
            <div>
                <p>Already have an account?</p>
                <p on:click={toggleRegister}>Login</p>
            </div>
        {:else}
            <div>
                <p>Don't have an account?</p>
                <p on:click={toggleRegister}>Sign Up</p>
            </div>
        {/if}
    </div>
</div>

<style>
    .authContainer {
        display: flex;
        flex-direction: column;
        align-items: start;
        justify-content: center;
        padding: 24px;
        width: 200px;
    }
    
    h1 {
        font-size: 40px;
        margin: 0;
    }

    form {
        display: flex;
        flex-direction: column;
        align-items: start;
        gap: 8px;
        width: 200px;
        max-width: 100%;
    }

    input {
        margin: 0;
        font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
        outline: none;
    }

    p {
        margin: 0;
    }

    button {
        color: #decff1;
        font-family: 'VT323', monospace;
        font-weight: normal;
        background: #132410;
        font-size: 16px;
    }

    .error {
        color: coral;
    }

    .options {
        display: flex;
        flex-direction: column;
        align-items: start;
        margin-top: 20px;
    }

    .options p {
        font-size: 12px;
        text-align: left;
    }
</style>