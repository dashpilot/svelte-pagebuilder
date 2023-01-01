<script>
  import { onMount } from 'svelte';
  import Editor from "./editor/Editor.svelte"
  
  import User from "./editor/User.svelte"
  import { auth, db } from './editor/firebase';
  let user = false; 
  let showAccount = false;
  

  
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
  },
  {
    "id": "item-3",
    "component": "TwoCol",
    "title": "Third Item",
    "subtitle": "",
    "body": "<p>Lorem ipsum dolor site lila</p>",
    "image": ""
  },
  {
    "id": "item-4",
    "component": "TwoCol",
    "title": "Fourth Item",
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

{#each data.items as item}
<div class="landing-card">
  {#if item.component == 'Intro'}
  <Intro bind:item={item} />
  {/if}
  
  {#if item.component == 'TwoCol'}
  <TwoCol bind:item={item} />
  {/if}
</div>
{/each}

</div>


<Editor bind:data bind:layouts bind:user bind:showAccount />
  
{/if}


<style>
  
 
  
  .wrapper {
    margin: 0 auto;
    margin-top: 50px;
    max-width: 768px;
    margin-bottom: 300px;
    padding: 15px;
  }
  
  .landing-card {
    position: sticky;
    top: 30px;
 color: white !important;
    margin-bottom: 25px;
 
    border-radius: 10px;
    box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.1);
    color: #333;
    padding: 40px;
    height: 400px;
    background-color: #091a2f;
    background-image: linear-gradient(311deg,rgba(26,188,254,.23),rgba(9,26,47,.1));
  }
</style>

