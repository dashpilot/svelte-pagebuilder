<script>
export let item;
export let components;
if(!item){
components.push({
			"component": "Post",
			"name": "Post",
			"fields": {
			  "title":"txt",
			  "body": "rte"
		  }
	  })
}
</script> 
 
 {#if item}
 <section>
  <div class="container edit" id="{item.id}">
  <div class="row">
  <div class="col-md-2"></div>
	<div class="col-md-8">
	<h2>{item.title}</h2>
	{@html item.body}
	</div>
	<div class="col-md-2"></div>
  </div>
  </div>
  </section>
  {/if}