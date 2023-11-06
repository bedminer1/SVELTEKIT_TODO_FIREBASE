<script>
    import { authHandlers, authStore } from "../../store/store";
    // @ts-ignore
    import { getDoc, doc, setDoc } from 'firebase/firestore';
    import { db } from "$lib/firebase/firebase";
    import TodoItem from "$lib/TodoItem.svelte";

    // @ts-ignore
    /**
   * @type {any[]}
   */
    let todoList = []
    let currentTodo = ''
    let error = false
    let successfulSave = false

    authStore.subscribe(curr => {
        // @ts-ignore
        todoList = curr.data.todos
    })

    function addTodo() {
        
        if (!currentTodo) {
            error = true
        }
        // @ts-ignore
        todoList = [...todoList, currentTodo]
        currentTodo = ''
    }

    // @ts-ignore
    // @ts-ignore
    function editTodo(index) {
        // @ts-ignore
        let newTodoList = todoList.filter((val,i) => {
            return i !== index
        })

        // @ts-ignore
        currentTodo = todoList[index]
        todoList = newTodoList
    }

    // @ts-ignore
    function removeTodo(index) {
        // @ts-ignore
        let newTodoList = todoList.filter((val,i) => {
            return i !== index
        })

        todoList = newTodoList
    }

    // @ts-ignore
    async function saveTodos() {
        try {
            // @ts-ignore
            const userRef = doc(db, 'users', $authStore.user.uid)
            await setDoc(
                userRef,
                   { todos: todoList, }, {merge: true,}
            )
            setTimeout(() => {
                successfulSave = true
            }, 200);

            setTimeout(() => {
                successfulSave = false
            }, 1300);

        } catch (err) {
            console.log('There was an error saving information')
        }
    }
</script>

{#if !$authStore.loading}

<div class="mainContainer">
    <div class="headerContainer">
            <h1>Todo List</h1>
            <div class="btn-container">
                <button on:click={saveTodos} class:successfulSave={successfulSave}>
                    {#if successfulSave}
                        Done
                    {:else}
                        Save
                    {/if}
                </button>
                <button on:click={authHandlers.logout}>Logout</button>
            </div>
           
    </div>
    <div class={"enterTodo" + (error ? 'errorBorder' : '')}>
        <form on:submit|preventDefault={addTodo}>
            <input type="text" placeholder="Enter todo" bind:value={currentTodo}/> 
        </form>
    </div>
    <main>
        {#if todoList.length === 0}
            <p style="font-size: 25px;">Nothing To Do</p>
        {/if}
        {#each todoList as todo, index}
            <TodoItem todo={todo} index={index} removeTodo={removeTodo} editTodo={editTodo} />
        {/each}
    </main>
</div>
{:else}
    <p>Loading..</p>
{/if}

<style>
    .mainContainer {
        display: flex;
        flex-direction: column;
        min-height: 80vh;
        gap: 24;
        width: 100%;
        max-width: 1000px;
        margin: 0 auto;
    }

    .headerContainer {
        display: flex;
        align-items: center;
        justify-content: end;
        gap: 35%;
    }

    h1 {
        font-size: 50px;
        text-align: center;
    }

    button {
        background-color: rgb(21, 21, 21);
        width: auto;
        height: 30px;
        font-size: 20px;
        border: rgb(106, 106, 106) 1px solid;
        border-right: none;
        border-top: none;
        border-radius: 2px;
        padding: 3px;
    }

    .successfulSave {
        background-color: rgb(14, 101, 14);
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
        font-size: 30px;
        border: grey dotted 1px;
    }

</style>