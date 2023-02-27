<svelte:head>
<link href="templates/cards/style.css" rel="stylesheet" />
</svelte:head>
  
  
  <script>
  export let data;  
  let user = false;
  import Editor from "../../editor/Editor.svelte"
	  
  import Intro from "./Intro.svelte"
  import TwoCol from "./TwoCol.svelte"
  import Waves from "./Waves.svelte"
  
  
  let layouts = [{
		"component": "Intro",
		"name": "Intro",
		"fields": {
		  "title":"txt",
		  "body": "rte"
		}
	  },{
	  "component": "TwoCol",
	  "name": "Two Columns",
	  "fields": {
		"title":"txt",
		"body": "rte",
		"image": "img"
	  }
	},{
	  "component": "Waves",
	  "name": "Waves",
	  "fields": {
		"title":"txt",
		"body": "rte",
		"image": "img"
	  }
	}];
  </script>
  
  <div class="wrapper mt-5">
  
  {#each data.items as item, index}
  
  
  
	{#if item.component == 'Intro'}
	<Intro bind:item={item} />
	{/if}
	
	{#if item.component == 'TwoCol'}
	<TwoCol bind:item={item} />
	{/if}
	
	{#if item.component == 'Waves'}
	<Waves bind:item={item} />
	{/if}
  
  
  {/each}
  
  </div>
  
  
  <Editor bind:data bind:layouts />
	  
	  
<style>
	
</style>