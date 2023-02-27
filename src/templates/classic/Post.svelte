<script>
export let item;
export let components;
components.push({
			"component": "Post",
			"name": "Post",
			"fields": {
			  "title":"txt",
			  "body": "rte"
		  }
	  })
</script> 
 
 <section id="{item.id}" class="edit" data-fields="title=txt&amp;body=rte">
  <div class="container">
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