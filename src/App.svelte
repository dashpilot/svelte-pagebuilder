
<svelte:head>
<link href="templates/classic/style.css" rel="stylesheet" />
</svelte:head>
  
<script>
  import { onMount } from 'svelte';

    
  import Header from "./components/Header.svelte"
  import Post from "./components/Post.svelte"
  import News from "./components/News.svelte"
  import Features from "./components/Features.svelte"
  
  
  import Editor from "./editor/Editor.svelte"
  
  let data;
  let components = [];
  let item = false;
  let index;
  let user = false;

  onMount(async function() {
    const response = await fetch(cfg.dataPath);
    data = await response.json();
    
    console.log(components)
    window.components = components;
  })
  
  // this function receives updates from the cms
  window.relay = function(mydata) {
    // console.log(data);
    data = mydata;
  }
</script>

{#if data}
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
{/if}  

  
<!-- We need to include them at least once -->
<Header bind:item bind:components />	
<Post bind:item bind:components /> 
<News bind:item bind:components />
<Features bind:item bind:components />
  
<Editor />
    



