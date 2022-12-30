<script>
	import { onMount } from 'svelte';
	import { fade, fly } from 'svelte/transition';
	import TipTap from "./TipTap.svelte"
	
	export let data;
	
	let editing = false;
	let curIndex = false;
	
	onMount(async () => {
		document.querySelectorAll('.edit').forEach((el)=>{
		  
		  el.addEventListener('click', (e)=>{
			  
			  document.querySelectorAll('.editing').forEach((myel)=>{
				  myel.classList.remove('editing');
			  })
			  e.originalTarget.closest('.edit').classList.add('editing');
			  
			  if(editing == true){
				  editing = false;
			
			  	setTimeout(()=>{
				
					let id = e.originalTarget.closest('.edit').getAttribute('id');
					let index = data.items.findIndex(x => x.id == id);
					
					curIndex = index;
					editing = true;
				
				}, 400)
			}else{
				let id = e.originalTarget.closest('.edit').getAttribute('id');
				let index = data.items.findIndex(x => x.id == id);
				
				curIndex = index;
				editing = true;
			}
		  
		  })
		  
		})
		
	});
	  
	
</script> 
 
 
 {#if editing}
<div class="content-editor" in:fly="{{ x: 350, duration: 700 }}" out:fly="{{ x: 350, duration: 150 }}">
  
 <button type="button" class="btn-close float-end mb-3" aria-label="Close" on:click={() => editing = false}></button>
 
  <input type="text" class="form-control" bind:value={data.items[curIndex].title}>

  
  <TipTap bind:html={data.items[curIndex].body} />

  
  
</div>  
{/if}


<style>
	.content-editor {
	  width: 450px;
	  position: fixed;
	  top: 0;
	  right: 0;
	  height: 100%;
	  padding: 20px;
	  background-color: white;
	}
	
	.form-control{
		margin-bottom: 15px;
	}
</style>