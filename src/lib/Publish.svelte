<script>
	//import { auth, googleProvider} from './firebase';
	export let action;
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
			
			console.log("result: "+result)
			
		setTimeout(()=>{	
			loading = false;
			done = true;
			
		}, 1000)
		
		
	}
	
</script>


<div class="backdrop">
	
	
	<div class="modal" style="display: block;">
	  <div class="modal-background"></div>
	  <div class="modal-card">
		<header class="modal-card-head">
		  <p class="modal-card-title">Publish your page</p>
		  <button class="delete" aria-label="close" on:click={()=>action=false}></button>
		</header>
		<section class="modal-card-body">
		  {#if loading}
			Publishing...
			
			{:else if !done}
			Publish your page
			
			{/if}
			
			{#if done}
			Your site has been published!
			{/if}
		</section>
		<footer class="modal-card-foot">
		  <button class="button is-success" on:click={doPublish}>
			
			{#if loading}
		  <i class="fas fa-spinner fa-spin"></i> 
			{:else}
			<i class="fas fa-rocket"></i> 
			{/if}
			
			&nbsp;Publish</button>
		</footer>
	  </div>
	</div>

</div>