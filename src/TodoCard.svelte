<script>
    import { createEventDispatcher } from 'svelte'
    import { slide } from 'svelte/transition'
    
    export let content, listName, index;

    const dispatch = createEventDispatcher()

    function handleDeleteCard(){
        dispatch('deleteCard', {index})
    }

    function handleMoveRight(){
        dispatch('moveRight', {index})
    }

    function handleMoveLeft(){
        dispatch('moveLeft', {index})
    }
</script>

<div class="card mb-3 has-background-primary-light" transition:slide>
    <div class="card-content">
        <div class="columns">
            <div class="column is-1">
                {#if listName != 'Tasks' }
                    <span class="icon has-text-primary" on:click={handleMoveLeft}>
                        <i class="fa fa-chevron-left"></i>
                    </span>
                {/if}
            </div>
            <div class="column is-9">{content}</div>
            <div class="column is-1" on:click={handleDeleteCard}>
                <span class="icon has-text-danger">
                    <i class="fa fa-trash"></i>
                </span>
            </div>
            <div class="column is-1">
                {#if listName != 'Done' }
                    <span class="icon has-text-primary" on:click={handleMoveRight}>
                        <i class="fa fa-chevron-right"></i>
                    </span>
                {/if}
            </div>
        </div>
    </div>
</div>