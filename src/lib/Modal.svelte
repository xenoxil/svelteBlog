<script>
  export let mode
  export let open
  export let currentPost
  export let saveFunction
  export let closeFunction

  $: post = mode==='edit' ? structuredClone(currentPost) : {title:'',creationDate:'',editDate:'',content:''};

  $: if (currentPost?.creationDate || currentPost?.editDate || currentPost?.title || currentPost?.content || mode) {
    const titleInput = document.querySelector('.modal__titleInput');
    const textInput = document.querySelector('.modal__textInput');
    if(titleInput && textInput)    
    {titleInput.value = post ? post?.title : '';
    textInput.textContent = post ? post?.content : '';}
   
} 
 </script>
 
 <section class={`modal ${open ? "modal_open" : null}`}>
  <form class="modalCard">
  <div>
    <p>Заголовок</p><input on:change={(e)=>{post.title = e.target.value}} class="modal__titleInput" type="text"/>
  </div>
  <div>
    <p>Текст поста</p><div on:input={(e)=>{
      post.content = e.target.textContent}} class="modal__textInput" contenteditable/>
  </div>
  {#if mode==='edit'}
  <div class="postCard__footer">
    <div class="postCard__footerItem">Дата создания:<p class="postCard__footerDates">{currentPost?.creationDate}</p></div>
    {#if currentPost.editDate}
    <div class="postCard__footerItem">Дата редактирования:<p class="postCard__footerDates">{currentPost?.editDate}</p></div>
    {/if}
  </div>
  {/if}
  <div class="modal__btnGrp">
    <button type="reset" on:click={()=>{closeFunction()}}>Закрыть</button>
    <button type="submit" on:click={(evt)=>{
      evt.preventDefault();
      saveFunction(post)}}>Сохранить</button></div>
</form>
 </section>
 
 <style>
  button{
    background-color: green;
  }
   .modal{
    padding:25px;
     position: absolute;
     width:100%;
     min-height:100%;
     top:0;
     left:0;
     display:none;
     background-color: white;
     border-radius:10px;
     border:0.5px solid black;
     transition: opacity 0.3s linear 2s;
     z-index: 1;
     
   }
  .modal_open{
    display:block;
    transition: display 0.3s linear 2s;
    z-index: 3; 
  }
  .modal__textInput{
    max-width: 70% ;
    margin: 0 auto 15px;
    overflow: auto;
    min-height:300px;
    border-radius: 10px;
    border: 1px solid lightgrey; 

  }
  .modal__titleInput{
    width: 50% ;
    min-height:20px;
    border-radius: 10px;
    border: 1px solid lightgrey; 
  }

   .postCard__footer{
     display: flex;
     flex-direction: row;
     justify-content: space-between;
     width:100%;
   }
   .postCard__footerItem{
     display: flex;    
   }
   .postCard__footerDates{
     margin:0
   }
   .postCard__editBtn{
     border:0;
     background-color: wheat;
     border-radius:5px
   }
   .modal__btnGrp{
    display: flex;
    gap:15px;
    justify-content: center;
    margin-top:25px;
   }
   
 </style>
 