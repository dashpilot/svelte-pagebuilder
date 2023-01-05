<script>
	import { onMount } from 'svelte';
	export let item;
	

	onMount(()=>{
		if(item.title == 'Lorem ipsum'){
			item.title = "We help you grow.";
		}
		
	})
</script>

<section id="{item.id}" class="edit landing-card">
<div class="card-body">
<div class="row h-100">
	<div class="col-md-6 d-flex align-items-center">
		
		<div class="card-text">
		<h2>{item.title}</h2>
		
		<p>{@html item.body}</p>
		</div>
		
	</div>
	<div class="col-md-6"></div>
	
	
</div>

	
	{#if item.image}
	<img src={item.image} class="img-fluid" />
	{/if}
</div>
</section>

<style>
	.card-body{
		width: 100%;
		height: 100%;
		background-image: url(/graphics/stats.png);
		background-size: cover;
		background-repeat: no-repeat;
	}
	
	.card-text{
		padding-left: 40px;
	}
</style>