<script>
	import { onMount } from 'svelte';
	export let item;
	
	onMount(()=>{
		if(item.title == 'Lorem ipsum'){
			item.title = "This is some text.";
		}
		

		
		const flip_btn = document.querySelectorAll('.flip-btn')[0];
		 
		  flip_btn.addEventListener( 'click', function() {
		
			flip_btn.closest('.card-body').classList.add('is-flipped');
			
		  });
		  
		  const back_btn = document.querySelectorAll('.back-btn')[0];
		   
			back_btn.addEventListener( 'click', function() {
			
			  back_btn.closest('.card-body').classList.remove('is-flipped');
			  
			});
	})
</script>

<section id="{item.id}" class="edit landing-card">
	

<div class="card-body">
		
		<div class="w-100 card-front d-flex align-items-center text-center card__face" style="display: block;">
			
			<div class="w-100">
			<i class="fas fa-address-card fa-large"></i>
			
		<h2>{item.title}</h2>
		
		{@html item.body}
		
		<button class="btn btn-dark flip-btn">Read More</button>
			</div>
		
		</div>
		
		
		<div class="w-100 card-back card__face">
			<h2>Welcome to the back!</h2>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed ut justo nec ipsum cursus laoreet sed non massa. Sed id sem in lectus hendrerit vulputate ut sit amet nulla. Suspendisse pulvinar velit sed feugiat sollicitudin. Sed nec vehicula neque. Sed semper, justo ac suscipit rutrum, diam enim varius felis, eget efficitur arcu metus id lectus. Sed mollis vehicula magna, sit amet posuere lectus. Nulla dapibus vel ipsum id fringilla. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Morbi at nunc gravida sem cursus lacinia. Mauris sem eros, ultrices ornare magna non, consectetur placerat lorem. Nam gravida metus justo, nec semper urna tristique ut. Mauris lectus tortor, vehicula in porta id, luctus ac ex.</p>
		
		<button class="btn btn-dark back-btn">Back</button>
		</div>
</div>

	
</section>


