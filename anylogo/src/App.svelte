<script>
  import { onMount, onDestroy } from 'svelte';


  let url;
  let filename;
  let current_url;
  let input;

  onMount(()=>{input.focus();
  });
  onDestroy(()=>{URL.revokeObjectURL(filename)});

  async function getLogo(){
    if(url != undefined) {
      const res = await fetch(`https://logo.clearbit.com/${url}`);
      const blob = await res.blob();
      if (res.ok) {
        current_url = url;
        return URL.createObjectURL(blob);
      } else {
        throw new Error('No Images Found !');
      }
    }
  }
  function handleClick() {
    filename = getLogo();
  }

  function handleEnter(event) {
    if(event.key == 'Enter'){
      filename = getLogo();
    }
  }
</script>
<svelte:head>
  <title>Anylogo</title>
</svelte:head>
<h1>Anylogo</h1>
<a href="https://clearbit.com">Logos provided by Clearbit</a>
<p>needed to find logo of a website ? use Anylogo type in the url, download the logo !</p>
<input type="text" bind:this={input} bind:value={url} on:keyup={handleEnter}>
<button on:click={handleClick} class="button">Get Logo</button>
{#await filename}
  <p>Loading...</p>
{:then value} 
  {#if value}
    <table>
      <tr>
        <td rowspan="2" width="128px" height="128px"><img src="{value}" alt=""></td>
        <td>{current_url}.png</td>
      </tr>
      <tr><td><a href="{value}" download="{current_url}.png" class="button">Download</a></td></tr>
    </table>
  {/if}
{/await}
<style>
  tr, td {  
    vertical-align: middle;
    box-sizing: border-box;
    padding: 4px;
  }

  tr{
    height: 64px;
  }
  h1 {
    font-size: 48px;
    font-weight: bolder;
    margin-top: 10%;
  }

  p {
    font-size: 24px;
    margin: 10px 0 20px 0;
  }
  h1, p{
    text-align: center;
  }

  input, button {
    margin: 10px auto;
    display: block;
    max-width: 90%;
    width: 400px;
    height: 40px;
    border-radius: 10px;
    border: None;
    box-sizing: border-box;
  }
  input, input:focus, input:active {
    border: 2px solid dodgerblue;
    outline: none;
    color: dodgerblue;
    font-size: 24px;
  }
  img {
    border: 4px dashed dodgerblue;
    border-radius: 10px;
  }
  table {
    margin: 10px auto;
    max-width: 90%;
    width: 400px;
    height: 128px;
    box-sizing: border-box;
  }
  .button {
    display: block;
    text-decoration: none;
    font-weight: bold;
    font-size: 24px;
    text-align: center;
    color: white;
    background-color: dodgerblue;
    border-radius: 10px;
  }
  table a {
    height: 100%;
    width: 100%;
    line-height: 64px;
  }
</style>