<script>
    import { createEventDispatcher } from 'svelte';

    export let value;
    export let name;
    export let min = 0;
    export let max = 100;
    const dispatch = createEventDispatcher();
    const change = () => dispatch('change');
</script>

<input type="range" id={name} name={name} min={min} max={max} bind:value={value} on:change={change} />
<label for={name}><slot></slot>: {value}</label>