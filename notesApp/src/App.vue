<script setup>
  import {ref} from "vue";

  const modalIsOn = ref(false);

  function toggleModal() {
    if(modalIsOn.value == false) modalIsOn.value = true;
    else modalIsOn.value = false;
  }

  const newNote = ref({
    // this stays an empty, undefined set. specifying properties happens downstream
    });

  const notes = ref([]);
  const errorMessage = ref("");
  
  function addNote() {
    if(checkLength(newNote.value.title, 4) && checkLength(newNote.value.body, 10)){
      notes.value.push({
        id: getNextId(),
        content: newNote.value,
        date: new Date(),
        color: getRandomColor()
      })
      toggleModal();
      newNote.value = {
      title:"", 
      body:"", 
      };
    }
    else if (!checkLength(newNote.value.title, 4)){
      errorMessage.value = "Title needs to be atleast 4 characters";
    }
    else if (!checkLength(newNote.value.body, 10)){
      errorMessage.value = "Body needs to be atleast 10 characters";
    }
  }

  function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  }
  
  function getNextId() {
    var biggest = 0;
    notes.value.forEach(note => {
      if(note.id > biggest) biggest = note.id;
    });
    return biggest+1;
  }
  function checkLength(object, lim){
    return (object.length >= lim)
  }

</script>
<!-- =========================================================================================================================================== -->
<template>
  <main>
    <div class="overlay" v-if="modalIsOn">
      <div class="modal">
        <div class="title">
          <label for="Title">Title: </label>
          <textarea v-model.trim="newNote.title" name="Title" id="title" cols="35" rows="1"></textarea>
        </div>
        <textarea v-model.trim="newNote.body" name="Note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage" class="errorMessage">{{ errorMessage }}</p>
        <div class="buttons">
          <button @click="addNote()">Add Note</button>
          <button @click="toggleModal()">Close</button>
        </div>
      </div>
    </div>

    <div class="container">
    <header>
      <h1>Notes</h1>
      <button class="plus" @click="toggleModal()">+</button>
    </header>
    <div class="card-container">
      <div class="card" v-for="note in notes" :style="{backgroundColor: note.color}" :key="note.id">
        <p class="card-title">{{note.content.title}}</p>
        <p class="card-body">{{note.content.body}}</p>
        <p class="card-date">{{note.date.toLocaleDateString("en-US").replace(/\//g,'-')}}</p>
      </div>
    </div>
    </div>
    
  </main>
</template>
<!-- =========================================================================================================================================== -->
<style scoped>
  main{
    width: 100vw;
    height: 100vh;
  }
 
  .container{
    max-width: 800px;
    padding: 10px;
    margin: 0 auto;
  }
  header{
    display:flex;
    justify-content: space-between;
    align-items: center;
  }
  h1{
    font-weight: bold;
    margin-bottom: 2rem;
    font-size: 4em;
  }
  .plus{
    border: none;
    border-radius: 100%;

    padding:0;
    width:50px;
    height:50px;
    background-color: #212121;

    cursor: pointer;
    color: #fff;
    font-size: 2em;    
  }
  .card-container{
    display:flex;
    flex-wrap: wrap;
    
  }
  .card{
    width: 225px;
    height: 225px;
    background-color: #decc44;
    padding: 1em;
    margin-right: 20px;
    margin-bottom:20px;

    border-radius: 15px;
    
    display:flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .card-title, .card-body{
    width: 200px;
    word-wrap: break-word;
  }
  .card-title{
    font-weight: bold;
  }

  .card-date{
    text-align: right;
    font-size: .75rem;
  }

  .overlay{
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: #000000C4;
    z-index: 10;

    display:flex;
    align-items: center;
    justify-content: center;
  }
  .modal{
    display: flex;
    flex-direction: column;

    width: 500px;
    background-color: aliceblue;
    padding: 20px;

    border-radius: 10px;
  }
  .modal .title{
    display: flex;
    align-items: left;
    margin-bottom: 15px;
    
    font-size:1.2em;    
  }
  .title textarea{
    margin: 0 10px;
    font-size: 1rem;
    vertical-align: bottom;
    
  }
  .modal .buttons{
    display: flex;
    align-items: center;
    justify-content:center;
  }
  .modal button{
    width: 150px;
    margin: 0 10px;
    margin-top: 15px;
    padding: 10px 20px;
    background-color: #212121;
    border: none;
    cursor: pointer;

    font-size: 1.2em;
    color: white;
  }
  .errorMessage{
    vertical-align:bottom;
    color:white;
    background-color:red;
    padding-left: 10px;
  }


/* @media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
} */
</style>
