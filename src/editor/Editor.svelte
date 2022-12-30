<script>
	import { onMount } from 'svelte';
	import { fade, fly } from 'svelte/transition';
	

	import TipTap from "./widgets/TipTap.svelte"
	import Image from "./widgets/Image.svelte"
	
	export let data;
	export let layouts;
	export let user;
	
	export let showAccount;
	
	let editing = false;
	let adding = false;
	
	let curIndex = false;
	let curLayout = false;
	
	let showPublish = false;
	
	onMount(async () => {
		
	
		makeEditable()
	
		
	});
	
	function makeEditable(){
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
				
				let mycomponent = data.items[curIndex].component;
				
				curLayout = layouts.filter(x => x.component == mycomponent)[0]
				
				console.log(curLayout)
				
				adding = false;
		
		  })
		  
		})
	}
	
	
	function showAdd(){
		adding = true;
		editing = false;
	}
	
	function add(layout){
		
		
		let newItem = {};
		
		newItem.id = "item-"+Date.now();
		newItem.component = layout.component;
		Object.keys(layout.fields).forEach((key)=>{
			
			if(layout.fields[key]=='txt'){
				newItem[key] = "Lorem Ipsum"
			}
			if(layout.fields[key]=='rte'){
				newItem[key] = "Lorem ipsum dolor site amet"
			}
			if(layout.fields[key]=='img'){
				newItem[key] = "img/placeholder.jpg"
			}
			
		})
		
		console.log(newItem)
		
		data.items.unshift(newItem);
		
		data = data;
		
		adding = false;
		
		setTimeout(()=>{
			makeEditable()
		}, 50)
		
		
	}  
	
	
	function doPublish(){
		alert('Not available yet')
	}
	
</script> 



	

 
{#if editing}

<div class="content-editor" in:fly="{{ x: 350, duration: 500 }}" out:fly="{{ x: 350, duration: 150 }}">
	
	<h5 class="float-start">Edit</h5>
  
 <button type="button" class="btn-close float-end mb-3" aria-label="Close" on:click={() => editing = false}></button>

<div class="clear"></div>

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


{#if adding}

<div class="content-editor editor-start" in:fly="{{ x: -350, duration: 500 }}" out:fly="{{ x: -350, duration: 150 }}">
  
  <h5 class="float-start">Add Content</h5>
  
 <button type="button" class="btn-close float-end mb-3" aria-label="Close" on:click={() => adding = false}></button>

<div class="clear"></div>

{#each layouts as layout}

<div class="box" on:click={() => add(layout)}>
	
	{layout.name}
	
</div>

{/each}
  
</div>  
{/if}



{#if showPublish}
<div class="backdrop">
	
	<div class="modal" style="display:block;">
	  <div class="modal-dialog">
		<div class="modal-content">
		  <div class="modal-header pt-4">
			<h5 class="modal-title">Publish your page</h5>
			<button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" on:click="{() => showPublish = false}"></button>
		  </div>
		  <div class="modal-body">
			  
			  Publish your page
			 
		  </div>
		  <div class="modal-footer">
			  
			  <button class="btn btn-dark" on:click={doPublish}><i class="fas fa-sign-out-alt"></i> &nbsp;Publish</button>
			  
		  </div>
		</div>
		
	  </div>
	</div>
</div>
{/if}



<div class="dock">
	
<img src="img/account.png" class="grow" on:click={() => showAccount = true} />

<img src="img/plus.png" class="grow" on:click={showAdd} />

<img src="img/publish.png" class="grow" on:click={() => showPublish = true} />

</div>





<style>
	.content-editor {
	  max-width: 100%;
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
	  z-index: 999;
	  
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
	
	.editor-start{
		right: auto;
		left: 0;
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
	
	.clear{
		clear: both;
	}
	
	.box{
		border: 1px solid #CCC;
		padding: 10px;
		margin-bottom: 10px;
	}
	
	h5{
		color: black;
		padding-top: 1px;
	}
	
	.dock{
		text-align: center;
		position: fixed;
		bottom: 20px;
		left: 0;
		width: 100%;
		
	}
	
	.dock img{
		margin: 0px 5px;
		width: 65px;
	}

	
	.grow { transition: all .2s ease-in-out; }
	.grow:hover { transform: scale(1.1); cursor: pointer; }
</style>