<script>
  import { onMount } from 'svelte';
  import Editor from "./editor/Editor.svelte"
  
  import Intro from "./blocks/Intro.svelte"
  import TwoCol from "./blocks/TwoCol.svelte"
  
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
  }];
  
  let data = {
    "items": [{
      "id": "item-1",
      "component": "Intro",
      "title": "Hello World",
      "body": "<p>Lorem ipsum dolor site amet</p>"
    },
  {
    "id": "item-2",
    "component": "TwoCol",
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
  {#if item.component == 'Intro'}
  <Intro bind:item={item} />
  {/if}
  
  {#if item.component == 'TwoCol'}
  <TwoCol bind:item={item} />
  {/if}
{/each}

</div>


<Editor bind:data bind:layouts />

