<script>
	import ColorPicker from 'svelte-awesome-color-picker';// or hsv or hex

	export let data;
	let font = data.design.font;
	let hex = data.design.color1; 

	
	function setColor(e) {
	
	  let myhex = e.detail.hex;
		
	
	  let myhex2 = colorContrast(myhex);
	  
	  let root = document.documentElement;
	  root.style.setProperty('--bg-color', myhex);
	  root.style.setProperty('--fg-color', myhex2);
	  
	  data.design.color1 = myhex;
	  data.design.color2 = myhex2;
	  
	 /*
	  localStorage.setItem('bg-color', hex);
	  localStorage.setItem('fg-color', hex2);
	  */
	
	}
	
	function setFont(myfont) {
	  font = myfont;
	  data.design.font = myfont;
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


{data.design.font}

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

<div class="label mt-3">Color</div>
<div class="picker">
<ColorPicker bind:hex label="&nbsp;" on:input={setColor} />
</div>


<style>
	.list-group-item{
		cursor: pointer;
		color: black;
	}
</style>
