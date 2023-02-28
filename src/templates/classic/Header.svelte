<script>
export let item;
export let components;
if(!item){
components.push({
			"component": "Header",
			"name": "Header",
			"fields": {
			  "title":"txt",
			  "subtitle": "txt",
			  "icon": "icn",
			  "button_text": "txt",
			  "body": "rte"
		  }
	  })
}
</script>

{#if item}
<section>
	<div class="container edit text-center" id="{item.id}">
  	<i class="{item.icon} fa-big"></i>
  	<h1>{item.title}</h1>
  	<p>{item.subtitle}</p>
  	<a href="{item.button_link}" class="btn btn-outline-dark">{item.button_text}</a>
	</div>
</section>
{/if}