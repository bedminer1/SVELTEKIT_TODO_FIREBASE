<script>
    export let index
    export let todo
    export let editTodo
    export let removeTodo
</script>

<div class='todo'>
    <p>{index + 1}. {todo}</p>
    <div class="actions">
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
        <img on:click={() => {editTodo(index)}} src="/icons/edit.png" alt="edit">
        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <img on:click={() => {removeTodo(index)}} src="/icons/delete.png" alt="delete">
    </div>
</div>

<style>
        .todo {
        display: flex;
        align-items: center;
        justify-content: space-between;
        color: #decff1;
        background-color: rgb(24, 24, 24);
        width: 400px;
        height: 30px;
        padding: 12px;
        border-bottom: grey 1px solid;
        border-left: rgb(155, 78, 11) 4px solid;
    }

    .actions {
        display: flex;
        align-items: center;
        justify-content: space-between;
        width: 60px;
        gap: 8px;
        font-size: 1.3rem;
    }

    .actions img {
        width: 25px;
        color: white;
        cursor:pointer;
        -webkit-filter: grayscale(1) invert(1);
        filter: grayscale(1) invert(1);
    }

    .actions img:hover {
        border: 1px solid coral;
    }
</style>