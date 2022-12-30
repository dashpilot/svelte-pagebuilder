<script>
	import { onMount } from 'svelte';
	import { fade, fly } from 'svelte/transition';
	import TipTap from "./widgets/TipTap.svelte"
	import Image from "./widgets/Image.svelte"
	
	export let data;
	export let layouts;
	
	let editing = false;
	let curIndex = false;
	let curLayout = false;
	
	onMount(async () => {
		
		console.log(layouts)
		
		document.querySelectorAll('.edit').forEach((el)=>{
		  
		  el.addEventListener('click', (e)=>{
			  
			  document.querySelectorAll('.editing').forEach((myel)=>{
				  myel.classList.remove('editing');
			  })
			  e.originalTarget.closest('.edit').classList.add('editing');
			  
			  
				let id = e.originalTarget.closest('.edit').getAttribute('id');
				let index = data.items.findIndex(x => x.id == id);
				
				curIndex = index;
				editing = true;
				
				let mylayout = data.items[curIndex].layout;
				
				curLayout = layouts.filter(x => x.layout == mylayout)[0]
				
				console.log(curLayout)
				
				
		  
		  })
		  
		})
		
	});
	  
	
</script> 
 
 
{#if editing}

<div class="content-editor" in:fly="{{ x: 350, duration: 500 }}" out:fly="{{ x: 350, duration: 150 }}">
  
 <button type="button" class="btn-close float-end mb-3" aria-label="Close" on:click={() => editing = false}></button>


  {#each Object.keys(curLayout.fields) as mykey}


  {#if curLayout.fields[mykey] == 'txt'}
  <div class="label">{mykey}</div>
		<input type="text" class="form-control" bind:value={data.items[curIndex][mykey]} />
	{/if}
  
  
  {#if curLayout.fields[mykey] == 'rte'}
  	{#key data.items[curIndex].id}
	  
	  <div class="label">{mykey}</div>
	  <TipTap bind:item={data.items[curIndex]} bind:key={mykey} />
    {/key}
  {/if}

  
  {#if curLayout.fields[mykey] == 'img'}
  	<div class="label">{mykey}</div>
  	<Image bind:item={data.items[curIndex]} bind:key={mykey} />
  {/if}
  

  
  {/each}
	
  <button class="btn btn-dark" on:click={() => editing = false}>Save</button>
  
  
</div>  
{/if}


<style>
	.content-editor {
	  width: 400px;
	  position: fixed;
	  top: 0;
	  right: 0;
	  height: 100%;
	  padding: 20px;
	  padding-top: 15px;
	  background-color: white;
	  border-left: 1px solid #ced4da;
	  color: #777;
	  
	  font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
		font-size: 16px;
		line-height: 24px;
		font-weight: 400;
		font-synthesis: none;
		text-rendering: optimizeLegibility;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		-webkit-text-size-adjust: 100%;
	}
	
	.label{
		text-transform: uppercase;
		letter-spacing: 0.03em;
		font-size: 14px;
		margin-bottom: 3px;
	}
	
	.form-control{
		margin-bottom: 15px;
	}
</style>