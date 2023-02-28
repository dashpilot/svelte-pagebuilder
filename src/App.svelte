<script>
  import { onMount } from 'svelte';

  
  /*
  import User from "./editor/User.svelte"
  import { auth, db } from './editor/firebase';
  */
  let user = false; 
  let showAccount = false;
  
  import CardsTemplate from "./templates/cards/Index.svelte"
  import ClassicTemplate from "./templates/classic/Index.svelte"
  
  let index;
  let data;
  

  onMount(async function() {
    const response = await fetch(cfg.dataPath);
    data = await response.json();
  })
  
  // this function receives updates from the cms
  window.relay = function(mydata) {
    // console.log(data);
    data = mydata;
  }
</script>

{#if data}
{#if data.config.template=='cards'}
<CardsTemplate bind:data />
  {:else if data.config.template=='classic'}
  <ClassicTemplate bind:data />

{:else}
Template not found
{/if}

{/if}


<!--
<User bind:user bind:showAccount />
-->




