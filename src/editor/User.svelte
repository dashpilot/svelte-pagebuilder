<script>
  import { onMount } from 'svelte';
  import { auth, googleProvider} from './firebase';
  
  export let user;
  export let showAccount;
  
  async function signIn() {
	auth.signInWithPopup(googleProvider).then(function(myuser) {
	  let handle = myuser.additionalUserInfo.username;
	  localStorage.setItem('handle', handle);
	  user = myuser;
	  console.log(user)
	}, function(e) {
	  console.log(e);
	});
  }
  
  async function signOut() {
	  auth.signOut().then(function() {
		console.log('Signed Out');
		user = false;
		showAccount = false;
	  }, function(error) {
		console.error('Sign Out Error', error);
	  });
  }

 onMount(async () => { 
 	auth.onAuthStateChanged(myuser => {
	// if user is not logged in the auth will be null
		if (myuser) {
	  	
	  	user = myuser;
   	
	  	console.log(user)
	  	console.log('logged in');
	  	// getData('posts');
		} else {
	  	console.log('not logged in');
		}
	});
});
  
  
</script>


   
{#if user}

{#if showAccount}
<div class="backdrop">
	
	<div class="modal" style="display:block;">
	  <div class="modal-dialog">
		<div class="modal-content">
		  <div class="modal-header pt-4">
			<h5 class="modal-title">Your Account</h5>
			<button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" on:click="{() => showAccount = false}"></button>
		  </div>
		  <div class="modal-body">
			  Logged in as <b>{user.email}</b>
		  </div>
		  <div class="modal-footer">
			  
			  <button class="btn btn-dark" on:click={signOut}><i class="fas fa-sign-out-alt"></i> &nbsp;Sign Out</button>
			  
		  </div>
		</div>
		
	  </div>
	</div>
</div>
{/if}

{:else}

<div class="card m-auto mt-5" style="width: 18rem; color: #222">
 
  <div class="card-body text-center">
	<h3 class="card-title">Sign In</h3>


	<button class="btn btn-dark w-100 mt-3" on:click={signIn}><i class="fab fa-google"></i> &nbsp;Sign in with Google</button>


  </div>
</div>


{/if}
