<script>
import {dndzone} from "svelte-dnd-action";
import {flip} from "svelte/animate";
import {slugify} from "./Helpers.svelte"

export let data;
let items = data.categories;
let title;

const flipDurationMs = 300;
  
  function handleDndConsider(e) {
	items = e.detail.items;
  }
  function handleDndFinalize(e) {
	items = e.detail.items;
	data.categories = items;
	console.log(data)
  }
  
  function deleteCat(id){
	  console.log(id)
	  var sure = confirm("Are you sure you wish to delete this item?")
	  if(sure){
		  let index = data.categories.findIndex(x=>x.id==id);
		  data.categories.splice(index, 1);
		  data = data;
		  items = data.categories;
	  }
  }
  
  function addCat(){
	  var id = newId();
	  var slug = slugify(title, false)
	  var exists = data.categories.filter(x=>x.slug==slug)[0]
	  if(exists){
		  slug = slugify(title, id)
		  console.log(slug)
	  }
	  
	  let newCat = {}
	  newCat.id = id;
	  newCat.title = title;
	  newCat.slug = slug;
	  
	  data.categories.push(newCat);
	  data = data;
	  items = data.categories;
  }
  
  function newId(){
		let highest_id = Math.max(...data.categories.map(x => x.id));
		return highest_id+1;
	}
</script>

<div class="label mt-3">Add Category</div>
<div class="input-group mb-1">
  <input type="text" class="form-control" placeholder="Category Name" aria-describedby="basic-addon2"  bind:value={title}>
  <div class="input-group-append">
	<button class="btn btn-outline-secondary" type="button" on:click={addCat}>Add</button>
  </div>
</div>



<div class="label">Drag to reorder</div>	

  <ul class="list-group entries-list" use:dndzone="{{items, flipDurationMs}}" on:consider="{handleDndConsider}" on:finalize="{handleDndFinalize}">
	{#each items as item(item.id)}
	<li class="list-group-item item-list" animate:flip="{{duration: flipDurationMs}}">
		
	<div class="row">
		<div class="col-9 text-truncate">
			
			{#if item.title==''}Untitled{:else}{item.title}{/if}
			
		</div>
		<div class="col-3 text-end">
			{#if item.slug!=='home'}
			<button class="btn btn-outline-secondary btn-delete" on:click={()=>deleteCat(item.id)}><i class="fas fa-trash"></i></button>
			{:else}
			<button class="btn btn-outline-secondary btn-opacity"><i class="fas fa-trash"></i></button>
			{/if}
			
		</div>
	</div>	
		</li>
	{/each}
  </ul>
  
  
  <style>
		.item-list{
		  border: 1px solid #D2D2D2;
		  border-bottom: 0;
		  padding: 15px;
		}
		
		.item-list:last-child{
		  border-bottom: 1px solid #D2D2D2;
		}
		
		.item-list a{
		  color: black;
		}
		
		.item-list a:hover{
		  text-decoration: underline;
		}
		
		.col-9{
			padding-top: 6px;
			padding-left: 10px;
		}
		
		.btn-opacity{
			opacity: 0;
		}
		
		.list-group-item{
			padding: 8px 15px 8px 18px;
		}
  </style>