<script>
 
  import Editor from "./editor/Editor.svelte"
 
  let data = {
    "items": [{
      "id": "item-1",
      "layout": "2-col",
      "title": "Hello World",
      "body": "<p>Lorem ipsum dolor site amet</p>"
    },
  {
    "id": "item-2",
    "layout": "2-col",
    "title": "Second Item",
    "body": "<p>Lorem ipsum dolor site lila</p>"
  }]
  }

</script>


<div class="container mt-5">

{#each data.items as item}
  <section id="{item.id}" class="edit">
    <h1>{item.title}</h1>
    <p>{@html item.body}</p>
  </section>
{/each}

</div>


<Editor bind:data />

