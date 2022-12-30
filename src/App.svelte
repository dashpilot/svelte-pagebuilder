<script>
  import Editor from "./editor/Editor.svelte"
 
  let data = {
    "items": [{
      "id": "item-1",
      "layout": "2-col",
      "title": "Hello World",
      "body_rte": "<p>Lorem ipsum dolor site amet</p>",
      "subtitle_txt": "",
      "image_img": ""
    },
  {
    "id": "item-2",
    "layout": "2-col",
    "title": "Second Item",
    "body_rte": "<p>Lorem ipsum dolor site lila</p>",
    "image_img": ""
  }]
  }

</script>


<div class="container mt-5">

{#each data.items as item}
  <section id="{item.id}" class="edit">
    <h1>{item.title}</h1>
    <p>{@html item.body_rte}</p>
    
    {#if item.subtitle_txt}
    {item.subtitle_txt}
    {/if}
    
    {#if item.image_img}
    <img src={item.image_img} class="img-fluid" />
    {/if}
  </section>
{/each}

</div>


<Editor bind:data />

