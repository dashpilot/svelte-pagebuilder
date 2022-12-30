<script>
	import { onMount } from 'svelte';
	
	export let item;
	export let layouts;
	
	// register the fields
	onMount(async () => {
		layouts.push({
			"layout": "TwoCol",
			"fields": {
				"title":"txt",
				"body": "rte",
				"image": "img"
			}
		})
	})
</script>

<section id="{item.id}" class="edit">

	<h1>{item.title}</h1>
	
	
	
	<p>{@html item.body}</p>
	
	{#if item.image_img}
	<img src={item.image} class="img-fluid" />
	{/if}

</section>