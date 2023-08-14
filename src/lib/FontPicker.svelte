<script>
import { onMount, onDestroy } from 'svelte';
export let data;
export let base;

let font = data.design.font;
let fontList;

function selectFont(){
	
	WebFont.load({
		google: {
			families: [font+':400,600,700']
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
	const response = await fetch(base+'google-fonts-selection.json');
	fontList = await response.json();
});

onDestroy(async () => {
	// update the font on close
	data.design.font = font;
});

</script>

{#if fontList}


<select class="form-control" bind:value={font} on:change="{selectFont}">
	{#each fontList as item}
	<option value="{item.family}">{item.family}</option>
{/each}
</select>

{/if}