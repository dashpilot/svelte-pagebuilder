<script>
	import { auth, googleProvider} from './firebase';
	export let showPublish;
	export let data;
	
	let loading = false;
	let done = false;
	
	function setData(service, path, type, content) {
	  let opts = {};
	  opts.path = path;
	  opts.type = type;
	  opts.content = content;
	  call_api(service + '/set-data', opts).then(function(res) {
		if (res.ok) {
		  console.log(res.msg);
		} else {
		  console.log('An error occured' + res);
		}
	  });
	}
	
	function getData(service, path) {
	  let opts = {};
	  opts.path = path;
	  call_api(service + '/get-data', opts).then(function(res) {
		if (res.ok) {
		  console.log(res.msg);
		} else {
		  console.log('An error occured: ' + res);
		}
	  });
	}
	
	async function call_api(route, mydata) {
	
	  try {
		const idToken = await auth.currentUser.getIdToken(true);
	
		var settings = {
		  method: 'post',
		  body: JSON.stringify(mydata),
		  headers: {
			'Authorization': idToken,
			'Content-Type': 'application/json'
		  }
		};
		try {
		  const fetchResponse = await fetch('/api/' + route, settings);
		  const result = await fetchResponse.json();
		  return result;
		} catch (e) {
		  return e;
		}
	
	  } catch (e) {
		console.log("Not signed in");
		return "User is not signed in.";
	  }
	
	}
	
	async function doPublish(){
		
		loading = true;
		
		let result = await setData("s3", "data.json", "json", data);
		
		console.log(result)
		
		loading = false;
		done = true;
	}
	
</script>


<div class="backdrop">
	
	<div class="modal" style="display:block;">
	  <div class="modal-dialog">
		<div class="modal-content">
		  <div class="modal-header pt-4">
			<h5 class="modal-title">Publish your page</h5>
			<button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" on:click="{() => showPublish = false}"></button>
		  </div>
		  <div class="modal-body">
			  
			  {#if loading}
			  Publishing...
			  
			  {:else if !done}
			  Publish your page
			  
			  {/if}
			  
			  {#if done}
			  Your site has been published!
			  {/if}
			  
			 
		  </div>
		  <div class="modal-footer">
			  
			  <button class="btn btn-dark" on:click={doPublish}>
				  
				  {#if loading}
				<i class="fas fa-spinner fa-spin"></i> 
				  {:else}
				  <i class="fas fa-rocket"></i> 
				  {/if}
				  
				  &nbsp;Publish</button>
			  
		  </div>
		</div>
		
	  </div>
	</div>
</div>
