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
  import Features from "./Features.svelte"

  let components = [];
  let item = false;

onMount(async () => {
	console.log(components)
});

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
	{#if item.component == 'Features'}
	<Features bind:item bind:components />
	{/if}
	
  
  {/each}
  
<!-- We need to include them at least once -->
<Header bind:item bind:components />	
<Post bind:item bind:components /> 
<News bind:item bind:components />
<Features bind:item bind:components />
	
<Editor bind:data bind:components />
	  
