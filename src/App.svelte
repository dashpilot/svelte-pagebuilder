<svelte:head>
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css"
/>
<link
  rel="stylesheet"
  href="bulma-switch.css"
/>
<script src="https://ajax.googleapis.com/ajax/libs/webfont/1.6.26/webfont.js"></script>
</svelte:head>

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
  
  let curTab = 'content';
  
  let wideEditor;

  
  // send the data
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
        let index = data.posts.findIndex(x => x.id == id);
        
        curIndex = index;
       
        
        let mycomponent = data.posts[curIndex].component;
        
        curComponent = components.filter(x => x.component == mycomponent)[0]
        
        console.log(curComponent)
        
        action = 'edit'
        curTab = 'content'
        
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
    
    data.posts.unshift(newItem);
    
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
  
  function deleteItem(id) {
    let r = confirm('Are you sure you want to delete this item?');
    if (r == true) {
      data.posts.splice(data.posts.findIndex(x => x.id === id), 1);
      data = data;
      
      setTimeout(()=>{
        makeEditable()
      }, 50)
      
      action = false;
 
    }
  }
  
</script> 


  

 
{#if action=='edit'}

<div class="wdgt content-editor" class:editor-wide={wideEditor} in:fly="{{ x: 350, duration: 500 }}" out:fly="{{ x: 350, duration: 150 }}">
  
 <span class="close" on:click={closeEditor}>&times;</span>
  
  
 <div class="tabs">
   <ul>
     <li class:is-active={curTab=='content'} on:click={()=>curTab='content'}><a>Content</a></li>
     <li  class:is-active={curTab=='options'} on:click={()=>curTab='options'}><a>Options</a></li>
   </ul>
 </div>
  
  
  <!--
  <h3 class="fl-left">Edit</h3>
  

 
 -->
<div class="wdgt-body">

<div class="clear"></div>

{#if curTab == 'content'}

  {#each Object.keys(curComponent.fields) as mykey}
  
  <div class="label">{mykey.replace('_', ' ')}</div>


  {#if curComponent.fields[mykey] == 'txt'}

    <input type="text" class="input" bind:value={data.posts[curIndex][mykey]} />
  {/if}
  
  
  {#if curComponent.fields[mykey] == 'rte'}
    {#key data.posts[curIndex].id}
    
    
    <TipTap bind:item={data.posts[curIndex]} bind:key={mykey} />
    {/key}
  {/if}
  
  {#if curComponent.fields[mykey] == 'txta'}
  
    
    
    <textarea class="input" bind:value={data.posts[curIndex][mykey]}></textarea>
   
  {/if}

  
  {#if curComponent.fields[mykey] == 'img'}
    
    <Image bind:item={data.posts[curIndex]} bind:key={mykey} />
  {/if}
  
  {#if curComponent.fields[mykey] == 'icn'}
 
  <IconPicker bind:item={data.posts[curIndex]} bind:key={mykey} />
  {/if}
  
  {/each}
  
  {:else if curTab=='options'}
  
  
  <div class="label">Expand Editor</div>
  <div class="field">
    <label class="switch">
    <input type="checkbox" bind:checked={wideEditor}>
    <span class="check is-success"></span>
    <span class="control-label">expand</span>
    </label>
  </div>
  
  
  
  <div class="label">Delete</div>
  <button class="button" on:click={()=>deleteItem(data.posts[curIndex].id)}><i class="fas fa-trash"></i> &nbsp;Delete</button>
  
  {/if}


    

</div>

<div class="wdgt-footer">
  <button class="button is-link btn-save" on:click={closeEditor}>Save</button>
</div>
  

</div>  


  
{/if}


{#if action=='add'}

<div class="wdgt content-editor editor-start" in:fly="{{ x: -350, duration: 500 }}" out:fly="{{ x: -350, duration: 150 }}">
  
  <h3 class="fl-left">Add Content</h3>
  
 <span class="close mb-10" on:click={() => action = false}>&times;</span>

<div class="clear"></div>

{#each components as component}

<div class="box" on:click={() => add(component)}>
  
  {component.name}
  
</div>

{/each}
  
</div>  
{/if}


{#if action=='design'}
  <div class="wdgt content-editor editor-start" in:fly="{{ x: -350, duration: 500 }}" out:fly="{{ x: -350, duration: 150 }}">
    
 <span class="close" on:click={()=>action=false}>&times;</span>
    

  
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



<img src="img/settings.png" class="grow" on:click={()=>action='design'} />

<img src="img/plus.png" class="grow" on:click={()=>action='add'} />

<img src="img/publish.png" class="grow" on:click={()=>action='publish'} />

</div>


{#if wideEditor && action=='edit'}
<div class="backdrop"></div>
{/if}


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
    z-index: 99999999999;
    
    font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
    font-size: 16px;
    line-height: 24px;
    font-weight: 400;
    font-synthesis: none;
    text-rendering: optimizeLegibility;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    -webkit-text-size-adjust: 100%;
    
    overflow: hidden;
    overflow-y: auto;
    
    /* transition: all 1s; */
  }

  
  .editor-start{
    right: auto;
    left: 0;
    border-right: 1px solid #ced4da;
    border-left: 0;
    width: 400px;
  }
  
  
  .editor-wide {
    width: 800px !important;
    left: calc(50% - 400px);
    border-right: 1px solid #ced4da;
  }

  .editor-wide .wdgt-footer{
    width: 800px !important;
   left: calc(50% - 400px);
   padding-right: 20px;
   text-align: right;
  }

  
  .input{
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



