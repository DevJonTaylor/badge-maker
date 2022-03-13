<script>
  import { createEventDispatcher } from 'svelte';
  export let color = '#000000';
  export let colorName;
  const dispatch = createEventDispatcher();
  const change = () => dispatch('change');
  const mousedown = () => dispatch('mousedown')
</script>

<input type="color" name={colorName} id={colorName} bind:value="{color}" on:contextmenu|preventDefault={mousedown} on:change={change} /> 
  - <label for={colorName}><slot></slot>: {color}</label>