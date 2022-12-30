<script>
	import { onMount } from 'svelte';
	
	export let item;
	export let layouts;
	
	// register the fields
	onMount(async () => {
		layouts.push({
			"layout": "Intro",
			"fields": {
				"title":"txt",
				"body": "rte"
			}
		})
	})
		
	
</script>

<section id="{item.id}" class="edit">
	
	<div class="w-50 m-auto text-center">
		
		<h2>{item.title}</h2>
		
		{@html item.body}
		
	</div>
	
</section>