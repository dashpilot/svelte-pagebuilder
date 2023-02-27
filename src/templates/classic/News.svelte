 <script>
 export let item;
 export let components;
 if(!item){
 components.push({
			 "component": "News",
			 "name": "News",
			 "fields": {
			   "title":"txt",
			   "body": "rte",
			   "image": "img"
		   }
	   })
}
 </script> 
 
 {#if item}
 <section>
  <div class="container edit" id="{item.id}">
	<div class="row">
	<div class="col-md-6">
	  <h2>{item.title}</h2>
	  {@html item.body}
	</div>
	<div class="col-md-6">
		{#if item.image}
			<img src="{item.image}" class="img-fluid" />
		
		{:else if item.youtube}
		<div class="responsive-container">
		<iframe
		  title="video"
		  class="responsive-iframe"
		  width="560"
		  height="315"
		  :src="'https://www.youtube.com/embed/' + youtube_parser(item.youtube_url)"
		  frameborder="0"
		  allowfullscreen />
		  </div>
	  {:else}
		<img src="https://vue-pagebuilder.vercel.app/editor/img/placeholder.jpg" class="img-fluid" />
	  {/if}
	  </div>
	</div>
  </div>
  </section>
  {/if}