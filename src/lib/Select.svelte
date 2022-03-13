<script>
    import { createEventDispatcher } from 'svelte';

    export let options;
    export let value;
    export let name;

    const dispatch = createEventDispatcher();
    const change = () => dispatch('change')
</script>

<select name="{name}" id="{name}" bind:value={value} on:change={change}>
    {#each options as option}
    <option value={option}>{option.split('-').map(item => `${item[0].toUpperCase()}${item.slice(1)}`).join(' ')}</option>
    {/each}
</select>
<label for="{name}"><slot></slot></label>