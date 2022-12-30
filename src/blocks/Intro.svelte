<script>

	export let item;

</script>

<section id="{item.id}" class="edit">
	
	<div class="w-50 m-auto text-center">
		
		<h2>{item.title}</h2>
		
		{@html item.body}
		
	</div>
	
</section>