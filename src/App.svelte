<script>
  import { onMount } from 'svelte';
  import Editor from "./editor/Editor.svelte"
  
  import User from "./editor/User.svelte"
  import { auth, db } from './editor/firebase';
  let user = false; 
  let showAccount = false;
  
let index;
  
  import Intro from "./blocks/Intro.svelte"
  import TwoCol from "./blocks/TwoCol.svelte"
  import Waves from "./blocks/Waves.svelte"
  
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
  
  let data = {
    "items": [{
      "id": "item-1",
      "component": "Intro",
      "title": "SliderStack",
      "body": "<p>Lorem ipsum dolor site amet</p>"
    },
  {
    "id": "item-2",
    "component": "TwoCol",
    "title": "Lorem ipsum dolor",
    "subtitle": "",
    "body": "<p>Lorem ipsum dolor site lila</p>",
    "image": ""
  },
  {
    "id": "item-3",
    "component": "Waves",
    "title": "Lorem ipsum dolor",
    "subtitle": "",
    "body": "<p>Lorem ipsum dolor site lila</p>",
    "image": ""
  },
  {
    "id": "item-4",
    "component": "Intro",
    "title": "Lorem ipsum dolor",
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

<User bind:user bind:showAccount />

{#if user}
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


<Editor bind:data bind:layouts bind:user bind:showAccount />
  
{/if}


<style>
  
 
  
  
</style>

