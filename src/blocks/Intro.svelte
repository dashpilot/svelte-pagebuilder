<script>
	import { onMount } from 'svelte';
	export let item;
	export let index;
	
	onMount(()=>{
		if(item.title == 'Lorem ipsum'){
			item.title = "We help you grow.";
		}
	})
</script>

<section id="{item.id}" class="edit landing-card d-flex align-items-center text-center">
	

		
		<div class="w-100">
			
			<i class="fas fa-address-card fa-large"></i>
			
		<h2>{item.title}</h2>
		
		{@html item.body}
		</div>
		

	
</section>