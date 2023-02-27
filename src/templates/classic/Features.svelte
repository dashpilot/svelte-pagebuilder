 <script>
 export let item;
 export let components;
 components.push({
			 "component": "Features",
			 "name": "Features",
			 "fields": {
			   "title":"txt",
			   "body": "rte",
			   "title_2":"txt",
			   "body_2": "rte"   
		   }
	   })
 </script>
 
{#if item}
<section>
  <div class="container edit" id="{item.id}">
	<div class="row">
	<div class="col-md-6 text-center">
	  <h3>{item.title}</h3>
	  {@html item.body}
	</div>
	<div class="col-md-6 text-center">
	  <h3>{item.title_2}</h3>
	  {@html item.body_2}
	</div>
	</div>
  </div>
</section>
{/if}