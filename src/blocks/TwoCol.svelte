<script>
	import { onMount } from 'svelte';
	export let item;
	export let index;
	
	
	
	let margin = index*20;
	
	onMount(()=>{
		if(item.title == 'Lorem ipsum'){
			item.title = "We help you grow.";
		}
		
	})
</script>

<section id="{item.id}" class="edit landing-card" style="margin-top: 10px">

<div class="row h-100">
	<div class="col-md-6 d-flex align-items-center">
		
		<div>
		<h2>{item.title}</h2>
		
		<p>{@html item.body}</p>
		</div>
		
	</div>
	<div class="col-md-6"></div>
	
	
</div>

	
	{#if item.image}
	<img src={item.image} class="img-fluid" />
	{/if}

</section>

<style>
	.landing-card{
		background-image: url(graphics/stats.png);
		background-size: cover;
		background-repeat: no-repeat;
	}
	
</style>