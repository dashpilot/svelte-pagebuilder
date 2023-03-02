<script>
import { onMount } from 'svelte';



let font = 'Lobster';
let fontList;

function selectFont(){
	console.log(font)
	
	WebFont.load({
		google: {
			families: [font+':300,400,700']
		},
		loading: function() {
			console.log("Fonts are being loaded");
		},
		active: function() {
			console.log("Fonts have been rendered")
			let root = document.documentElement;
			root.style.setProperty('--font', font);
			// root.style.setProperty('--spacing', spacing);
			
		}
	});
}

onMount(async () => {
	const response = await fetch('google-fonts.json');
	fontList = await response.json();
});

</script>

{#if fontList}
<div class="select is-fullwidth mb-10">
<select bind:value={font} on:change="{selectFont}">
	{#each fontList as item}
	<option value="{item.family}">{item.family}</option>
{/each}
</select>
</div>
{/if}