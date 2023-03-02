<svelte:head>
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css"
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
  
  function deleteItem(id) {
    let r = confirm('Are you sure you want to delete this item?');
    if (r == true) {
      data.items.splice(data.items.findIndex(x => x.id === id), 1);
      data = data;
      
      setTimeout(()=>{
        makeEditable()
      }, 50)
      
      action = false;
      
      
    
    }
  }
  
  function moveItem(id) {
    var from = data.items.findIndex(x => x.id == id);
    var to = from + 1;
    console.log(from);
    console.log(to);
    var f = data.items.splice(from, 1)[0];
    data.items.splice(to, 0, f);
    data = data;
  }
  
</script> 



  

 
{#if action=='edit'}

<div class="wdgt content-editor" in:fly="{{ x: 350, duration: 500 }}" out:fly="{{ x: 350, duration: 150 }}">
  
  <h3 class="fl-left">Edit</h3>
  
 <span class="close" on:click={closeEditor}>&times;</span>

<div class="clear"></div>

  {#each Object.keys(curComponent.fields) as mykey}
  
  <div class="label">{mykey.replace('_', ' ')}</div>


  {#if curComponent.fields[mykey] == 'txt'}

    <input type="text" class="input" bind:value={data.items[curIndex][mykey]} />
  {/if}
  
  
  {#if curComponent.fields[mykey] == 'rte'}
    {#key data.items[curIndex].id}
    
    
    <TipTap bind:item={data.items[curIndex]} bind:key={mykey} />
    {/key}
  {/if}
  
  {#if curComponent.fields[mykey] == 'txta'}
  
    
    
    <textarea class="input" bind:value={data.items[curIndex][mykey]}></textarea>
   
  {/if}

  
  {#if curComponent.fields[mykey] == 'img'}
    
    <Image bind:item={data.items[curIndex]} bind:key={mykey} />
  {/if}
  
  {#if curComponent.fields[mykey] == 'icn'}
 
  <IconPicker bind:item={data.items[curIndex]} bind:key={mykey} />
  {/if}
  
  {/each}
  
  <div class="label">Options</div>
  

  <button class="button" on:click={()=>moveItem(data.items[curIndex].id)}><i class="fas fa-arrow-down"></i> &nbsp;Move Down</button>
  
  <button class="button is-danger" on:click={()=>deleteItem(data.items[curIndex].id)}><i class="fas fa-trash"></i> &nbsp;Delete</button>

  
  <div class="clear mb-4"></div>
  
  <button class="button is-link" on:click={closeEditor}>Save</button>
  
  
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
    
    <h3 class="fl-left">Design</h3>
    
   <span class="close fl-right mb-15" on:click={() => action = false}>&times;</span>
  
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
    
    overflow: hidden;
    overflow-y: auto;
  }

  
  .editor-start{
    right: auto;
    left: 0;
    border-right: 1px solid #ced4da;
    border-left: 0;
    width: 300px;
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



