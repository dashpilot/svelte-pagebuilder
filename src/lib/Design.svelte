<script>
	import ColorPicker from 'svelte-awesome-color-picker';
	import SortableCategories from './SortableCategories.svelte';
	import SortablePosts from './SortablePosts.svelte';
	import FontPicker from "./FontPicker.svelte"
	
	
	export let data;
	export let base;
	
	let font = data.design.font;
	let hex = data.design.color1; 
	
	let curTab = 'design'
	
	let cat;
	if(typeof window.category !== 'undefined'){
		cat = data.categories.filter(x=>x.slug==window.category)[0]
	}else{
		cat = data.categories.filter(x=>x.slug=='home')[0]
	}
	
	let items = data.posts.filter(x=>x.category==cat.id)

	
	function setColor(e) {
	
	  let myhex = e.detail.hex;
		
	
	  let myhex2 = colorContrast(myhex);
	  
	  let root = document.documentElement;
	  root.style.setProperty('--bg-color', myhex);
	  root.style.setProperty('--fg-color', myhex2);
	  
	  data.design.color1 = myhex;
	  data.design.color2 = myhex2;

	
	}
	

	function colorContrast(hex) {
	
	  var threshold = 150; /* about half of 256. Lower threshold equals more dark text on dark background  */
	  var hRed = hexToR(hex);
	  var hGreen = hexToG(hex);
	  var hBlue = hexToB(hex);
	
	  function hexToR(h) {
		return parseInt((cutHex(h)).substring(0, 2), 16)
	  }
	
	  function hexToG(h) {
		return parseInt((cutHex(h)).substring(2, 4), 16)
	  }
	
	  function hexToB(h) {
		return parseInt((cutHex(h)).substring(4, 6), 16)
	  }
	
	  function cutHex(h) {
		return (h.charAt(0) == "#") ? h.substring(1, 7) : h
	  }
	
	  var cBrightness = ((hRed * 299) + (hGreen * 587) + (hBlue * 114)) / 1000;
	  if (cBrightness > threshold) {
		return "#000000";
	  } else {
		return "#ffffff";
	  }
	}
	


</script>


  
 <div class="tabs">
   <ul>
	 <li class:is-active={curTab=='design'} on:click={()=>curTab='design'}><a>Design</a></li>
	 <li  class:is-active={curTab=='categories'} on:click={()=>curTab='categories'}><a>Categories</a></li>
	 <li  class:is-active={curTab=='posts'} on:click={()=>curTab='posts'}><a>Posts</a></li>
   </ul>
 </div>
 
 
 {#if curTab=='design'}

<div class="label">Color</div>
<ul class="list-group mb-15">
	<li class="list-group-item">
		<div class="picker">
		<ColorPicker bind:hex label="pick color" on:input={setColor} />
		</div>
	</li>
</ul>

<div class="label">Font</div>

<FontPicker bind:data bind:base />

{:else if curTab=='categories'}

<SortableCategories bind:data />
	
{:else if curTab=='posts'}
	
	<SortablePosts bind:data bind:items bind:cat={cat.id} />
	
{/if}


<style>
	.list-group-item{
		cursor: pointer;
		color: black;
	}

</style>
