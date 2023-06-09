<script setup>
  import { ref } from 'vue'

  const showModal = ref(false)
  const newNote = ref('')
  const notes = ref([])
  const errorMessage = ref('')

  function getRandomColor() {
    const color = "hsl(" + Math.random() * 360 + ", 100%, 75%"
    return color
  }

  function getRandomId() {
    const id = Math.floor(Math.random() * 1000000)
    return id
  }

  function closeModal() {
    showModal.value = false
    newNote.value = ''
    errorMessage.value = ''
  }

  function addNote() {
    if (newNote.value.length < 1) {
      return errorMessage.value = 'Note must have at least 1 character'
    }
    notes.value.push({
      text: newNote.value,
      date: new Date(),
      id: getRandomId(),
      backgroundColor: getRandomColor(),
    })
    closeModal()
  }
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay" @click="closeModal">
      <div class="modal" @click.stop=''>
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="closeModal">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button class="circle plus" @click="showModal = true"></button>
      </header>
      <div class="cards-container">
        <div 
          v-for="note in notes" 
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
          :key="note.id" 
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString('en-US') }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {
    height: 100vh;
    width: 100vw;
  }

  .container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  h1 {
    font-weight: bold;

    font-size: 75px;
    color: rgb(255, 255, 255)
  }

  .circle {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(255, 255, 255);
    border-radius: 100%;
    color: rgb(21, 20, 20);
    font-size: 45px;
  }

  .circle:hover{
    background: #ebebebf4;
  }
  .circle:active{
    background: radial-gradient(#aaa, #fff);
  }
  .circle:before,
  .circle:after{
    content:'';position:absolute;top:0;left:0;right:0;bottom:0;
  }

  .circle.plus:before,
  .circle.plus:after {
    background:rgb(0, 0, 0);
  }
  .circle.plus:before{
    width: 2px;
    margin: 3px auto;
  }
  .circle.plus:after{
    margin: auto 3px;
    height: 2px;
    box-shadow: none;
  }

  .card {
    color: black;
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .date {
    font-size: 12.5px;
    font-weight: bold;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  .modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
  }

  .modal .close {
    background-color: rgb(193, 15, 15);
    margin-top: 7px;
  }

  .modal p {
    color: rgb(193, 15, 15);
  }
</style>