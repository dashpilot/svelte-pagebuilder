<script>
	import {onMount} from "svelte";
	export let item;
	export let key;
	
	let showPicker = false;
	let icons;
	
	
	onMount(async function() {
		const response = await fetch('icons.json');
		icons = await response.json();
	})
	
	function selectIcon(icon){
		item[key] = icon;
		showPicker = false;
	}
	
</script>

<button class="button mb-15" on:click={()=>showPicker=true}><i class="{item[key]}"></i> &nbsp;Select Icon</button>

{#if showPicker}
<div class="backdrop">
  <div class="modal-screen">
	<div class="editor-header">
	  <h3 class="fl-left">Icon Picker</h3>
	  <span class="close fl-right" aria-label="Close" on:click={()=>showPicker=false}>&times;</span>
	 
	</div>
	<div class="clear mb-3"></div>
	<div class="iconpicker-icons">
	{#if icons}
	  {#each Object.keys(icons) as key}
		<i class="{icons[key]} m-2 icon" on:click={()=>selectIcon(icons[key])}></i>
	  {/each}
	  {/if}
	</div>
  </div>
</div>
{/if}

<style>
	.icon:hover{
		color: black;
		cursor:pointer;
	}
	
	.modal-screen {
	  max-width: 800px;
	  height: 500px;
	  margin: 0 auto;
	  margin-top: 100px;
	  background-color: white;
	  border-radius: 12px;
	  padding: 15px;
	  overflow-y: auto;
	}
	
	.modal-screen h4 {
	  color: black;
	}
</style>