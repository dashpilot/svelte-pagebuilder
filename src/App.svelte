<script>
  import { onMount } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  
  import Design from "./lib/Design.svelte"
  import Publish from "./lib/Publish.svelte"
  
  import TipTap from "./widgets/TipTap.svelte"
  import Image from "./widgets/Image.svelte"
  import IconPicker from "./widgets/IconPicker.svelte"
  
  let data;
  let components;

  let action = false;
 
  
  let curIndex = false;
  let curComponent = false;
  
  let showPublish = false;
  
  // send the 
  $: data && window.relay(data);
  
  onMount(async () => {
  
    /*
    const response = await fetch(cfg.dataPath);
    data = await response.json();
    */
    
    setTimeout(()=>{
      makeEditable()
      
      // get the components from the SPA
      components = window.components;
      data = window.data;
    }, 250)
    
  });
  
  function makeEditable(){
    document.querySelectorAll('.edit').forEach((el)=>{
      
      el.addEventListener('click', (e)=>{
        
        
        if(!e.originalTarget.classList.contains('btn')){
        
        document.querySelectorAll('.editing').forEach((myel)=>{
          myel.classList.remove('editing');
        })
        e.originalTarget.closest('.edit').classList.add('editing');
        
        
        let id = e.originalTarget.closest('.edit').getAttribute('id');
        let index = data.items.findIndex(x => x.id == id);
        
        curIndex = index;
       
        
        let mycomponent = data.items[curIndex].component;
        
        curComponent = components.filter(x => x.component == mycomponent)[0]
        
        console.log(curComponent)
        
        action = 'edit'
        
        }
    
      })
      
    })
  }
  

  function add(component){
    
    
    let newItem = {};
    
    newItem.id = "item-"+Date.now();
    newItem.component = component.component;
    Object.keys(component.fields).forEach((key)=>{
      
      if(component.fields[key]=='txt'){
        newItem[key] = "Lorem Ipsum"
      }
      if(component.fields[key]=='rte' || component.fields[key]=='txta'){
        newItem[key] = "Lorem ipsum dolor site amet"
      }
      if(component.fields[key]=='img'){
        newItem[key] = "img/placeholder.jpg"
      }
      if(component.fields[key]=='icn'){
        newItem[key] = "fas fa-bolt"
      }
      
    })
    
    console.log(newItem)
    
    data.items.unshift(newItem);
    
    data = data;
    
    action = false;
    
    setTimeout(()=>{
      makeEditable()
    }, 50)
    
    
  }  
  
  function closeEditor(){
    document.querySelectorAll('.editing').forEach((myel)=>{
        myel.classList.remove('editing');
      })
      
    action = false
  }
  
  
</script> 



  

 
{#if action=='edit'}

<div class="content-editor" in:fly="{{ x: 350, duration: 500 }}" out:fly="{{ x: 350, duration: 150 }}">
  
  <h5 class="float-start">Edit</h5>
  
 <button type="button" class="btn-close float-end mb-3" aria-label="Close" on:click={closeEditor}></button>

<div class="clear"></div>

  {#each Object.keys(curComponent.fields) as mykey}
  
  <div class="label">{mykey.replace('_', ' ')}</div>


  {#if curComponent.fields[mykey] == 'txt'}

    <input type="text" class="form-control" bind:value={data.items[curIndex][mykey]} />
  {/if}
  
  
  {#if curComponent.fields[mykey] == 'rte'}
    {#key data.items[curIndex].id}
    
    
    <TipTap bind:item={data.items[curIndex]} bind:key={mykey} />
    {/key}
  {/if}
  
  {#if curComponent.fields[mykey] == 'txta'}
  
    
    
    <textarea class="form-control" bind:value={data.items[curIndex][mykey]}></textarea>
   
  {/if}

  
  {#if curComponent.fields[mykey] == 'img'}
    
    <Image bind:item={data.items[curIndex]} bind:key={mykey} />
  {/if}
  
  {#if curComponent.fields[mykey] == 'icn'}
 
  <IconPicker bind:item={data.items[curIndex]} bind:key={mykey} />
  {/if}
  
  {/each}
  
  <button class="btn btn-dark" on:click={closeEditor}>Save</button>
  
  
</div>  
{/if}


{#if action=='add'}

<div class="content-editor editor-start" in:fly="{{ x: -350, duration: 500 }}" out:fly="{{ x: -350, duration: 150 }}">
  
  <h5 class="float-start">Add Content</h5>
  
 <button type="button" class="btn-close float-end mb-3" aria-label="Close" on:click={() => action = false}></button>

<div class="clear"></div>

{#each components as component}

<div class="box" on:click={() => add(component)}>
  
  {component.name}
  
</div>

{/each}
  
</div>  
{/if}


{#if action=='design'}
  <div class="content-editor editor-start" in:fly="{{ x: -350, duration: 500 }}" out:fly="{{ x: -350, duration: 150 }}">
    
    <h5 class="float-start">Design</h5>
    
   <button type="button" class="btn-close float-end mb-3" aria-label="Close" on:click={() => action = false}></button>
  
  <div class="clear"></div>
  
  <Design bind:data />
    
  </div>  
{/if}



{#if action=='publish'}
<Publish bind:action bind:data />
{/if}



<div class="dock">
  
<!--
<img src="img/account.png" class="grow" on:click={() => showAccount = true} />
-->

<img src="img/plus.png" class="grow" on:click={()=>action='add'} />

<img src="img/settings.png" class="grow" on:click={()=>action='design'} />

<img src="img/publish.png" class="grow" on:click={()=>action='publish'} />

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
    border-right: 1px solid #ced4da;
    border-left: 0;
  }
  
 
  
  .form-control{
    margin-bottom: 15px;
  }
  
  textarea{
    resize: none;
    height: 100px;
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
    width: 55px;
  }

  
  .grow { transition: all .2s ease-in-out; }
  .grow:hover { transform: scale(1.1); cursor: pointer; }
</style>



