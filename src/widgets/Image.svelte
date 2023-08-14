<script>
	
	import { onMount } from 'svelte';
	
	export let key;
	export let item;
	// export let settings;
	
	let uploading = false;

	onMount(async () => {
	
	  document.getElementById('fileInput').addEventListener('change', function(e) {
	
		uploading = true;
	
		
		var width = 800;
		  
		  
		  
		var img = new Image();
		img.onload = function() {
		  var canvas = document.createElement('canvas'),
			ctx = canvas.getContext("2d"),
			oc = document.createElement('canvas'),
			octx = oc.getContext('2d');
	
		  canvas.width = width; // destination canvas size
		  canvas.height = canvas.width * img.height / img.width;
	
		  var cur = {
			width: Math.floor(img.width * 0.5),
			height: Math.floor(img.height * 0.5)
		  }
	
		  oc.width = cur.width;
		  oc.height = cur.height;
	
		  octx.drawImage(img, 0, 0, cur.width, cur.height);
	
		  while (cur.width * 0.5 > width) {
			cur = {
			  width: Math.floor(cur.width * 0.5),
			  height: Math.floor(cur.height * 0.5)
			};
			octx.drawImage(oc, 0, 0, cur.width * 2, cur.height * 2, 0, 0, cur.width, cur.height);
		  }
	
		  ctx.drawImage(oc, 0, 0, cur.width, cur.height, 0, 0, canvas.width, canvas.height);
		  var base64Image = canvas.toDataURL('image/jpeg')
	
		  console.log(base64Image);
		  
		  item[key] = base64Image;
		  
		  /*
		  let opts = {};
		  opts.path = 'img/'+Date.now()+".jpg";
		  opts.type = 'img';
		  opts.data = base64Image;
		  call_api('api/save', opts).then(function(res) {
			if (res.ok) {
			  console.log('Saved');
			  let newItem = {'filename': res.path};
			  item[key].push(newItem);
			  item = item;
			  uploading = false;
			} else {
			  console.log('Error saving');
			  setTimeout(function(){
				  uploading = false;
			  }, 1000)
	
			}
		  });
		  */
	
		  // cleaning up
		  URL.revokeObjectURL(img.src)
	
		}
		img.src = URL.createObjectURL(e.target.files[0]);
	
	
	  })
	
	});
	
	function clickSelect(mykey){
	  document.querySelector('#fileInput').click();
	}
</script>


<input type="file" id="fileInput" class="fileInput" accept="image/*" data-name="{key}" />
<button class="btn btn-outline-secondary mb-3" on:click="{() => clickSelect(key)}"><i class="fas fa-image"></i> &nbsp;Upload Image</button>
<br>
	
<style>
	.fileInput{
		display: none;
	}
</style>