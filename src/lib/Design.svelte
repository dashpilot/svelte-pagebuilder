<script>
	import ColorPicker from 'svelte-awesome-color-picker';// or hsv or hex

	export let data;
	let font = 'Rubik';
	let hex = '#f7fafc'; 
	let color;
	
	function setColor(myhex) {
	  color = myhex;
	  let hex2 = colorContrast(hex);
	  let root = document.documentElement;
	  root.style.setProperty('--bg-color', hex);
	  root.style.setProperty('--fg-color', hex2);
	  
	  /*
	  localStorage.setItem('bg-color', hex);
	  localStorage.setItem('fg-color', hex2);
	  */
	}
	
	function setFont(myfont) {
	  font = myfont;
	  let spacing = '0';
	  if (font == 'Rubik') {
		spacing = '-0.04em';
	  }
	  let root = document.documentElement;
	  root.style.setProperty('--font', font);
	  root.style.setProperty('--spacing', spacing);
	  
	  /*
	  localStorage.setItem('font', font);
	  localStorage.setItem('spacing', spacing);
	  */
	}
</script>


<div class="label">Color</div>
<div class="picker">
<ColorPicker bind:hex label="&nbsp;" />
</div>


<div class="label">Font</div>
<div class="list-group">
<a class="list-group-item" on:click={()=>setFont('Rubik')}>{#if font=='Rubik'}<i class="fas fa-check mr-2"></i>&nbsp;{/if} Default</a>
<a class="list-group-item" on:click={()=>setFont('Lobster')}>{#if font=='Lobster'}<i class="fas fa-check mr-2"></i>&nbsp;{/if} Lobster</a>
<a class="list-group-item"  on:click={()=>setFont('Playfair Display')}>{#if font=='Playfair Display'}<i class="fas fa-check mr-2"></i>&nbsp;{/if} Playfair Display</a>
<a class="list-group-item" on:click={()=>setFont('Raleway')}>{#if font=='Raleway'}<i class="fas fa-check mr-2"></i>&nbsp;{/if} Raleway</a>
<a class="list-group-item" on:click={()=>setFont('Cutive Mono')}>{#if font=='Cutive Mono'}<i class="fas fa-check mr-2"></i>&nbsp;{/if} Cutive Mono</a>
<a class="list-group-item" on:click={()=>setFont('Helvetica')}>{#if font=='Helvetica'}<i class="fas fa-check mr-2"></i>&nbsp;{/if} Helvetica</a>
<a class="list-group-item" on:click={()=>setFont('Georgia')}>{#if font=='Georgia'}<i class="fas fa-check mr-2"></i>&nbsp;{/if} Georgia</a>
</div>

<style>
	.list-group-item{
		cursor: pointer;
		color: black;
	}
</style>
