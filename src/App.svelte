<script>
  import { onMount } from 'svelte';
  import Editor from "./editor/Editor.svelte"
  
  import Intro from "./blocks/Intro.svelte"
  import TwoCol from "./blocks/TwoCol.svelte"
  
  let layouts = [{
      "layout": "Intro",
      "fields": {
        "title":"txt",
        "body": "rte"
      }
    },{
    "layout": "TwoCol",
    "fields": {
      "title":"txt",
      "body": "rte",
      "image": "img"
    }
  }];
  
  let data = {
    "items": [{
      "id": "item-1",
      "layout": "Intro",
      "title": "Hello World",
      "body": "<p>Lorem ipsum dolor site amet</p>"
    },
  {
    "id": "item-2",
    "layout": "TwoCol",
    "title": "Second Item",
    "subtitle": "",
    "body": "<p>Lorem ipsum dolor site lila</p>",
    "image": ""
  }]
  }
  
  
   
  onMount(async () => {
   
      // 'layouts' contains the names and fields of the registered components
      console.log(layouts)
    
  })

</script>


<div class="container mt-5">

{#each data.items as item}
  {#if item.layout == 'Intro'}
  <Intro bind:item={item} />
  {/if}
  
  {#if item.layout == 'TwoCol'}
  <TwoCol bind:item={item} />
  {/if}
{/each}

</div>


<Editor bind:data bind:layouts />

