<script>
    import '$lib/global.css'
</script>

<div class='main-container'>
    <slot />
</div>

<style>
    .main-container {
        min-height: 100vh;
        margin: 1em;
        position: relative;
        display: flex;
        flex-direction: column;
    }
</style>