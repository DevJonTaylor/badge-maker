<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    export let textName;
    export let value;
    export let disabled = false;
    const keyup = () => dispatch('keyup');
</script>

<input bind:value={value} on:keyup={keyup} name={textName} id={textName} disabled={disabled} />
<label for={textName}><slot></slot></label>