<script setup>
import { ref } from "vue"

const showModal = ref(false)
const newNote = ref("")
const notes = ref([])
const errorMessage = ref("")
const helpMessage = ref("Click ➕ to add a new note. Click ❌ on a note to delete it. Click ❌ button to delete all of the notes. Happy noting!")

const getRandomLightColor = () => {
  return `hsl(${Math.random() * 360} , 90%, 75%)`
}

const addNote = () => {
  if (newNote.value.length > 9) {
    notes.value.push({
      id: Math.floor(Math.random() * 1000000000),
      text: newNote.value,
      date: new Date(),
      backgroundColor: getRandomLightColor()
    })
    showModal.value = false
    newNote.value = ""
    errorMessage.value = ""
  } else {
    errorMessage.value = "Your note is too short!"
  }
}

const addFirstNote = () => {
  notes.value.push({
    id: Math.floor(Math.random() * 1000000000),
    text: helpMessage,
    date: new Date(),
    backgroundColor: getRandomLightColor()
  })
};
</script>

<template>
  <main>
    <div class="overlay" v-if="showModal">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="30"></textarea>
        <p class="error-message">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button @click="showModal = false, errorMessage = '', newNote = ''">Close</button>
      </div>
    </div>
    <div class="container">
      <h1>Easy Notes</h1>
      <button id="add-btn" @click="showModal = true">➕</button>
      <button id="delete-btn" @click="notes = []">❌</button>
      <button id="help-btn" @click="addFirstNote">❓</button>
      <div class="cards-container">
        <div :key="note.id" v-for="note in notes" class="card" :style="{ backgroundColor: note.backgroundColor }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString() }}</p>
          <p class="delete-card" @click="notes = notes.filter(n => note.id !== n.id)">❌</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  width: 100vw;
  height: 100vh;
}

.container {
  display: grid;
  place-items: center;
  grid-template-rows: 18vh 1fr;
  width: 100vw;
  height: 100vh;
}

h1 {
  font-weight: 700;
  margin-bottom: 25px;
  font-size: 8vmin;
  text-align: center;
  justify-self: left;
  margin-left: 10vmin;
}

#add-btn {
  border: none;
  padding: 10px;
  width: 10vmin;
  height: 10vmin;
  cursor: pointer;
  background-color: hsl(0, 0%, 100%);
  border-radius: 50%;
  font-size: 4vmin;
  font-weight: 700;
  position: fixed;
  right: 40vmin;
  top: 3vmin;
  z-index: 1;

}

#delete-btn {
  border: none;
  padding: 10px;
  width: 10vmin;
  height: 10vmin;
  cursor: pointer;
  background-color: hsl(0, 0%, 100%);
  border-radius: 50%;
  font-size: 4vmin;
  font-weight: 700;
  position: fixed;
  right: 10vmin;
  top: 3vmin;
  z-index: 1;
}

#help-btn {
  border: none;
  padding: 10px;
  width: 10vmin;
  height: 10vmin;
  cursor: pointer;
  background-color: hsl(0, 0%, 100%);
  border-radius: 50%;
  font-size: 4vmin;
  font-weight: 700;
  position: fixed;
  right: 25vmin;
  top: 3vmin;
  z-index: 1;
}

.card {
  width: 30vmin;
  height: 30vmin;
  background-color: aqua;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 1vmin;
  padding: 1vmin;
  color: #000;
  font-size: 2vmin;
  align-self: baseline;
}

.delete-card {
  position: absolute;
  bottom: 1vmin;
  right: 1vmin;
  cursor: pointer;
  font-size: 2.5vmin;
}

.date {
  font-size: 2vmin;
  font-style: italic;
}

.error-message {
  font-size: 3vmin;
  color: hsl(0, 100%, 35%);
  text-align: center;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  width: 100vw;
  min-height: 80vh;
  align-self: baseline;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: hsla(0, 0%, 100%, 0.4);
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 35vw;
  height: 35vw;
  border-radius: 15px;
  padding: 3vmin;
  position: relative;
  display: flex;
  flex-direction: column;
  background-color: hsl(0, 0%, 60%);
}

.modal textarea {
  font-size: 4vmin;
  background-color: hsl(0, 0%, 40%);
  color: #fff;
  border-radius: 15px;
}

.modal button {
  font-size: 4vmin;
  background-color: hsl(0, 0%, 20%);
  color: #fff;
  border: none;
  cursor: pointer;
  margin-top: 2vmin;
  border-radius: 15px;
}

@media (max-width: 500px) {
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  h1 {
    font-size: 12vmin;
    margin-left: 0;
    margin-bottom: 20vmin;
  }

  #add-btn {
    top: 20vmin;
    right: 64vmin;
    width: 15vmin;
    height: 15vmin;
    font-size: 6vmin;
  }

  #delete-btn {
    top: 20vmin;
    right: 20vmin;
    width: 15vmin;
    height: 15vmin;
    font-size: 6vmin;
  }

  #help-btn {
    top: 20vmin;
    right: 42vmin;
    width: 15vmin;
    height: 15vmin;
    font-size: 6vmin;
  }

  .card {
    width: 60vmin;
    height: 60vmin;
    font-size: 4vmin;
  }

  .date {
    font-size: 4vmin;
  }

  .delete-card {
    font-size: 5vmin;
  }

  .modal {
    width: 90vw;
    height: 90vw;
  }

  .modal textarea {
    font-size: 6vmin;
  }

  .error-message,
  .modal button {
    font-size: 5vmin;
  }
}
</style>