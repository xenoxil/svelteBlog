<script>
  import { onMount } from "svelte";
import Modal from "./lib/Modal.svelte";
import PostCard from "./lib/PostCard.svelte";
import dayjs from 'dayjs';

const savedPosts = localStorage.getItem('posts');
  let posts= savedPosts ? JSON.parse(savedPosts) : [
    {title:'Заголовок1',creationDate:'2023-10-01',editDate:'2023-10-02', content:'text1',id:'1'},
  {title:'Заголовок2',creationDate:'2023-09-27',editDate:'',content:'text2',id:'2'},
  {title:'Заголовок3',creationDate:'2023-09-02',editDate:'2023-09-24',content:'text3',id:'3'}
]
  let modalOpen = false;
  let currentPost={};
  let mode='create';
  $: if(posts){
    localStorage.setItem('posts',JSON.stringify(posts));
  }


  function handleSaveClick(postData){    
    if(!(postData.content?.length>3 && postData.title?.length>2)) return alert(`Длины заголовка и текста должны быть больше 2ух символов`)
    if(mode==='create'){
      posts.unshift({title:postData.title,creationDate:`${dayjs(new Date()).format('YYYY-MM-DD')}`,editDate:'',content: postData.content,id:`${new Date}`});
      modalOpen=false;
      posts=posts;         
      return
    }
   
    const currentPost = posts.find(post=>post.id===postData.id);
    currentPost.content = postData.content;
    currentPost.title = postData.title;
    currentPost.editDate = `${dayjs(new Date()).format('YYYY-MM-DD')}`;
    modalOpen=false;
    posts=posts;   
  }

  function onCloseClick(){
      modalOpen=false;
      posts=posts;
    }

  function handleEditClick(post){
    mode='edit';
    currentPost=post;   
    modalOpen=true;        
  }

  function handleCreateClick  (){
    mode='create';   
    currentPost={title:'',creationDate:'',editDate:''};    
    modalOpen=true;   
  }
  function handleViewClick(post){
    mode='view';   
    currentPost=post; 
    modalOpen=true;   
  }
</script>

<main class="main">
  <button class="postCreateBtn" on:click={handleCreateClick}>+</button>
   <section class="postList">
    {#each posts as post}
         <PostCard title={post.title} creationDate={post.creationDate} editDate={post.editDate} currentPost={post} 
         onEditClick={()=>handleEditClick(post)} onViewClick={()=>{handleViewClick(post)}}/>
{/each}
   </section>
   <Modal open={modalOpen} currentPost={currentPost} mode={mode} saveFunction={handleSaveClick} closeFunction={onCloseClick}/>
</main>

<style>
  .main{
    width:100%;
    height:100%;
    position: relative;
    z-index: 2;
  }
  .postList{
    width:50vw;
  }
  .postCreateBtn{
    border:0;
    color:white;
    background-color: green;
    border-radius:5px;
    margin: 0 0 15px 45vw   
  }
  .postCreateBtn:focus{
    outline: 0;
  }
  
</style>
