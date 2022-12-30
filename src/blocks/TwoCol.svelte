<script>
	export let item;
</script>

<section id="{item.id}" class="edit">

	<h1>{item.title}</h1>
	
	<p>{@html item.body}</p>
	
	{#if item.image_img}
	<img src={item.image} class="img-fluid" />
	{/if}

</section>