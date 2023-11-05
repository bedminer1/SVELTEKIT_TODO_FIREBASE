<script context='module'>

</script>

<script>
    let todoList = ['Do the groceries']
    let currentTodo = ''
    let error = false
    function addTodo() {
        
        if (!currentTodo) {
            error = true
        }
        todoList = [...todoList, currentTodo]
        currentTodo = ''
    }
</script>

<div class="mainContainer">
    <div class="headerContainer">
            <h1>Todo List</h1>
            <button>Save</button>
    </div>
    <div class={"enterTodo" + (error ? 'errorBorder' : '')}>
        <form on:submit|preventDefault={addTodo}>
            <input type="text" placeholder="Enter todo" bind:value={currentTodo}/> 
        </form>
    </div>
    <main>
        {#each todoList as todo, index}
            <div class='todo'><p>{index + 1}: {todo}</p></div>
        {/each}
    </main>
</div>

<style>
    .mainContainer {
        display: flex;
        flex-direction: column;
        min-height: 100vh;
        gap: 24;
        width: 100%;
        max-width: 1000px;
        margin: 0 auto;
    }

    .headerContainer {
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    main {
        display: flex;
        flex-direction: column;
        gap: 8px;
        margin-top: 20px;
    }

    .enterTodo {
        display: flex;
        align-items: stretch;
    }

    .errorBorder {
        border: coral 1px solid;
    }

    .enterTodo input {
        background-color: transparent;
        outline: none;
        border: none;
        padding: 14px;
        color: #decff1;
        border: grey dotted 1px;
    }

    .todo {
        display: flex;
        align-items: center;
        color: #decff1;
        background-color: rgb(24, 24, 24);
        width: 200px;
        height: 30px;
        padding: 12px;
    }
</style>