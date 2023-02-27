<svelte:head>
<link href="templates/classic/style.css" rel="stylesheet" />
</svelte:head>
  
  
  <script>
  import { onMount } from 'svelte';
  export let data;  
  let user = false;
  import Editor from "../../editor/Editor.svelte"
	  
  import Header from "./Header.svelte"
  import Post from "./Post.svelte"
  import News from "./News.svelte"

  let components = [];

  



onMount(async () => {
	console.log(components)
});
  
  /*
  let layouts = [{
		"component": "Header",
		"name": "Header",
		"fields": {
		  "title":"txt",
		  "subtitle": "txt",
		  "icon": "txt",
		  "button_text": "txt",
		  "body": "rte"
	  }
	}];
	*/
  </script>

  
  {#each data.items as item, i}

  
	{#if item.component == 'Header'}
	<Header bind:item bind:components />		
	{/if}
	{#if item.component == 'Post'}
	<Post bind:item bind:components />
	{/if}
	{#if item.component == 'News'}
	<News bind:item bind:components />
	{/if}
  
  {/each}
  

  
  <Editor bind:data bind:components />
	  
