<script>
    import { createEventDispatcher } from 'svelte';
    export let isChecked = false;
    export let name;

    const dispatch = createEventDispatcher();
    const change = () => dispatch('change');
</script>

<input type="checkbox" bind:checked="{isChecked}" name="{name}" id="{name}" on:change={change} /> - <label for="{name}"><slot></slot></label>