 <script>
 export let item;
 export let components;
if(!item){
 components.push({
			 "component": "Features",
			 "name": "Features",
			 "fields": {
			   "title":"txt",
			   "body": "txta",
			   "title_2":"txt",
			   "body_2": "txta"   
		   }
	   })
 }
 </script>
 
{#if item}
<section>
  <div class="container edit" id="{item.id}">
	<div class="row">
	<div class="col-md-6 text-center">
	  <h3>{item.title}</h3>
	  {item.body}
	</div>
	<div class="col-md-6 text-center">
	  <h3>{item.title_2}</h3>
	  {item.body_2}
	</div>
	</div>
  </div>
</section>
{/if}