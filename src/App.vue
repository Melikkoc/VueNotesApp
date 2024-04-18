<script setup>
import { ref } from 'vue';

const showModal = ref(false);
// two way binding newNote
const newNote = ref('');
const errorMessage = ref('');
const notes = ref([]);

function getRandomColor() {
  return 'hsl(' + Math.random() * 360 + ', 100%, 75%)';
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = 'Note needs to be 10 characters or more');
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });
  showModal.value = false;
  newNote.value = '';
  errorMessage.value = '';
};
</script>
<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString('en-GB') }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
  font-family: Arial, Helvetica, sans-serif;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 65px;
  margin-left: 5px;
  text-decoration: underline;
}
header button {
  border: none;
  padding: 10px;
  margin-right: 5px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: hsl(0, 0%, 0%);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}
button:hover {
  background-color: hsl(0, 0%, 20%);
}
.card {
  width: 225px;
  height: 225px;
  background-color: hsl(43, 84%, 55%);
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
  background-color: hsla(0, 0%, 0%, 0.77);
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
textarea {
  font-size: 15px;
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
  background-color: hsl(0, 100%, 44%);
  margin-top: 10px;
}
.modal p {
  color: hsl(0, 100%, 44%);
}
</style>
