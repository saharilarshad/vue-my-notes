<script setup lang="ts">
import { ref } from "vue"

const showModal = ref(false)
const newNote = ref("")
const errorMessage = ref("")
const notes = ref([])

const getNotesLocalStorage = JSON.parse(localStorage.getItem('notes' || []))

const randomHsl = () => `hsla(${Math.random() * 360}, 100%, 70%, 1)`

const addNote = () => {
  if (newNote.value.length < 10) return errorMessage.value = "you must have more at least 10 character"

   notes.value.push({
    id: Math.floor(Math.random() * 10000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: randomHsl()
  })

  localStorage.setItem('notes',JSON.stringify(notes.value))

  showModal.value = false
  newNote.value = ""
}
</script>

<template>
  <main>
    <div class="overlay" v-if="showModal">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button class="btn-addNote" @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">X</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>My-Notes</h1>
        <button class="btn-add" @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div class="card" v-for="note in getNotesLocalStorage" :key="note.id" :style="{ backgroundColor: note.backgroundColor }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleString() }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
  /* background: slategray; */
  /* overflow-x: none; */
  position: relative;
}

.container {
  height: 100%;
  width: 50rem;
  background: white;
  display: flex;
  flex-direction: column;
  margin: auto;
}

header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 3rem;
}

h1 {
  color: #42b883;
}

.btn-add {
  height: 2rem;
  width: 2rem;
  border-radius: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  outline: none;
  color: white;
  background: #42b883;
  font-size: 2rem;

  border: none;
  cursor: pointer;

}

.cards-container {
  height: 100%;
  width: 100%;
  background: #dbdada;
  padding: 2rem;
  border-radius: 1rem;
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.card {
  position: relative;
  display: flex;
  flex-direction: column;
  background: rgb(221, 203, 203);
  width: 10rem;
  height: 10rem;
  border-radius: 1rem;
  padding: 1rem;
}

.date {
  position: absolute;
  bottom: 1rem;
}

.overlay {
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgb(0, 0, 0, 0.77);
  height: 100%;
  width: 100%;
  z-index: 10;
}

.modal {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 10rem;
  width: 40rem;
  border-radius: 1rem;
  background-color: white;
  padding: 3rem;
  position: relative;
}

textarea {
  /* display: flex;
  align-items: center;
  justify-content: center; */
  width: 100%;
  border-radius: 1rem;
  padding: 1rem;
}

.btn-addNote {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  border-radius: 1rem;
  border: none;
  background-color: #42b883;
  color: white;
  padding: 5px;
  cursor: pointer;

}

.close {
  position: absolute;
  right: 1rem;
  top: 1rem;
  cursor: pointer;
  background-color: rgb(226, 42, 42);
  color: white;
  border: none;
}

.modal p {
  color: red;
  margin: 1rem 0;
}
</style>