<script>
  import { onMount } from 'svelte';
  import Editor from "./editor/Editor.svelte"
  
  import User from "./editor/User.svelte"
  import { auth, db } from './editor/firebase';
  let user = false; 
  let showAccount = false;
  
  import CardsTemplate from "./templates/cards/Index.svelte"
  import ClassicTemplate from "./templates/classic/Index.svelte"
  
  let index;
  

  let data = {
    "config": {
      "template": "classic",
    },
    "items": [{
      "id": "item-1",
      "component": "Header",
      "title": "Tijpo",
      "subtitle": "That's not a typo.",
      "button_text": "Contact Us",
      "body": "<p>Lorem ipsum dolor site amet</p>"
    },
  {
    "id": "item-2",
    "component": "Post",
    "title": "Lorem Ipsum",
    "body": "<p>Lorem ipsum dolor site amet.</p>"
  }]
  }
  
  
   
  onMount(async () => {
   
      
    
  })

</script>

{#if data.config.template=='cards'}
<CardsTemplate bind:data />
  {:else if data.config.template=='classic'}
  <ClassicTemplate bind:data />

{:else}
Template not found
{/if}


<!--
<User bind:user bind:showAccount />
-->




