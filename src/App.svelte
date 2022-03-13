<script>
  import Color from './lib/Color.svelte';
  import Checkbox from './lib/Checkbox.svelte';
  import Text from './lib/Text.svelte';
  import Select from './lib/Select.svelte';
import Range from './lib/Range.svelte';

  const staticUrl = 'https://img.shields.io/badge/';
  let href = './LICENSE';
  let text1 = 'license';
  let text2 = 'MIT';
  let logo = '';
  let logoColor = '#000000';
  // let logoWidth = '40';
  let color1 = '#00ff00';
  let color2 = '#ffffff';
  let isColor2;
  let isLogoStyles;
  let url = '';
  let md = '';
  let styles = ['none', 'plastic', 'flat', 'flat-square', 'for-the-badge', 'social'];
  let style = 'none';

  function updateUrl() {
    const c1 = color1.slice(1);
    const c2 = color2.slice(1);
    const queries = [];
    const badgeText = !text1 && !text2 
      ? `license-MIT-${c1}`
      : !text1
        ? `${encodeURIComponent(text2)}-${c1}`
        : !text2
          ? `${encodeURIComponent(text1)}-${c1}`
          : `${encodeURIComponent(text1)}-${encodeURIComponent(text2)}-${c1}`;
    
    if(isColor2) queries.push(`labelColor=${c2}`);
    if(style !== 'none') queries.push(`style=${style}`);
    if(logo) {
      queries.push(`logo=${logo}`);
      if(isLogoStyles) queries.push(`logoColor=${logoColor.slice(1)}`);
    }


    url = queries.length > 0 
      ? `${staticUrl}${badgeText}?${queries.join('&')}`
      : `${staticUrl}${badgeText}`

    md = !href ? `![${text1} ${text2}](${url})` : `[![${text1} ${text2}](${url})](${href})`;
  }

  updateUrl();
  
  function coypToClipboard(event) {
    const target = event.target;
    const input = document.querySelector('.hide-but-show');
    if(!target.matches('.click-copy-url') && !target.matches('.click-copy-md')) return
    input.value = !target.matches('.click-copy-url') ? md : url;
    if(!input.select) input.setSelectionRange(0, 99999); // For mobile
      else input.select();

      navigator.clipboard.writeText(input.value);
  }

  document.addEventListener('mousedown', pasteColor);

  function pasteColor(event) {
    if(!event.target.matches('[type="color"]')) return;
    if(event.button === 2) {
      switch(event.target.id) {
        case 'color1':
          navigator.clipboard.readText()
            .then(paste => color1 = paste)
            .then(() => updateUrl())
            .catch(console.error);
          break;
        case 'color2':
        navigator.clipboard.readText()
            .then(paste => color2 = paste)
            .then(() => updateUrl())
            .catch(console.error);
          break;
        case 'logo-color':
        navigator.clipboard.readText()
            .then(paste => logoColor = paste)
            .then(() => updateUrl())
            .catch(console.error);
          break;
        default:
          break;
      }
    }
  }
</script>

<main>
  <img src={url} alt="" /><br />
  <input class="hide-but-show" value={url} />
  <h3 on:click={coypToClipboard} class="click-copy-url">Click For URL: {url}</h3>
  <h3 on:click={coypToClipboard} class="click-copy-md">Click For Markdown: {md}</h3>
  <Text bind:value={href} on:keyup={updateUrl} textName="href">Link for Badge</Text>
  <br /><br />
  <Text bind:value={text1} on:keyup={updateUrl} textName="text1">Badge Text 1</Text>
  <br /><br />
  <Text bind:value={text2} on:keyup={updateUrl} textName="text2">Badge Text 2</Text>
  <br />
  <hr />
  <br />
  <Checkbox name="logo-styles" bind:isChecked={isLogoStyles} on:change={updateUrl}>Apply Logo Styles</Checkbox><br />
  <Text bind:value={logo} on:keyup={updateUrl} textName="logo">Logo</Text>
  {#if isLogoStyles}
  <br />
  <Color colorName="logo-color" on:keyup={pasteColor} bind:color="{logoColor}" on:change={updateUrl}>Logo Color</Color>
  <br />
  <!-- <Range name="logo-width" min={10} max={100} bind:value={logoWidth} on:change={updateUrl}>Logo Width</Range> -->
  {/if}
  <br />
  <hr />
  <br />
  <Select name="styles" options={styles} bind:value={style} on:change={updateUrl}>Style Options</Select>
  <br />
  <hr />
  <br />
  <Checkbox name="color2" bind:isChecked="{isColor2}" on:change={updateUrl} >Use Color 2</Checkbox>
  <br/>
  <Color bind:color="{color1}" colorName="color1" on:change={updateUrl}>Color 1</Color>
  {#if isColor2}
   - <Color colorName="color2" on:keyup={pasteColor} on:change={updateUrl} bind:color="{color2}">Color 2</Color>
  {/if}
</main>

<style>
  .click-copy-url, .click-copy-md {
    cursor: pointer;
  }

  .hide-but-show {
    width: 0;
    position: absolute;
    z-index: -1;
    bottom: 0;
    right: 0;
  }
</style>