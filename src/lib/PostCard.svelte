<script>
 export let title
 export let creationDate
 export let editDate
 export let onEditClick
 export let onViewClick
</script>

<div class="postCard">
  <h3 >{title}</h3>
  <div class="postCard__footerRow">
    <div class="postCard__footerItem">Дата создания:<p class="postCard__footerDates">{creationDate}</p></div>
    {#if editDate}
    <div class="postCard__footerItem">Дата редактирования:<p class="postCard__footerDates">{editDate}</p></div>
    {/if}
  </div>
  <div class="postCard__footerRow">
    <button class="postCard__btn" on:click={onEditClick}>Редактировать</button>
    <button class="postCard__btn postCard__btn_view" on:click={onViewClick}>Просмотр</button>
  </div>
  
</div>

<style>
  .postCard{
    padding:10px;
    border-radius: 10px;
    margin-bottom: 10px;
    box-shadow: 3px 1px 3px 2px grey;
  }
  .postCard__footerRow{
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
  .postCard__btn{
    border:0;
    background-color: wheat;
    border-radius:5px
  }
  
  .postCard__btn_view{
    background-color: lightblue;
  }
</style>
